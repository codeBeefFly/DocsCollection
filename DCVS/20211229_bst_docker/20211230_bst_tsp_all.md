# 20211230: 详细记录 bst tsp all 编译修改



---



[toc]



---

## 0. 基本信息：

```shell
TS 版本:

# git rev-parse HEAD
4f0fb55ba3b306c81fb685fedbdece2c7a4836b8

# git log
commit 4f0fb55ba3b306c81fb685fedbdece2c7a4836b8 (HEAD -> master, origin/master, origin/HEAD)
Author: yuwei <yuwei.dong@aitronx.com>
Date:   Mon Dec 6 08:39:42 2021 +0800

    ticket# 7159 fix sometime hor parkspace is too short

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

BST docker 版本：

/opt/bstos/1.1.2.1/

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

文件夹：

work_3

```





## 1. BST 下编译完整的 TSP 需要修改的文件：



```shell
# git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   CMakeLists.txt
	modified:   cmake/nl2sol.cmake
	modified:   cmake/opencv.cmake
	modified:   tauristar_perception/tauristar_avm/library/avm_util/lens.cpp

```



当前情况下，一共有4个文件需要修改（仅限于当前情况），如果之后有编译出错，可以使用当前的文件修改作为还原。



**文件1**： `CMakeLists.txt`

`build rosbridgecpp` 能否在 cmake 中作为选项指明。

```shell
root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform# git diff CMakeLists.txt 
diff --git a/src/tauristar_platform/CMakeLists.txt b/src/tauristar_platform/CMakeLists.txt
index dbadc113..b3bcd86e 100755
--- a/src/tauristar_platform/CMakeLists.txt
+++ b/src/tauristar_platform/CMakeLists.txt
@@ -110,7 +110,7 @@ else()  #################################### cross_make
   if(BUILD_HORIZON)
     OPTION(BUILD_WITH_ROSBRIDGECPP "build rosbridgecpp" OFF)
   else()
-    OPTION(BUILD_WITH_ROSBRIDGECPP "build rosbridgecpp" ON)
+    OPTION(BUILD_WITH_ROSBRIDGECPP "build rosbridgecpp" OFF)
   endif()
   OPTION(BUILD_WITH_CAFFE "build caffe-jacinto app" OFF)
   OPTION(BUILD_WITH_BSD "build bsd app" OFF)

```



**文件2**：`cmake/nl2sol.cmake`

这个文件暂时记录一下，之后不一定需要。

```shell
root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform# git diff CMakeLists.txt 
diff --git a/src/tauristar_platform/CMakeLists.txt b/src/tauristar_platform/CMakeLists.txt
index dbadc113..b3bcd86e 100755
--- a/src/tauristar_platform/CMakeLists.txt
+++ b/src/tauristar_platform/CMakeLists.txt
@@ -110,7 +110,7 @@ else()  #################################### cross_make
   if(BUILD_HORIZON)
     OPTION(BUILD_WITH_ROSBRIDGECPP "build rosbridgecpp" OFF)
   else()
-    OPTION(BUILD_WITH_ROSBRIDGECPP "build rosbridgecpp" ON)
+    OPTION(BUILD_WITH_ROSBRIDGECPP "build rosbridgecpp" OFF)
   endif()
   OPTION(BUILD_WITH_CAFFE "build caffe-jacinto app" OFF)
   OPTION(BUILD_WITH_BSD "build bsd app" OFF)
root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform# git diff cmake/nl2sol.cmake 
diff --git a/src/tauristar_platform/cmake/nl2sol.cmake b/src/tauristar_platform/cmake/nl2sol.cmake
index d9a29d57..41ad918c 100644
--- a/src/tauristar_platform/cmake/nl2sol.cmake
+++ b/src/tauristar_platform/cmake/nl2sol.cmake
@@ -10,7 +10,8 @@ if (cross_make)
     set(NL2SOL_LIBRARIES_PATH ${PROJECT_SOURCE_DIR}/third_party/nl2sol/install_horizon)
   endif()
 else()
-  set(NL2SOL_LIBRARIES_PATH ${PROJECT_SOURCE_DIR}/third_party/nl2sol/install_x86)
+  # set(NL2SOL_LIBRARIES_PATH ${PROJECT_SOURCE_DIR}/third_party/nl2sol/install_x86)
+  set(NL2SOL_LIBRARIES_PATH ${PROJECT_SOURCE_DIR}/third_party/nl2sol/install_horizon)
 endif()
 
 message("NL2SOL LIBRARIES PATH: ${NL2SOL_LIBRARIES_PATH}")

```



**文件3**：`cmake/opencv.cmake`

使用的是 `opencv 344` 版本。

```shell
root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform# git diff cmake/opencv.cmake 
diff --git a/src/tauristar_platform/cmake/opencv.cmake b/src/tauristar_platform/cmake/opencv.cmake
index 788d2ee9..2d2b0e2c 100644
--- a/src/tauristar_platform/cmake/opencv.cmake
+++ b/src/tauristar_platform/cmake/opencv.cmake
@@ -38,5 +38,7 @@ if (cross_make)
   endif()
 else()
     #set(OpenCV_DIR "${PROJECT_SOURCE_DIR}/third_party/install/x86_64/opencv_v3.4.4/share/OpenCV")
-    find_package(OpenCV REQUIRED)
+    #find_package(OpenCV REQUIRED)
+    set(OpenCV_DIR "/usr/local/share/OpenCV")
+    find_package(OpenCV REQUIRED PATHS /usr/local/ NO_DEFAULT_PATH)
 endif()

```



**文件4**：`tauristar_perception/tauristar_avm/library/avm_util/lens.cpp `

这个文件的修改只是用来debug。

```shell
# git diff tauristar_perception/tauristar_avm/library/avm_util/lens.cpp 
diff --git a/src/tauristar_platform/tauristar_perception/tauristar_avm/library/avm_util/lens.cpp b/src/tauristar_platform/tauristar_perception/tauristar_avm/library/avm_util/lens.cpp
index 9a0ec248..27788b63 100644
--- a/src/tauristar_platform/tauristar_perception/tauristar_avm/library/avm_util/lens.cpp
+++ b/src/tauristar_platform/tauristar_perception/tauristar_avm/library/avm_util/lens.cpp
@@ -84,8 +84,10 @@ static std::unique_ptr<FakeLensData> static_fakelensdata;
 
 float *getLenData(int type)
 {
-    if (type < cameralensdata.CameraLensNum)
+    if (type < cameralensdata.CameraLensNum){
+        printf("**** 1. LenData[type]: %f, %f, %f\n", LenData[type][0], LenData[type][1], LenData[type][2]);
         return LenData[type];
+    }
 
     if (type > 10000) {
         if (!static_fakelensdata) {
@@ -145,6 +147,7 @@ void lensSet()
 {
     for (int i = 0; i != cameralensdata.CameraLensNum; i++) {
         LenData[i] = cameralensdata.lendisData[i];
+        printf("**** 2. LenData[i]: %f\n", LenData[i]);
     }
 }
 
@@ -176,8 +179,11 @@ bool AnalysisCameraLensDataBase(char const *lensDatpath)
     size_t sz = fread(&cameralensdata.CameraLensNum, sizeof(int), 1, fp);
     for (int i = 0; i<cameralensdata.CameraLensNum; i++) {
         sz = fread(&cameralensdata.SensorPara[i], sizeof(CameraParam), 1, fp);
+        printf("**** 3. cameralensdata.SensorPara[%d]: %d\n", cameralensdata.SensorPara[i].eyeW);
         sz = fread(&cameralensdata.datalength[i], sizeof(int), 1, fp);
+        printf("**** 4. cameralensdata.datalength[%d]: %d\n", cameralensdata.datalength[i]);
         sz = fread(&cameralensdata.lendisData[i], sizeof(float) * cameralensdata.datalength[i], 1, fp);
+        printf("**** 5. cameralensdata.lendisData[%d]: %d\n", cameralensdata.lendisData[i][3]);
     }
 
     fclose(fp);

```



---



## 2. BST AVM standalone 编译



这个可能要借鉴一下最后一次编译的文件修改。



### 1. 尝试 ubuntu 18.04 下 stand alone 编译 `tauristar_avm`（完成）

路径：`src/tauristar_platform/tauristar_perception/tauristar_avm`



**编译操作：**

| ![image-20211230194548946](20211230_bst_tsp_all.assets/image-20211230194548946.png) | ![image-20211230194624300](20211230_bst_tsp_all.assets/image-20211230194624300.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |



**编译命令：**

```cmake
# cmake 命令示例

cmake -DOpenCV_DIR=/root/usr/local/share/OpenCV -DBUILD_WITH_ROS=OFF -DBUILD_WITH_ROSBRIDGECPP=OFF ..


# 关于 opencv 路径
    #set(OpenCV_DIR "${PROJECT_SOURCE_DIR}/third_party/install/x86_64/opencv_v3.4.4/share/OpenCV")
    find_package(OpenCV REQUIRED)
    
    set(OpenCV_INCLUDE_DIRS ${DEPS_ROOT}/opencv/include)
    set(OpenCV_LIBS_PATH ${DEPS_ROOT}/opencv/lib
    link_directories(${OpenCV_LIBS_PATH})
    
# 关于 boost 路径
    find_package(Boost REQUIRED COMPONENTS serialization system program_options thread filesystem regex)
  
    set(Boost_INCLUDE_DIRS ${DEPS_ROOT}/boost/include)
    set(Boost_LIBRARIES_PATH ${DEPS_ROOT}/boost/lib)
    link_directories(${Boost_LIBRARIES_PATH})
```



**编译实操：**

```shell
# 在 /src/tauristar_platform/tauristar_perception/tauristar_avm 目录下

mkdir build
cd build

# 我的 opencv 路径 /usr/local/opencv-3.4.5/

cmake .. -DAVM_STANDALONE=ON -DOpenCV_DIR=/usr/local/opencv/opencv-3.4.5/share/OpenCV
make -j10

# 将头文件，库文件以及二进制文件拷贝到 install 目录下

make install

src/tauristar_platform/tauristar_perception/tauristar_avm$ tree . -L 1
.
├── app
├── build
├── CMakeLists.txt
├── install
├── library
└── README.md


src/tauristar_platform/tauristar_perception/tauristar_avm/install$ tree
.
├── bin
...
├── include
...
...
└── lib
    ├── cmake
    │   └── yaml-cpp
	...
    ├── liblib_avm_algobase.so					# bst 需要的 avm 库
    ├── liblib_avm_bvc.so						# bst 需要的 avm 库
    ├── liblib_avm_calib.so						# bst 需要的 avm 库
    ├── liblib_avm_dcal.so						# bst 需要的 avm 库
    ├── liblib_avm.so							# bst 需要的 avm 库
    ├── liblib_avm_util.so						# bst 需要的 avm 库
	...
    └── pkgconfig
		...

```



**所需要的头文件：**

```shell
ds18@ubuntu:~/Work/TSP_BST_DOC$ tree library/
library/
├── avm_algo_base
│   └── point2D.h
├── avm_bvclib
│   ├── avm_bvc_engine.h
│   └── bright_alg.h
├── avm_calib_lib
│   ├── bvsCalibAlg.h
│   ├── deavm_edgelet.h
│   └── preprocessing.h
├── avm_lib
│   ├── camerainit.h
│   ├── gl3DModel.h
│   ├── lut_fixedgrid_2d.h
│   ├── ResidueImageStitch.h
│   ├── rv3dmodel.h
│   ├── single3dmodel.h
│   └── singleudis.h
└── avm_util
    ├── bv3d_xml.h
    ├── lens.h
    └── xmlResolve.h

5 directories, 16 files

```



### 2. 尝试在 bst docker 下 standalone 编译 `tauristar_avm`

```shell
root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/build# /opt/bstos/1.1.2.1/environment-setup-aarch64-bst-linux root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/build# cmake .. -DAVM_STANDALONE=ON -DOpenCV_DIR=/usr/local/share/OpenCV
-- Toolchain file defaulted to '/opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/share/cmake/OEToolchainConfig.cmake'
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/bin/aarch64-bst-linux/aarch64-bst-linux-gcc
-- Check for working C compiler: /opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/bin/aarch64-bst-linux/aarch64-bst-linux-gcc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/bin/aarch64-bst-linux/aarch64-bst-linux-g++
-- Check for working CXX compiler: /opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/bin/aarch64-bst-linux/aarch64-bst-linux-g++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- CMAKE_BINARY_DIR:  /home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/build
-- PROJECT_SOURCE_DIR /home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm
-- Found OpenCV: /usr/local (found version "3.4.4") 
-- Boost version: 1.68.0
-- Found the following Boost libraries:
--   filesystem
--   system
CMake Deprecation Warning at /home/work_3/TauristarPlatform/src/tauristar_platform/third_party/yaml-cpp/CMakeLists.txt:10 (cmake_policy):
  The OLD behavior for policy CMP0012 will be removed from a future version
  of CMake.

  The cmake-policies(7) manual explains that the OLD behaviors of all
  policies are deprecated and that a policy should be set to OLD only under
  specific short-term circumstances.  Projects should be ported to the NEW
  behavior and not rely on setting a policy to OLD.


CMake Deprecation Warning at /home/work_3/TauristarPlatform/src/tauristar_platform/third_party/yaml-cpp/CMakeLists.txt:14 (cmake_policy):
  The OLD behavior for policy CMP0015 will be removed from a future version
  of CMake.

  The cmake-policies(7) manual explains that the OLD behaviors of all
  policies are deprecated and that a policy should be set to OLD only under
  specific short-term circumstances.  Projects should be ported to the NEW
  behavior and not rely on setting a policy to OLD.


-- Performing Test FLAG_WEXTRA
-- Performing Test FLAG_WEXTRA - Success
-- a--- Build type: 
-- b--- Build type: 
-- Build spdlog: 1.4.2
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Looking for pthread_create
-- Looking for pthread_create - not found
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Build type: Release
-- CMAKE_INSTALL_PREFIX: /home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/install
AVM UTIL: STANDALONE
-- Configuring done
-- Generating done
-- Build files have been written to: /home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/build

```



#### 遇到的问题：

##### 1. 报错：optimizer，（已解决）

```
[ 11%] Linking CXX shared library liboptimizer.so
[ 12%] Building CXX object yaml-cpp/CMakeFiles/yaml-cpp.dir/src/convert.cpp.o
/opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/libexec/aarch64-bst-linux/gcc/aarch64-bst-linux/8.3.0/real-ld: skipping incompatible /home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/../../third_party/nl2sol/install_x86/libnl2sol.so when searching for -lnl2sol
/opt/bstos/1.1.2.1/sysroots/x86_64-bstsdk-linux/usr/libexec/aarch64-bst-linux/gcc/aarch64-bst-linux/8.3.0/real-ld: cannot find -lnl2sol
collect2: error: ld returned 1 exit status
optimizers/CMakeFiles/optimizer.dir/build.make:83: recipe for target 'optimizers/liboptimizer.so' failed
make[2]: *** [optimizers/liboptimizer.so] Error 1
CMakeFiles/Makefile2:299: recipe for target 'optimizers/CMakeFiles/optimizer.dir/all' failed
make[1]: *** [optimizers/CMakeFiles/optimizer.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[ 12%] Building CXX object library/avm_dcal_lib/CMakeFiles/lib_avm_dcal.dir/DynamicCalibEngine.cc.o

```

解决：

修改上级 `CMakeLists.txt`：将与 optimizer 有关的代码全部注释掉。

```shell
root@77da538bab4a:/home/work_3/TauristarPlatform/src/tauristar_platform/tauristar_perception/tauristar_avm/build# git diff ../CMakeLists.txt 
diff --git a/src/tauristar_platform/tauristar_perception/tauristar_avm/CMakeLists.txt b/src/tauristar_platform/tauristar_perception/tauristar_avm/CMakeLists.txt
index ba621d26..548bc85b 100644
--- a/src/tauristar_platform/tauristar_perception/tauristar_avm/CMakeLists.txt
+++ b/src/tauristar_platform/tauristar_perception/tauristar_avm/CMakeLists.txt
@@ -20,9 +20,9 @@ IF (AVM_STANDALONE)
   include_directories("${THIRD_PARTY_ROOT}/yaml-cpp/include")
 
   link_directories(${THIRD_PARTY_ROOT}/nl2sol/install_x86)
-  set(OPTIMIZER_ROOT ${PROJECT_SOURCE_DIR}/../../optimizers)
-  add_subdirectory(${OPTIMIZER_ROOT} optimizers)
-  include_directories(${OPTIMIZER_ROOT})
+  #set(OPTIMIZER_ROOT ${PROJECT_SOURCE_DIR}/../../optimizers)
+  #add_subdirectory(${OPTIMIZER_ROOT} optimizers)
+  #include_directories(${OPTIMIZER_ROOT})
 
   SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
   SET(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)
@@ -57,7 +57,7 @@ ENDIF()
 
 set (AVM_SYSTEM_LIBS
   ${AVM_SYSTEM_LIBS}
-  optimizer
+  #optimizer
   yaml-cpp
   ${OpenCV_LIBS}
   ${Boost_LIBRARIES}

```

编译结果：

```shell
cc1plus: warning: include location "/usr/local/include" is unsafe for cross-compilation [-Wpoison-system-directories]
cc1plus: warning: include location "/usr/local/include/opencv" is unsafe for cross-compilation [-Wpoison-system-directories]
[ 96%] Linking CXX executable ../../bin/d2t_conv
[ 96%] Built target d2t_conv
[ 97%] Linking CXX executable ../../bin/t2d_conv
[ 97%] Built target t2d_conv
[ 97%] Linking CXX executable ../../bin/bvs_calib
[ 97%] Built target bvs_calib
[ 97%] Linking CXX executable ../../bin/avm_bvc
[ 97%] Built target avm_bvc
[ 97%] Linking CXX executable ../../bin/cvt_yuv
[ 97%] Built target cvt_yuv
[ 98%] Linking CXX executable ../../bin/avm_dynamic_cal
[ 98%] Built target avm_dynamic_cal
[100%] Linking CXX executable ../../bin/avm_dcal
[100%] Built target avm_dcal
[100%] Linking CXX executable ../../bin/avm_calib
[100%] Built target avm_calib

```

