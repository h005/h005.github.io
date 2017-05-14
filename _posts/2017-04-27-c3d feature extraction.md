---
layout: post
title: C3D network
img: bunny1.jpg
moto: 静下心来，少说话，多做事！
date: 2017-04-27 15:33:00 +0800
categories: document
tag: Ubuntu
---

* content
{:toc}

# C3D net for feature extraction

C3D-v1.1 is released with new models (Mar 01, 2017). And there is no documentation for v1.1 yet. So I intend to make a summary about the C3D-v1.0.

## C3D-v1.0 for Feature Extraction with Frames

I am intend to make classification for 3D models with the frames of different viewpoints. To extract the C3D features, we should prepare the input files first. The frame names are formmatted as "path/%06d.jpg". Note that: frame numbers starting from **1 to N** for using frames as inputs. 

Change to the folder of 
```
YOUR_C3D_HOME/examples/c3d_feature_extraction
```

Then run the sh file
```
sh c3d_sport1m_feature_extraction_frm.sh
```

Check the c3d_sport1m_feature_extraction_frm.sh file, and the content is:

```
mkdir -p output/c3d/v_ApplyEyeMakeup_g01_c01
mkdir -p output/c3d/v_BaseballPitch_g01_c01
cd input/frm
tar xvzf v_ApplyEyeMakeup_g01_c01.tar.gz
tar xvzf v_BaseballPitch_g01_c01.tar.gz
cd ../..
GLOG_logtosterr=1 ../../build/tools/extract_image_features.bin prototxt/c3d_sport1m_feature_extractor_frm.prototxt conv3d_deepnetA_sport1m_iter_1900000 0 50 1 prototxt/output_list_prefix.txt fc7-1 fc6-1 prob
```

after analyze this sh file, we can find the core command is 
```
../../build/tools/extract_image_features.bin prototxt/c3d_sport1m_feature_extractor_frm.prototxt conv3d_deepnetA_sport1m_iter_1900000 0 50 1 prototxt/output_list_prefix.txt fc7-1 fc6-1 prob
```

The parameters are:

* \<feature_extractor_prototxt_file\>, the prototxt file, which contains the **input list file**.

* \<c3d_pre_trained_model\>, the C3D model pre-trained model downloaded.

* \<gpu_id\>, GPU id you would like to run, if it is set to -1, then it will use CPU.

* \<mini_batch_size\>, your mini batch size. Default is 50, but you can modify this number, depend on you GPU memory.

* \<number_of_mini_batches\>, Number of mini-batches you want to extarct features. For exampels, if you have 100 clips to extract features and you are using mini-batch size of 50, then this number should be set to 2. However, if you have 101 clips to be extracted features, then this number shoudl be set to 3.

* \<output_prefix_file\>, the output prefix file.

* \<feature_name1\>, just the feature name, one of {fc6-1, fc7-1, fc8-1, pool5, prob, ...}

* \<feature_name2\>, one of {fc6-1, fc7-1, fc8-1, pool5, prob, ...}

We should pay attention to that the input list file was contained in the prototxt file.

And my **c3d_modelNet40_frm.sh** file contains the context:

```
GLOG_logtosterr=1 ../../build/tools/extract_image_features.bin prototxt/modelNet40/modelNet_model40.prototxt conv3d_deepnetA_sport1m_iter_1900000 0 50 389 prototxt/modelNet40/output_list_frm_model40.txt fc7-1 fc6-1 prob
```


### Prepare the setting files

We should prepare two files: **input list** and the **output prefix** file.

#### input list file

Each line of the input list file has the format:

```
<string_path> <starting_frame> <label>

example:

/home/dinggou/3dData/ModelNet40/airplane/test/airplane_0627/ 1 0
/home/dinggou/3dData/ModelNet40/airplane/test/airplane_0627/ 17 0
/home/dinggou/3dData/ModelNet40/airplane/test/airplane_0628/ 1 0
```

#### output list file

The output file prefix file is used to specify the locations for extracting features to be saved. C3D will save features are **output_prefix.[feture_name]** (e.g. **prefix.fc6**). Note that **C3D will NOT create the folder contains the output features**.

```
<output_prefix>

exmaple:

/home/dinggou/3dData/ModelNet40_resC3D/airplane_test_airplane_0627_1
/home/dinggou/3dData/ModelNet40_resC3D/airplane_test_airplane_0627_17
/home/dinggou/3dData/ModelNet40_resC3D/airplane_test_airplane_0628_1
```

------

In my project, I put the files of **input_list_frm_model40.txt**, **output_list_frm_model40.txt**, and **modelNet_model40.prototxt** into the folder of **modelNet40**. 

And this folder is in the directory of **YOUR_C3D_HOME/examples/c3d_feature_extraction/prototxt**

Besides, I create my own feature extract sh file named as **c3d_modelNet40_frm.sh** and put this file into the directory of **YOUR_C3D_HOME/examples/c3d_feature_extraction**.

```
GLOG_logtosterr=1 ../../build/tools/extract_image_features.bin prototxt/modelNet40/modelNet_model40.prototxt conv3d_deepnetA_sport1m_iter_1900000 0 50 389 prototxt/modelNet40/output_list_frm_model40.txt fc7-1 fc6-1 prob
```

