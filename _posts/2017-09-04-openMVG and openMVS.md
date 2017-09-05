---
layout: post
title: 3D Reconstruction with openMVG and openMVS
moto: For a new beginning
date: 2017-09-04 14:59:55
categories: document
img: 20170901.jpg
tag: 3D reconstruction
---

* content
{:toc}

# openMVG

[openMVG](https://github.com/openMVG/openMVG) provide a collection of tiny libraries that allow to solve computer vision problems and build complete SfM pipeline.

## build the library on Ubuntu 16.04

ref [build](https://github.com/openMVG/openMVG/blob/master/BUILD.md)

ATTENTION! clone the repository with **--recursive**

Build OpenMVG

```
$ git clone --recursive https://github.com/openMVG/openMVG.git
$ cd openMVG
$ ls
 AUTHORS BUILD  docs  logo  README  src  ...
$ cd ..
$ mkdir openMVG_Build
$ cd openMVG_Build

```

If you want to add unit tests and examples to the build, run

```
$ cmake -DCMAKE_BUILD_TYPE=RELEASE -DOpenMVG_BUILD_TESTS=ON -DOpenMVG_BUILD_EXAMPLES=ON . ../openMVG/src/
```

otherwise

```
$ cmake -DCMAKE_BUILD_TYPE=RELEASE . ../openMVG/src/
```

Compile the project

```
$make -j NBcore
```

## Structure from motion pipline

### [1. Image listing](http://openmvg.readthedocs.io/en/latest/software/SfM/SfMInit_ImageListing/)

### [2. Image description computation](http://openmvg.readthedocs.io/en/latest/software/SfM/ComputeFeatures/)

### [3. Corresponding images and correspondences computation](http://openmvg.readthedocs.io/en/latest/software/SfM/ComputeMatches/)

### 4. SfM solving

[openMVG_main_IncrementalSfM](http://openmvg.readthedocs.io/en/latest/software/SfM/IncrementalSfM/)

[openMVG_main_GlobalSfM](http://openmvg.readthedocs.io/en/latest/software/SfM/GlobalSfM/)

### 5. Optional further processing

[openMVG_main_ComputeSfM_DataColor](http://openmvg.readthedocs.io/en/latest/software/SfM/ComputeSfM_DataColor/)

[openMVG_main_ComputeStructureFromKnownPoses](http://openmvg.readthedocs.io/en/latest/software/SfM/ComputeStructureFromKnownPoses/)

[openMVG_main_ExportUndistortedImages](http://openmvg.readthedocs.io/en/latest/software/SfM/ExportUndistortedImages/)

### 6. Optional further processing of [MVS](http://openmvg.readthedocs.io/en/latest/software/MVS/MVS/)


At last, we can explore the pipline in the python script.


# [OpenMVS Open Multiple View Stereovision](http://cdcseacave.github.io/openMVS/)

I refer you to download the source code from the [openMVS](https://codeload.github.com/cdcseacave/openMVS/legacy.tar.gz/master)


### Dependencies

* [Eigen](https://launchpadlibrarian.net/188145886/libeigen3-dev_3.2.2-3_all.deb) **version 3.2 MUST version 3.2**
* OpenCV version 2.4 or higher
* Ceres version 1.10 or higher
* CGAL version 4.2 or higher
* Boost version 1.56 or higher
* VCG

------

### Compilation

```
#Prepare and empty machine for building:
sudo apt-get update -qq && sudo apt-get install -qq
sudo apt-get -y install build-essential git mercurial cmake libpng-dev libjpeg-dev libtiff-dev libglu1-mesa-dev
main_path=`pwd`

#Eigen (Required)
hg clone https://bitbucket.org/eigen/eigen#3.2
mkdir eigen_build && cd eigen_build
cmake . ../eigen
make && sudo make install
cd ..

#Eigen install
# Compiling the Eigen from the source code will encounter many problems, I suggest install this lib with the deb package.
sudo dpkg -i libeigen3-dev_3.2.2-3_all.deb


#Boost (Required)
sudo apt-get -y install libboost-iostreams-dev libboost-program-options-dev libboost-system-dev libboost-serialization-dev

#OpenCV (Required)
# you can compile the OpenCV from the source code
sudo apt-get -y install libopencv-dev

#CGAL (Required)
sudo apt-get -y install libcgal-dev libcgal-qt5-dev

#VCGLib (Required)
git clone https://github.com/cdcseacave/VCG.git vcglib

#Ceres (Required)

# There is an another website for the Ceres [http://docs.opencv.org/trunk/db/db8/tutorial_sfm_installation.html](http://docs.opencv.org/trunk/db/db8/tutorial_sfm_installation.html)

sudo apt-get -y install libatlas-base-dev libsuitesparse-dev
git clone https://ceres-solver.googlesource.com/ceres-solver ceres-solver
mkdir ceres_build && cd ceres_build
cmake . ../ceres-solver/ -DMINIGLOG=ON -DBUILD_TESTING=OFF -DBUILD_EXAMPLES=OFF
make -j2 && sudo make install
cd ..

#GLFW3 (Optional)
sudo apt-get -y install freeglut3-dev libglew-dev libglfw3-dev

#OpenMVS
git clone https://github.com/cdcseacave/openMVS.git openMVS
mkdir openMVS_build && cd openMVS_build
cmake . ../openMVS -DCMAKE_BUILD_TYPE=Release -DVCG_DIR="$main_path/vcglib"

#If you want to use OpenMVS as shared library, add to the CMake command:
-DBUILD_SHARED_LIBS=ON

#Install OpenMVS library (optional):
make -j4 && sudo make install
```

### Process the point cloud model

```
# Export the OpenMVG scene to the OpenMVS data format
$ openMVG_main_openMVG2openMVS -i PATH/sfm_data.(json/xml/bin) -d OUTPUT_PATH -o OUTPUT_PATH/Scene
```

#### 1. Dense point-cloud reconstruction for obtaining a complete and accurate as possible point-cloud

```
# Compute dense depth map per view and merge the depth map into a consistent point cloud
$ DensifyPointCloud scene.mvs
```

#### 2. Mesh reconstruction for extimating a mesh surface that explains the best the input point-cloud

```
# The initial point cloud be:
# - the calibration one (scene.mvs),
$ ReconstructMesh scene.mvs
# - or the dense one (scene_dense.mvs)
$ ReconstructMesh scene_dense.mvs
```

#### 3. Mesh texturing for computing a texture to color the mesh

```
# Compute the texture
$ TextureMesh scene_dense_mesh.mvs
```

