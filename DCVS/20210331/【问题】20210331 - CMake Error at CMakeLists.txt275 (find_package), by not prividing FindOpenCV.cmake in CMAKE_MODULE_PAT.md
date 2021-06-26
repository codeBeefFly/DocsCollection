# 【问题】20210331 - CMake Error at CMakeLists.txt:275 (find_package), by not prividing "FindOpenCV.cmake" in CMAKE_MODULE_PATH



## 问题描述

cmake 构建 opencv 出问题

**终端输出**

```
-- @ point 0
-- CMAKE_MODULE_PATH: /home/ds16/work/TauristarPlatform/src/tauristar_platform/cmake
-- CMAKE_PREFIX_PATH: /home/ds16/work/TauristarPlatform/src/tauristar_platform/third_party/g2o_9b41a4e/cmake_modules
-- TAURISTAR_SRC_DIR: /home/ds16/work/TauristarPlatform/src/tauristar_platform
-- CMAKE_BINARY_DIR:  /home/ds16/work/TauristarPlatform/src/tauristar_platform/cmake-build-debug
-- TAURISTAR_THIRD_PARTY: /home/ds16/work/TauristarPlatform/src/tauristar_platform/third_party
-- TAURISTAR_CROSS_COMPONENTS_DIR: /home/ds16/work/TauristarPlatform/src/tauristar_platform/cross_components
-- @ point 1
-- INCLUDE_OUTPUT_PATH: /home/ds16/work/TauristarPlatform/src/tauristar_platform/cmake-build-debug/include
-- @ POINT TST_PREBUILD_PATH /home/ds16/work/TauristarPlatform/src/tauristar_platform/third_party/install/x86_64
-- @ point OpenCV: 
CMake Error at CMakeLists.txt:275 (find_package):
  By not providing "FindOpenCV.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "OpenCV", but
  CMake did not find one.

  Could not find a package configuration file provided by "OpenCV" with any
  of the following names:

    OpenCVConfig.cmake
    opencv-config.cmake

  Add the installation prefix of "OpenCV" to CMAKE_PREFIX_PATH or set
  "OpenCV_DIR" to a directory containing one of the above files.  If "OpenCV"
  provides a separate development package or SDK, be sure it has been
  installed.


-- Configuring incomplete, errors occurred!
See also "/home/ds16/work/TauristarPlatform/src/tauristar_platform/cmake-build-debug/CMakeFiles/CMakeOutput.log".

[Finished]
```



---



## 问题解决

**构建 opencv 时要安装依赖**

TS 编译依赖（zhanghongbo）

sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler libatlas-base-dev

---

opencv 编译依赖
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
————————————————
版权声明：本文为CSDN博主「ZeroZone零域」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ksws0292756/article/details/79511170

---

opencv 编译依赖2

首先，更新以下系统：
$ sudo apt-get update
$ sudo apt-get upgrade

接着，安装需要的编译工具
$ sudo apt-get install build-essential cmake pkg-config

然后，安装相应的依赖包
$ sudo apt-get install libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
$ sudo apt-get install libxvidcore-dev libx264-dev
$ sudo apt-get install libgtk-3-dev
$ sudo apt-get install libatlas-base-dev gfortran
————————————————
版权声明：本文为CSDN博主「ZeroZone零域」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ksws0292756/article/details/79511170