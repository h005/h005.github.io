---
layout: post
title: Deep photo style transfer
moto: 默默看着时间，带着所有湍急而下
date: 2017-12-26 +0800
categories: document
tag: tensorflow style transfer
---

* content
{:toc}

# Deep photo style transfer code analysis

## photo_style.py file analysis

### function style(args, Matting)

```
def stylize(args, Matting):
# ------------------- GPU 资源分配方式 ----------------
    config = tf.ConfigProto()
    config.gpu_options.allow_growth = True
    sess = tf.Session(config=config)
# ----------- 读入图像并且获得图像的宽和高 --------------
    start = time.time()
    # prepare input images
    content_image = np.array(Image.open(args.content_image_path).convert("RGB"), dtype=np.float32)
    content_width, content_height = content_image.shape[1], content_image.shape[0]
# ------------ Matting Laplacian of Levin ------------
# express a grayscale matte as a locally affine combination of the input RGB channels.
    if Matting:
        M = tf.to_float(getLaplacian(content_image / 255.))
# -------------- 颜色通道转换，并且改变类型 --------------
# --------------------- 内容图像 ----------------------
    content_image = rgb2bgr(content_image)
    content_image = content_image.reshape((1, content_height, content_width, 3)).astype(np.float32)
# --------------------- 风格图像 ----------------------
    style_image = rgb2bgr(np.array(Image.open(args.style_image_path).convert("RGB"), dtype=np.float32))
    style_width, style_height = style_image.shape[1], style_image.shape[0]
    style_image = style_image.reshape((1, style_height, style_width, 3)).astype(np.float32)
# ------------------- 提取分割后的mask -----------------
    content_masks, style_masks = load_seg(args.content_seg_path, args.style_seg_path, [content_width, content_height], [style_width, style_height])
# ------------------- 减去VGG Net的均值 ----------------
    if not args.init_image_path:
        if Matting:
            print("<WARNING>: Apply Matting with random init")
        init_image = np.random.randn(1, content_height, content_width, 3).astype(np.float32) * 0.0001
    else:
        init_image = np.expand_dims(rgb2bgr(np.array(Image.open(args.init_image_path).convert("RGB"), dtype=np.float32)).astype(np.float32), 0)

#	VGG_MEAN = [103.939, 116.779, 123.68]

    mean_pixel = tf.constant(VGG_MEAN)
    input_image = tf.Variable(init_image)

# ----------- 提取VGG19网络中的某几层作为loss ------------
# 此外，此处定义这部分取出的部分为常量

    with tf.name_scope("constant"):
        vgg_const = Vgg19()
        vgg_const.build(tf.constant(content_image), clear_data=False)

# -------------------- content loss --------------------

        content_fv = sess.run(vgg_const.conv4_2)
        content_layer_const = tf.constant(content_fv)

# -------------------- style loss ----------------------

        vgg_const.build(tf.constant(style_image))
        style_layers_const = [vgg_const.conv1_1, vgg_const.conv2_1, vgg_const.conv3_1, vgg_const.conv4_1, vgg_const.conv5_1]
        style_fvs = sess.run(style_layers_const)
        style_layers_const = [tf.constant(fv) for fv in style_fvs]

# 定义输入的图像为变量，也是要优化的部分

    with tf.name_scope("variable"):
        vgg_var = Vgg19()
        vgg_var.build(input_image)

    # which layers we want to use?
    style_layers_var = [vgg_var.conv1_1, vgg_var.conv2_1, vgg_var.conv3_1, vgg_var.conv4_1, vgg_var.conv5_1]
    content_layer_var = vgg_var.conv4_2

    # The whole CNN structure to downsample mask
# ---------- 我理解的这一步只是为了得到网络的所有层 ---------
    layer_structure_all = [layer.name for layer in vgg_var.get_all_layers()]

# ------------------- 计算内容损失函数 -------------------

    # Content Loss
    loss_content = content_loss(content_layer_const, content_layer_var, float(args.content_weight))

# ------------------- 计算风格损失函数 -------------------

    # Style Loss
    loss_styles_list = style_loss(layer_structure_all, style_layers_const, style_layers_var, content_masks, style_masks, float(args.style_weight))
    loss_style = 0.0
    for loss in loss_styles_list:
        loss_style += loss

    input_image_plus = tf.squeeze(input_image + mean_pixel, [0])

# ------------------ 计算Affine损失函数 -----------------

    # Affine Loss
    if Matting:
        loss_affine = affine_loss(input_image_plus, M, args.affine_weight)
    else:
        loss_affine = tf.constant(0.00001)  # junk value

# ------------------ 计算total损失函数 ------------------

    # Total Variational Loss
    loss_tv = total_variation_loss(input_image, float(args.tv_weight))

# --------------- 选择优化方法并且返回图像 ----------------

    if args.lbfgs:
        if not Matting:
            overall_loss = loss_content + loss_tv + loss_style
        else:
            overall_loss = loss_content + loss_style + loss_tv + loss_affine

        optimizer = tf.contrib.opt.ScipyOptimizerInterface(overall_loss, method='L-BFGS-B', options={'maxiter': args.max_iter, 'disp': 0})
        sess.run(tf.global_variables_initializer())
        print_loss_partial = partial(print_loss, args)
        optimizer.minimize(sess, fetches=[loss_content, loss_styles_list, loss_tv, loss_affine, overall_loss, input_image_plus], loss_callback=print_loss_partial)

        global min_loss, best_image, iter_count
        best_result = copy.deepcopy(best_image)
        min_loss, best_image = float("inf"), None
        return best_result
    else:
        VGGNetLoss = loss_content + loss_tv + loss_style
        optimizer = tf.train.AdamOptimizer(learning_rate=args.learning_rate, beta1=0.9, beta2=0.999, epsilon=1e-08)
        VGG_grads = optimizer.compute_gradients(VGGNetLoss, [input_image])

        if Matting:
            b, g, r = tf.unstack(input_image_plus / 255., axis=-1)
            b_gradient = tf.transpose(tf.reshape(2 * tf.sparse_tensor_dense_matmul(M, tf.expand_dims(tf.reshape(tf.transpose(b), [-1]), -1)), [content_width, content_height]))
            g_gradient = tf.transpose(tf.reshape(2 * tf.sparse_tensor_dense_matmul(M, tf.expand_dims(tf.reshape(tf.transpose(g), [-1]), -1)), [content_width, content_height]))
            r_gradient = tf.transpose(tf.reshape(2 * tf.sparse_tensor_dense_matmul(M, tf.expand_dims(tf.reshape(tf.transpose(r), [-1]), -1)), [content_width, content_height]))

            Matting_grad = tf.expand_dims(tf.stack([b_gradient, g_gradient, r_gradient], axis=-1), 0) / 255. * args.affine_weight
            VGGMatting_grad = [(VGG_grad[0] + Matting_grad, VGG_grad[1]) for VGG_grad in VGG_grads]

            train_op = optimizer.apply_gradients(VGGMatting_grad)
        else:
            train_op = optimizer.apply_gradients(VGG_grads)

        sess.run(tf.global_variables_initializer())
        min_loss, best_image = float("inf"), None
        for i in xrange(1, args.max_iter):
            _, loss_content_, loss_styles_list_, loss_tv_, loss_affine_, overall_loss_, output_image_ = sess.run([
                train_op, loss_content, loss_styles_list, loss_tv, loss_affine, VGGNetLoss, input_image_plus
            ])
            if i % args.print_iter == 0:
                print('Iteration {} / {}\n\tContent loss: {}'.format(i, args.max_iter, loss_content_))
                for j, style_loss_ in enumerate(loss_styles_list_):
                    print('\tStyle {} loss: {}'.format(j + 1, style_loss_))
                print('\tTV loss: {}'.format(loss_tv_))
                if Matting:
                    print('\tAffine loss: {}'.format(loss_affine_))
                print('\tTotal loss: {}'.format(overall_loss_ - loss_tv_))

            if overall_loss_ < min_loss:
                min_loss, best_image = overall_loss_, output_image_

            if i % args.save_iter == 0 and i != 0:
                save_result(best_image[:, :, ::-1], os.path.join(args.serial, 'out_iter_{}.png'.format(i)))

        return best_image
```
tensorflow 中显存的分配有两种方式

* 按比例

```
config = tf.ConfigProto()
config.gpu_options.per_process_gpu_memory_fraction = 0.4
session = tf.Session(config = config, ...)
```

* 按需求增长

```
config = tf.ConfgiProto()
config.gpu_options.allow_growth = True
session = tf.Session(config = config, ...)
```

ref [http://blog.csdn.net/cq361106306/article/details/52950081](http://blog.csdn.net/cq361106306/article/details/52950081)


