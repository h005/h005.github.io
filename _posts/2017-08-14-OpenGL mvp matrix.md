---
layout: post
title: OpenGL MVP matrix analysis
moto: 你眼中看似落叶纷飞变化无常的世界，实际只是躺在上帝怀中一份早已谱好的乐章。
date: 2017-08-14 10:40:54 +0800
categories: document
img: DSC01209.jpg
tag: openGL
---

* content
{:toc}

# OpenGL Model-View matrix decomposition and analysis

一般的Model-View矩阵包含着旋转,平移和缩放的信息,这就会导致Model-View矩阵的旋转部分(R = ModelView(1:3,1:3))并不是一个正规的旋转矩阵(R * R' != I).

所以在分解Model-View矩阵之前要先将将其规则化,使得R *　Ｒ' == I.

规则化的代码如下：

```
glm::mat4 normalizedModelView(const glm::mat4 &mvMatrix)
{
    glm::mat3 R = glm::mat3(mvMatrix);
    float c = glm::length(R[0]);
    glm::mat4 normalizedModelView = mvMatrix;
    normalizedModelView /= c;
    normalizedModelView[3][3] = 1.f;
    return normalizedModelView;
}
```

Model-View矩阵其实是描述相机外参的，也就是说相机的位置，相机镜头的朝向，以及相机的正方向（这个方向决定了所拍摄的照片与竖直方向的夹角）

```
void recoveryLookAtWithModelView(
        const glm::mat4 &viewMatrix,
        glm::vec3 &eye,
        glm::vec3 &center,
        glm::vec3 &up) {
    glm::vec3 t = glm::vec3(viewMatrix[3]);
    glm::mat3 R = glm::mat3(viewMatrix);
    // OC是世界中心指向相机中心的向量（世界坐标系下），也是相机中心在世界坐标系下的表示
    // 从公式角度看，有:
    // | R t |   |OC|   |0|
    // |     | * |  | = | |
    // | 0 1 |   |1 |   |1|
    // 所以R * OC + t = 0，可以求出C在世界坐标系下的表示
    //
    // 另外，由于旋转矩阵R是正交矩阵，所以R^(-1) = R^T
    // 对正交矩阵用转置代替求逆能节省cpu资源
    eye = -glm::transpose(R) * t;
    center = glm::normalize(glm::transpose(R) * glm::vec3(0.f, 0.f, -1.f)) + eye;
    up = glm::normalize(glm::transpose(R) * glm::vec3(0.f, 1.f, 0.f));
}
```

# OpenGL Projection matrix decomposition and analysis

ref [https://stackoverflow.com/questions/10830293/decompose-projection-matrix44-to-left-right-bottom-top-near-and-far-boundary](https://stackoverflow.com/questions/10830293/decompose-projection-matrix44-to-left-right-bottom-top-near-and-far-boundary)

**需要注意的是，上面的参考是以投影矩阵的可读模式，并且下标是从１开始的**

投影矩阵决定了相机的内参，其中包括，远近截屏面，以及上下左右边界，需要确定６个参数．

### 正交投影

```
near   =  (1+m34)/m33;
far    = -(1-m34)/m33;
bottom =  (1-m24)/m22;
top    = -(1+m24)/m22;
left   = -(1+m14)/m11;
right  =  (1-m14)/m11;
```

### 透视投影

```
void Fea::decomposeProjMatrix(glm::mat4 &proj,
                              glm::mat4 &enlargedProjMatrix)
{
    // ref https://stackoverflow.com/questions/10830293/decompose-projection-matrix44-to-left-right-bottom-top-near-and-far-boundary
    float near = proj[3][2] / (proj[2][2] - 1);
    float far = proj[3][2] / (proj[2][2] + 1);
    float bottom = near * (proj[2][1] - 1) / proj[1][1];
    float top = near * (proj[2][1] + 1) / proj[1][1];
    float left = near * (proj[2][0] - 1) / proj[0][0];
    float right = near * (proj[2][0] + 1) / proj[0][0];

    enlargedProjMatrix = glm::frustum(3*left, 3* right, 3 * bottom, 3 * top, near, far);
}
```