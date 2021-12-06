# 20211130 - imu ros development (init node)

[toc]




---

**LOG:**

2021年11月30日：解决imu 串口问题。

2021年12月01日：imu 数据解析。

2021年12月02日：imu 数据解析，conti。解决：imu_raw 丢包，ros imu_tools 问题。cmake 编译 c++ 问题。

2021年12月03日：解析 imu raw 数据。

---

convention: maximum logs recorded 3 - 4.

---



## 2021年12月03日

### 记录：

#### Task 00：阅读：link00：ubuntu 添加 swap 空间

```shell
# 1. 查看剩余空间
ds18@ubuntu:~$ free -m

total        used        free      shared  buff/cache   available
Mem:           9937        7943         679         169        1314        1518
Swap:          2047         831        1216

# 2. 创建 swap 空间
ds18@ubuntu:~$ sudo mkdir /swap

# 3. 为 swap 空间分配内存
ds18@ubuntu:~$ sudo dd if=/dev/zero of=swapfile bs=1024 count=8000000

8000000+0 records in
8000000+0 records out
8192000000 bytes (8.2 GB, 7.6 GiB) copied, 18.8047 s, 436 MB/s

# 4. 把生成的文件转化为 swap 文件
ds18@ubuntu:~$ sudo mkswap -f swapfile

mkswap: swapfile: insecure permissions 0644, 0600 suggested.
Setting up swapspace version 1, size = 7.6 GiB (8191995904 bytes)
no label, UUID=91502adc-145e-41f9-9685-f6c38d3d1b4a


# 5. 激活 swap 文件
ds18@ubuntu:~$ sudo swapon swapfile

swapon: /home/ds18/swapfile: insecure permissions 0644, 0600 suggested.

# 6. 再次查看空间
ds18@ubuntu:~$ free -m

total        used        free      shared  buff/cache   available
Mem:           9937        7937         125         169        1874        1527
Swap:          9860         834        9025
ds18@ubuntu:~$ 

```

<img src="20211130 - imu ros development (init node).assets/image-20211203164211591.png" alt="image-20211203164211591" style="zoom:80%;" align="left"/>



### Task 01: 完成 imu ros 驱动的解码与 imu_tools 的使用

todo: 代码整理。

<img src="20211130 - imu ros development (init node).assets/image-20211206093957711.png" alt="image-20211206093957711" style="zoom:80%;" align="left" />

<img src="20211130 - imu ros development (init node).assets/image-20211206094045314.png" alt="image-20211206094045314" style="zoom:80%;" align="left"/>

### 参考：

link00: [Ubuntu增加Swap空间大小](https://blog.csdn.net/yc461515457/article/details/53610412)



---



## 2021年12月02日

### 记录

今天将继续 2021-12-01 的工作。



#### Task 00：阅读：link00：Linux下使用CMake编译C++

项目架构：

```shell
ds18@ubuntu:~/catkin_x/CPP_WS/test_cpp_cmake$ tree .
.
├── bin
│   └── hello_world
├── build
├── CMakeLists.txt
├── include
│   └── a.h
├── lib
└── src
    ├── a.cpp
    └── main.cpp

5 directories, 5 files
```

>1. **bin -> 放置c++可执行文件**
>2. **build -> 放置编译生成文件**
>3. **src -> 放置 源文件，通俗点就是.cpp文件**
>4. **include -> 放置头文件，通俗点就是.h文件**
>5. **lib -> 放置 库文件，就是.so .a文件**



CmakeList.txt 内容详细：

```cmake
# 1、首先开头给出要求cmake最低版本以及工程名称
cmake_minimum_required(VERSION 2.8)
project(test)

# 2、设置编译模式
set(CMAKE_BUILD_TYPE Release)

# 3、设置可执行文件与链接库保存的路径
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# 4、设置头文件目录
include_directories(
    ${PROJECT_SOURCE_DIR}/include
)

# 5、添加编译文件
add_executable(
    hello_world 
    src/main.cpp src/a.cpp
)
```

编译过程 + 结果：

```shell
ds18@ubuntu:~/catkin_x/CPP_WS/test_cpp_cmake/build$ cmake ..
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
b-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/ds18/catkin_x/CPP_WS/test_cpp_cmake/build

ds18@ubuntu:~/catkin_x/CPP_WS/test_cpp_cmake/build$ make
Scanning dependencies of target hello_world
[ 33%] Building CXX object CMakeFiles/hello_world.dir/src/main.cpp.o
[ 66%] Building CXX object CMakeFiles/hello_world.dir/src/a.cpp.o
[100%] Linking CXX executable ../bin/hello_world
[100%] Built target hello_world

ds18@ubuntu:~/catkin_x/CPP_WS/test_cpp_cmake$ tree
.
├── bin
│   └── hello_world
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 3.10.2
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── a.out
│   │   │   │   ├── CMakeCCompilerId.c
│   │   │   │   └── tmp
│   │   │   └── CompilerIdCXX
│   │   │       ├── a.out
│   │   │       ├── CMakeCXXCompilerId.cpp
│   │   │       └── tmp
│   │   ├── cmake.check_cache
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── feature_tests.bin
│   │   ├── feature_tests.c
│   │   ├── feature_tests.cxx
│   │   ├── hello_world.dir
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── CXX.includecache
│   │   │   ├── DependInfo.cmake
│   │   │   ├── depend.internal
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   ├── progress.make
│   │   │   └── src
│   │   │       ├── a.cpp.o
│   │   │       └── main.cpp.o
│   │   ├── Makefile2
│   │   ├── Makefile.cmake
│   │   ├── progress.marks
│   │   └── TargetDirectories.txt
│   ├── cmake_install.cmake
│   └── Makefile
├── CMakeLists.txt
├── include
│   └── a.h
├── lib
└── src
    ├── a.cpp
    └── main.cpp

14 directories, 38 files


ds18@ubuntu:~/catkin_x/CPP_WS/test_cpp_cmake/bin$ ./hello_world 
Ready
```





### 参考

link00: [Linux下使用CMake编译C++](https://zhuanlan.zhihu.com/p/373256365)



---



## 2021年12月01日

### 记录：

#### Task 00：ROS IMU TOOLS（todo）

ros imu_tools  订阅的topic为`imu/data_raw`，因此如果需要使用这个工具，自己编写的 imu 驱动就需要发布 `imu/data_raw`的 topic。使用 imu_tools 的前提，有 imu 驱动发布 `imu/data_raw`。（todo）



#### Task 01：根据 ROS OFFICIAL 编写 ROS 驱动

参考 link01，link02。link14 是一个标准的 ros driver，在完成简单的 ros driver 之后，可以参考其进行封装。



#### Task 02：阅读：link15：3.使用串口读取IMU数据并通过话题发布

阅读了这篇文章，需要拿到 imu_data.h 代码。可以参考其中的 serial_imu_node.cpp 中。

找到了这篇文章代码的下载路径，粗略阅读了源码，发现目前没有办法理解这套代码，需要理解 ros service。

代码下载路径：

```
git clone https://code.corvin.cn:3000/corvin_zhang/rasp_imu_hat_6dof.git
```

代码地址：

link: [corvin_zhang](https://code.corvin.cn/corvin_zhang) / [rasp_imu_hat_6dof](https://code.corvin.cn/corvin_zhang/rasp_imu_hat_6dof)



部分 serial_imu_node.cpp 代码：

```c++
#include <ros/ros.h>
#include <tf/tf.h>
#include <imu_data.h>
#include <sensor_msgs/Imu.h>
#include <std_msgs/Float32.h>
#include <std_msgs/Empty.h>

#include "serial_imu_hat_6dof/setYawZero.h"    // ros service
#include "serial_imu_hat_6dof/setBaudRate.h"   // ros service
#include "serial_imu_hat_6dof/setPinOutHL.h"   // ros service
#include "serial_imu_hat_6dof/getYawData.h"    // ros service
```



#### Task 03：解决 ros 自定义 msg 头红色下划线问题

这篇文章的代码已经在 `test_imu_serial.cpp` 中验证可行，可以基于这套代码继续优化。

<img src="20211130 - imu ros development (init node).assets/image-20211201142305840.png" alt="image-20211201142305840" style="zoom:50%;" align="left"/>



注：在 package 下创建 message，并在代码引用 message，需要连续 catkin_make 两次，第一次是 编译 message，第二次是源代码引用 message.h。如果只编译一次的话，会报错 `package/message.h` 文件没找到。

编译的`message.h` 在`devel/include/package`下面。

不过依然会看到头文件会有红色下划线，同时 IDE 的 run 无法运行的情况：

> typora 图像左对齐 `<img src = "./images/python/logo.png" align="center">`

<img src="20211130 - imu ros development (init node).assets/image-20211201163612910.png" alt="image-20211201163612910" style="zoom:50%;" align="left"/>

这是因为`imu_1129/test_serial.h` clion 无法通过路径找到：

解决办法，case vscode:

<img src="20211130 - imu ros development (init node).assets/image-20211201164212617.png" alt="image-20211201164212617" style="zoom:50%;" align="left"/>

解决办法，case clion:

<img src="20211130 - imu ros development (init node).assets/image-20211201164306419.png" alt="image-20211201164306419" style="zoom:50%;" align="left"/>

clion 有没有不改变 `CMakeLists.txt`就可以解决问题的方法？可以参考 link19 - 20，但是我觉得复杂。



#### Task 04：阅读：link18：解决 clion ros 编译问题

没有修改之前，没有发现什么问题。

修改之后的截图。（mark point 1）

<img src="20211130 - imu ros development (init node).assets/image-20211201195253219.png" alt="image-20211201195253219" style="zoom:50%;" aligh="left"/>

```cmake
# clion 有什么方法不添加这一行吗？
set(DEVEL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/../../devel/include)

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
        include
        ${catkin_INCLUDE_DIRS}
#        /home/ds18/catkin_x/IMU_ws/devel/include
        ${DEVEL_INCLUDE_DIR}
)
```

现在 catkin_make，与 clion cmake 都可以编译程序。步骤：

使用 clion 打开 `toplevel` 的 `CMakeLists.txt`，在`/home/ds18/catkin_x/IMU_ws/src/CMakeLists.txt`。

子`CMakeLists.txt`分别在`/home/ds18/catkin_x/IMU_ws/src/package1/src/, ../package2/src`中。

需要编译两次，因为 message head 问题。然后就可以使用 clion 运行程序了。



刚才又仔细分析了一下问题细节。clion 编译 ros 代码动态头文件找不到的情况。

成功的解决办法是：将 mark point 1 配置一遍，然后在 terminal 使用 `catkin_make` 编译整个 ros 空间。这里需要 `catkin_make` 两次：第一次会在 `xxx_ws/devel/include` 生成 `msg_name.h`，但是在编译引用这个头文件的源文件时会报错；第二次编译就可以正常结束了。

编译正常结束之后，使用 clion 的 运行按钮运行包含这个头文件的源代码，clion 会再次编译一遍（如何解决重复编译问题？），编译成功后，不会出现头文件找不到的报错问题。

以这种方式配置的 clion 可以不用在此 `package/src/CMakeLists.txt` 中的 `include_directories` 中指明 `msg_name.h` 所在的路径：`xxx_ws/devel/include`。如图：

<img src="20211130 - imu ros development (init node).assets/image-20211201212850401.png" alt="image-20211201212850401" style="zoom:50%;" align="left"/>

现在的问题就是重复编译问题，可以暂时不考虑。

暂时使用如下方式进行编译：先使用命令行编译两遍，在使用 ide 运行，运行时会再编译一遍。



#### Task 05：阅读：link04：ROS串口通信（2）以十六进制指令读取IMU数据

```
0
0
print ACC data!
0
0
0
数据帧头错误
[ INFO] [1638361646.384576485]: OUTPUT_Q4
数据帧头错误
[ INFO] [1638361646.384655929]: OUTPUT_GYR
数据帧头错误
[ INFO] [1638361646.384700285]: OUTPUT_ACC
print Q4 data!
0
0
0
0
print GRY data!
0
0
```

最新进展：

删除了不必要的代码，发现两个问题：1. 丢包率高，2. ros imu_tools 订阅消息出问题。

| <img src="20211130 - imu ros development (init node).assets/image-20211202190107906.png" alt="image-20211202190107906" style="zoom:50%;" align="left"/> |
| ------------------------------------------------------------ |
| <img src="20211130 - imu ros development (init node).assets/image-20211202190221053.png" alt="image-20211202190221053" style="zoom:50%;" align="left"/> |



### Task 06：阅读：link21：ROS中发布IMU传感器消息

Task 05 架构很不错，但是问题暂时解决不了。先通过 Task 06 完成 imu_tools 的操作。

完成 Task04 遗留问题 2：ros imu_tools rviz 显示。

| <img src="20211130 - imu ros development (init node).assets/image-20211202221215364.png" alt="image-20211202221215364" style="zoom:65%;" /> | ![image-20211202193301493](20211130 - imu ros development (init node).assets/image-20211202193301493.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |



2021年12月3日：到目前为止，已经解决了：1. 丢包问题，2. rviz 显示问题。还需要解决解码问题。

<img src="20211130 - imu ros development (init node).assets/image-20211203122255241.png" alt="image-20211203122255241" style="zoom:30%;" align="left"/>



---



### 知识点补充

#### 00：cmake 常量

```cmake
set(DEVEL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/../../devel/include)  
# 使用间接法，.h 头引用依然有红色下划线（错误），再次验证，没有红色下划线了（:)）
# 直接法 /home/ds18/catkin_x/IMU_ws/devel/include 就没有红色下划线问题

message(STATUS "ROOT: ${ROOT}")
message(STATUS "catkin_INCLUDE_DIRS: ${catkin_INCLUDE_DIRS}")
message(STATUS "FileParentDir: ${FileParentDir}")

MESSAGE(STATUS "CMAKE_BINARY_DIR:" ${CMAKE_BINARY_DIR})
MESSAGE(STATUS "PROJECT_BINARY_DIR:" ${PROJECT_BINARY_DIR})
MESSAGE(STATUS "CMAKE_SOURCE_DIR:" ${CMAKE_SOURCE_DIR})
MESSAGE(STATUS "PROJECT_SOURCE_DIR:" ${PROJECT_SOURCE_DIR})
MESSAGE(STATUS "CMAKE_CURRENT_SOURCE_DIR:" ${CMAKE_CURRENT_SOURCE_DIR})
MESSAGE(STATUS "CMAKE_CURRENT_BINARY_DIR:" ${CMAKE_CURRENT_BINARY_DIR})
MESSAGE(STATUS "CMAKE_CURRENT_LIST_FILE:" ${CMAKE_CURRENT_LIST_FILE})
MESSAGE(STATUS "CMAKE_CURRENT_LIST_LINE:" ${CMAKE_CURRENT_LIST_LINE})

SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
MESSAGE(STATUS "CMAKE_MODULE_PATH:" ${CMAKE_MODULE_PATH})
MESSAGE(STATUS "EXECUTABLE_OUTPUT_PATH:" ${EXECUTABLE_OUTPUT_PATH})
MESSAGE(STATUS "LIBRARY_OUTPUT_PATH:" ${LIBRARY_OUTPUT_PATH})

MESSAGE(STATUS "CMAKE_ROOT: ${CMAKE_ROOT}")
MESSAGE(STATUS "CMAKE_FIND_ROOT: ${CMAKE_FIND_ROOT_PATH}")
MESSAGE(STATUS "PROJECT_BINARY_DIR: ${PROJECT_BINARY_DIR}")
MESSAGE(STATUS "PROJECT_SOURCE_DIR: ${PROJECT_SOURCE_DIR}")
MESSAGE(STATUS "DEVEL_DIR: ${DEVEL_DIR}")
```

```
-- ROOT: 
-- catkin_INCLUDE_DIRS: /opt/ros/melodic/include;/opt/ros/melodic/share/xmlrpcpp/cmake/../../../include/xmlrpcpp;/usr/include
-- FileParentDir: 
-- CMAKE_BINARY_DIR:/home/ds18/catkin_x/IMU_ws/src/imu_1129/cmake-build-debug
-- PROJECT_BINARY_DIR:/home/ds18/catkin_x/IMU_ws/src/imu_1129/cmake-build-debug
-- CMAKE_SOURCE_DIR:/home/ds18/catkin_x/IMU_ws/src/imu_1129
-- PROJECT_SOURCE_DIR:/home/ds18/catkin_x/IMU_ws/src/imu_1129
-- CMAKE_CURRENT_SOURCE_DIR:/home/ds18/catkin_x/IMU_ws/src/imu_1129
-- CMAKE_CURRENT_BINARY_DIR:/home/ds18/catkin_x/IMU_ws/src/imu_1129/cmake-build-debug
-- CMAKE_CURRENT_LIST_FILE:/home/ds18/catkin_x/IMU_ws/src/imu_1129/CMakeLists.txt
-- CMAKE_CURRENT_LIST_LINE:153
-- CMAKE_MODULE_PATH:/home/ds18/catkin_x/IMU_ws/src/imu_1129/cmake
-- EXECUTABLE_OUTPUT_PATH:
-- LIBRARY_OUTPUT_PATH:
-- CMAKE_ROOT: /home/ds18/Software/clion-2021.2.2/bin/cmake/linux/share/cmake-3.20
-- CMAKE_FIND_ROOT: 
-- PROJECT_BINARY_DIR: /home/ds18/catkin_x/IMU_ws/src/imu_1129/cmake-build-debug
-- PROJECT_SOURCE_DIR: /home/ds18/catkin_x/IMU_ws/src/imu_1129
-- DEVEL_DIR: /home/ds18/catkin_x/IMU_ws/src/imu_1129/../../devel
```

**clion** 在 `CMakeLists.txt`文件中可以使用 缺省 显示出 cmake 的常量。可以尝试使用关键词：`PROJECT, ROOT, CMAKE, SOURCE`。



#### 01：clion namespace 不缩进

<img src="20211130 - imu ros development (init node).assets/image-20211201215400496.png" alt="image-20211201215400496" style="zoom:40%;" align="left"/>



---



### 参考：

link00: [使用imu_tools对IMU进行滤波并可视化](https://blog.csdn.net/learning_tortosie/article/details/103189118)

link01: [ROS从入门到精通系列（前言）-- ROS官方教程内容概要](https://haowang.blog.csdn.net/article/details/90745221)

link02: [ROS从入门到精通系列博客](https://blog.csdn.net/hhaowang/category_11131544.html)

link03: [ROS自定义msg类型及使用 -- 【可以参考一下ros自建msg的流程】](https://blog.csdn.net/u013453604/article/details/72903398)

link04: [ROS串口通信（2）以十六进制指令读取IMU数据 -- 【继续昨天的ROS代码进行优化】](https://blog.csdn.net/fb_941219/article/details/84486603)

link05: [在ROS中与其他器件使用十六进制串口通信 -- 【可以看一下】](https://blog.csdn.net/chduan_10/article/details/76619179)

link06: [ROS从入门到精通系列（十六）-- IMU in ROS -- 【最早看的文章之一，继续】](https://blog.csdn.net/hhaowang/article/details/104961725)

link07: [ROS从入门到精通系列（二）ROS官方基础教程与实例笔记整理 -- 【link01同作者，一起看】](https://haowang.blog.csdn.net/article/details/102637625)

link08: [ROS下IMU串口通讯接口（通用版）-- 【很简单的代码，可以尝试一下】](https://blog.csdn.net/zhuoyueljl/article/details/75453808?utm_medium=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-5.control404&depth_1-utm_source=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-5.control40)

link09: [ROS学习——读取IMU数据 --【很标准的 ros imu 开发流程，稍微复杂，link08之后可以尝试一下】](https://blog.csdn.net/just_do_it567/article/details/107223025?utm_medium=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-4.control404&depth_1-utm_source=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-4.control40)

link10: [ros接入IMU数据，打包发布topic -- 【很标准的 ros imu 开发流程】](https://blog.csdn.net/xinmei4275/article/details/85040164?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

link11: [ROS传感器之IMU实践 -- 【讲概念文章，实用】](https://zhuanlan.zhihu.com/p/268565374)

link12: [从零搭建ROS机器人平台 -- 【小白机器人的文章，yyds 之一】](https://blog.csdn.net/zhao_ke_xue/category_10285721.html?spm=1001.2014.3001.5482)

link13: [ROS——下位机驱动节点：读取并发布单片机IMU、编码器速度数据（python）-- 【很标准的 ros imu 开发流程】](https://blog.csdn.net/qq_45779334/article/details/113563618)

link14: [iblus](https://github.com/iblus)/**[imu4ros -- 【git 代码，需要补充很多知识后再去使用】](https://github.com/iblus/imu4ros)**

link15: [3.使用串口读取IMU数据并通过话题发布 --【最最开始的 imu 资料】](https://www.corvin.cn/2274.html)

link16: [【奥特学园】ROS机器人入门课程《ROS理论与实践》零基础教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ci4y1L7ZZ?p=56&spm_id_from=pageDriver)

link17: [【奥特学园】ROS机器人入门课程《ROS理论与实践》资料 Introduction · GitBook (autolabor.com.cn)](http://www.autolabor.com.cn/book/ROSTutorials/)

link18: [**Setup CLion with ROS**](https://answers.ros.org/question/284786/setup-clion-with-ros/)

link19: [clion+ros开发环境的搭建 -- 【这种方式可以解决头文件报错问题】](https://blog.csdn.net/qq_40957243/article/details/118313690?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.no_search_link)

link20: [ROS setup tutorial -- 【官方的 clion + ros 环境配置】](https://www.jetbrains.com/help/clion/ros-setup-tutorial.html)

link21: [ROS中发布IMU传感器消息 --【包含了 ros imu_tools 的使用】](https://www.cnblogs.com/21207-iHome/p/7832355.html)



---

## 2021年11月30日

### 记录：

#### Task 00：解决 编译 bug：undefind reference to XXX

```
test_imu_serial.cpp:(.text+0x56b): undefined reference to `serial::Serial::open()'
test_imu_serial.cpp:(.text+0x577): undefined reference to `serial::Serial::isOpen() const'
test_imu_serial.cpp:(.text+0x737): undefined reference to `serial::Serial::available()'
test_imu_serial.cpp:(.text+0x8c8): undefined reference to `serial::Serial::available()'
test_imu_serial.cpp:(.text+0x8e1): undefined reference to make[1]: *** [imu_1129/CMakeFiles/test_imu_serial.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[100%] Linking CXX executable /home/ds18/catkin_x/IMU_ws/devel/lib/serial_msgs/serial_example_node1
[100%] Built target listen1
[100%] Built target serial_example_node1
Makefile:140: recipe for target 'all' failed
make: *** [all] Error 2
Invoking "make -j8 -l8" failed
```

**解决：**

在 CMakeLists.txt --》find_package 中添加：serial

```cmake
find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        sensor_msgs
        std_msgs
        serial  # rospack find serial: /opt/ros/melodic/share/serial
        )
```

对 rospack package 做详细的理解：
```
# ros package 的 .cmake 文件查找路径：

ds18@ubuntu:~/catkin_x/IMU_ws$ rospack find serial
/opt/ros/melodic/share/serial

ds18@ubuntu:~/catkin_x/IMU_ws$ cd /opt/ros/melodic/share/serial/

ds18@ubuntu:/opt/ros/melodic/share/serial$ ls
cmake  package.xml

ds18@ubuntu:/opt/ros/melodic/share/serial$ ls cmake/
serialConfig.cmake  serialConfig-version.cmake

# ros package 的 头文件 (.h) 查找路径：

ds18@ubuntu:/opt/ros/melodic/include/serial$ pwd
/opt/ros/melodic/include/serial

ds18@ubuntu:/opt/ros/melodic/include/serial$ ls
serial.h  v8stdint.h

```

编辑结果：

```
[ 95%] Linking CXX executable /home/ds18/catkin_x/IMU_ws/devel/lib/serial_msgs/listen1
[ 95%] Built target test_imu_serial
[100%] Linking CXX executable /home/ds18/catkin_x/IMU_ws/devel/lib/serial_msgs/talker
[100%] Built target serial_example_node1
[100%] Built target listen1
[100%] Built target talker
```

![image-20211130102750174](20211130 - imu ros development (init node).assets/image-20211130102750174.png)

#### Task 01：run `test_imu_serial.cpp`：虚拟机无法打开串口

```
/home/ds18/catkin_x/IMU_ws/src/cmake-build-debug/devel/lib/imu_1129/test_imu_serial
[ERROR] [1638236870.112381249]: Unable to open port 

Process finished with exit code 255
```

**解决1：可能需要在 ubuntu 虚拟机中使用*串口工具*读取数据。**

linux 串口权限问题，解决（参考 link04）：

> 1. 查看 linux 下的 usb 设备：
>
>    ```
>    lsusb				# 列出所有 usb 设备
>    ls -l /dev/tty*     # 列出 /dev 下 所有 tty*
>    ls -l /dev/ttyUSB*  # 列出 /dev 下 所有 ttyUSB*
>    ```
>
>    ```
>    ds18@ubuntu:~$ lsusb
>    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
>    Bus 002 Device 010: ID 1a86:7523 QinHeng Electronics HL-340 USB-Serial adapter
>    Bus 002 Device 005: ID 0e0f:0008 VMware, Inc. 
>    Bus 002 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
>    Bus 002 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
>    Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
>    ```
>
> 2. 查看串口是否在使用：
>
>    **在使用 .cpp，minicom，cutecom 读取串口数据时，如果出现`input/output error`，可以不用频繁拔插 IMU 串口，这可能是 os 释放串口延时导致的。解决办法是，等待一段时间，再次打开设备。在三种情况下验证可行。**
>
>    ```
>    ds18@ubuntu:~$ ls -l /dev/ttyUSB0
>    crwxrwxrwx 1 root dialout 188, 0 Nov 29 22:04 /dev/ttyUSB0
>    
>    ds18@ubuntu:~$ ls -l /dev/ttyUSB1
>    ls: cannot access '/dev/ttyUSB1': No such file or directory
>    ```
>
> 3. **查看串口设备**：(这个有点问题，为什么是 failed to read modem？链接中就没有这个问题)
>
>    Daiwei 发的关于 ch341 的驱动代码：
>
>     <img src="20211130 - imu ros development (init node).assets/image-20211130153343732.png" alt="image-20211130153343732" style="zoom:50%;" />
>
>    ```
>    ds18@ubuntu:~$ dmesg | grep ttyUSB0
>    [19421.411925] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [20205.222915] ch341-uart ttyUSB0: failed to read modem status: -110
>    [20253.835193] ch341-uart ttyUSB0: failed to read modem status: -110
>    [21427.697504] ch341-uart ttyUSB0: failed to read modem status: -110
>    [21474.753316] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27381.306641] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27436.364640] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27446.369464] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27526.409866] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27535.441496] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27551.450303] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27594.472664] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27609.481462] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27796.586239] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27804.453665] ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
>    [27815.069697] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [28055.839581] ch341-uart ttyUSB0: failed to read modem status: -110
>    [28065.871445] ch341-uart ttyUSB0: failed to read modem status: -110
>    [28142.336373] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29448.633392] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29593.486393] ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
>    [29599.910891] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [29816.973696] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29855.022383] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29927.812395] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30036.908861] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30117.479713] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30304.616999] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30311.653111] ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
>    [30317.861884] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [30534.016853] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30551.054556] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30645.141575] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30654.173933] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30888.338850] ch341-uart ttyUSB0: failed to read modem status: -110
>    [31124.980341] ch341-uart ttyUSB0: failed to read modem status: -110
>    [31565.256791] ch341-uart ttyUSB0: failed to read modem status: -110
>    [31737.109282] ch341-uart ttyUSB0: failed to read modem status: -110
>    [32312.387312] ch341-uart ttyUSB0: failed to read modem status: -110
>    [32400.464474] ch341-uart ttyUSB0: failed to read modem status: -110
>    ```
>
> 4. 串口权限问题：
>
>    每次开机后对串口读写都要附加权限（串口默认普通用户没有读写权限）。
>
>    **临时权限修改：**
>
>    ```bash
>    sudo chmod 666 /dev/ttyUSB0
>             
>    ds18@ubuntu:~$ ll /dev/ttyUSB0
>    crwxrwxrwx 1 root dialout 188, 0 Nov 30 00:08 /dev/ttyUSB0
>    ```
>
>    **永久权限修改：**
>
>    创建 70-ttyusb-serial.rules 文件。这个文件本来是不存在的，只不过编辑器打开不存在的文件会自动创建。70-usb-serial.rules 文件名可以自定义，但必须以.rules结尾。
>
>    ttyUSB*表示所有这一格式的串口名，如果你的是ttyS*或其它，按需改。
>    0666表示加权模式，和chmod后面的参数一致，写成666也可以。
>    vibot_base是我自定义的串口名，就是为ttyUSB*创建一个超链接 ，如下图。**如果你不需要，可以去掉最后一项。**
>
>    ```
>    sudo gedit /etc/udev/rules.d/70-ttyusb.rules
>             
>    KERNEL=="ttyUSB*", MODE="0666", SYMLINK+="vibot_base"
>    ```



**尝试了两种串口工具**

1. minicom
2. cutecom

个人认为 cutecom 更好用，因为它有图形界面。

* minicom 使用总结：

  常用命令：`minicom -c on -H` ascii 颜色打开，使用 hex 模式读取。使用`ctrl + z, x`推出。

  ```
  # 读取失败 /dev/ttyUSB0 IMU 串口没有释放
  
  ds18@ubuntu:~$ minicom -c on -H
  
  minicom: cannot open /dev/ttyUSB0: Input/output error
  
  =====================================================================
  
  # 成功读取 /dev/ttyUSB0 的 IMU 数据
  
  ds18@ubuntu:~$ minicom -c on -H
  
  Welcome to minicom 2.7.1
  
  OPTIONS: I18n 
  Compiled on Aug 13 2017, 15:25:34.
  Port /dev/ttyUSB0, 03:43:53
  
  Press CTRL-A Z for help on special keys
  
  f3 fc ff ff ff f9 ff e1 ff 52 00 c4 ff 66 26 4b 7f 80 20 00 02 00 
  ```

  

* cutecom 使用总结：

  使用`cutecom`，打开`/dev/ttyUSB0`读取失败。 ![image-20211130200850655](20211130 - imu ros development (init node).assets/image-20211130200850655.png)

   打开，读取成功（需要等待 os 释放端口句柄？）。![image-20211130201054859](20211130 - imu ros development (init node).assets/image-20211130201054859.png)



#### Task 02：关于 task 01 串口设备占用问题的一些解决办法

现在有两个设备需要使用 imu 数据，也就是要占用 /dev/ttyUSB0。会出现的情况：当一个设备结束使用 imu，释放 /dev/ttyUSB0，会有释放延时（系统调度问题）。 

当 ros: test_imu_serial.cpp 结束进程，minicom 立刻读取 /dev/ttyUSB0 中的数据，会出现：

```
ds18@ubuntu:~$ minicom -c on -H
minicom: cannot open /dev/ttyUSB0: Input/output error
```



一种可能的找到 /dev/ttyUSB0 被哪一个 process 占用的理想流程：

假设 ros: test_imu_serial.cpp 正在占用 /dev/ttyUSB0。使用`top | grep test*` 查看进程 pid：

```
ds18@ubuntu:~$ top | grep test*
 27192 ds18      20   0  404092  10652   9700 S   1.7  0.1   0:00.96 test_imu_serial     
 27192 ds18      20   0  404092  10652   9700 S   1.3  0.1   0:01.00 test_imu_serial     
 27192 ds18      20   0  404092  10652   9700 S   2.0  0.1   0:01.06 test_imu_serial     
 27192 ds18      20   0  404092  10652   9700 S   1.7  0.1   0:01.11 test_imu_serial     
```

找到 占用的 pid（27192）后，进入`/proc/27192` ：![image-20211130162325943](20211130 - imu ros development (init node).assets/image-20211130162325943.png)

进入`/fd`目录，并查看所有文件： ![image-20211130162535171](20211130 - imu ros development (init node).assets/image-20211130162535171.png)

```
ds18@ubuntu:/proc/27192$ cd fd
ds18@ubuntu:/proc/27192/fd$ ls
0  1  10  12  2  3  4  5  6  7  8  9
ds18@ubuntu:/proc/27192/fd$ ls -l
total 0
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 0 -> /dev/pts/5
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 1 -> /dev/pts/5
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 10 -> /dev/ttyUSB0
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 12 -> 'socket:[428757]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 2 -> /dev/pts/9
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 3 -> 'socket:[432533]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 4 -> 'anon_inode:[eventpoll]'
lr-x------ 1 ds18 ds18 64 Nov 29 23:51 5 -> 'pipe:[432537]'
l-wx------ 1 ds18 ds18 64 Nov 29 23:51 6 -> 'pipe:[432537]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 7 -> 'socket:[432542]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 8 -> 'socket:[432545]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 9 -> 'socket:[432546]'
ds18@ubuntu:/proc/27192/fd$ 

```

可以看到 `/fd`目录下的`10 -> /dev/ttyUSB0`就是占用的端口，说明此时`/dev/ttyUSB0`被 pid=27192占用，如果想快速释放，可以使用`kill -9 27192`。

不过有一个问题需要注意，上面的解决办法是在知道执行程序的前提下进行的。如果我们不知道是哪一个执行程序在占用端口，那么就很复杂了。

再来一个例子：minicom 已经再使用`/dev/ttyUSB0`读取 imu 数据。我们查看其 pid 并关闭它，然后再用`test_imu_serial.cpp`读取 imu 数据。 ![image-20211130165923204](20211130 - imu ros development (init node).assets/image-20211130165923204.png)

从下至上：

```shell
# 4. minicom 显示的 imu hex 数据

# 3. 使用 top 查看 minicom pid

# 1. 使用 ps -a | grep mini 查看 minicom pid (27504)

# 2. 在 /proc/27504/fd 下找到 3 -> /dev/ttyUSB0

# 0. 使用 kill -9 27504 杀掉进程 27504

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

# * 运行 ros: test_imu_serial.cpp 显示 端口无法打开：

/home/ds18/catkin_x/IMU_ws/src/cmake-build-debug/devel/lib/imu_1129/test_imu_serial
[ERROR] [1638262911.528352652]: Unable to open port 

Process finished with exit code 255

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

# * 再次运行 ros: test_imu_serial.cpp 可以正常运行：

/home/ds18/catkin_x/IMU_ws/src/cmake-build-debug/devel/lib/imu_1129/test_imu_serial
[ INFO] [1638262996.610282856]: Serial Port initialized
[ INFO] [1638262996.611204440]: Reading from serial port

[ INFO] [1638262996.611257561]: Read: ?
[ INFO] [1638262996.631940408]: Reading from serial port

[ INFO] [1638262996.632009695]: Read: ?"
[ INFO] [1638262996.651928790]: Reading from serial port

[ INFO] [1638262996.651994743]: Read: ?
[ INFO] [1638262996.671842466]: Reading from serial port
```

可行的解决办法：

**在使用 .cpp，minicom，cutecom 读取串口数据时，如果出现`input/output error`，可以不用频繁拔插 IMU 串口，这可能是 os 释放串口延时导致的。解决办法是，等待一段时间，再次打开设备。在三种情况下验证可行。**



#### 知识补充：

##### 00: rospack serial: v8stdint.h 中类型整理

```cpp
#ifndef V8STDINT_H_  // 防止头文件被重复引用
#define V8STDINT_H_

#include <stddef.h>
#include <stdio.h>

#if defined(_WIN32) && !defined(__MINGW32__)

typedef signed char int8_t;					// 1字节，范围：-128 到 127(有符号位)
typedef unsigned char uint8_t;				// 1字节，范围： 0 到 255
typedef short int16_t;  // NOLINT			// 2字节，范围：-32,768 到 32,767
typedef unsigned short uint16_t;  // NOLINT // 2字节，范围：0 到 65,535
typedef int int32_t;              // 2或4字节，范围：-32,768 到 32,767 或 ...	
typedef unsigned int uint32_t;	  // 2或4字节，范围：0 到 65,535 或 0 到 4,294,967,295
typedef __int64 int64_t;					// 未知 __int64
typedef unsigned __int64 uint64_t;			// 未知 __int64
// intptr_t and friends are defined in crtdefs.h through stdio.h.

#else

#include <stdint.h>

#endif  // _WIN32 && !__MINGW32__

#endif  // V8STDINT_H_
```



关于：typedef

> typedef 为类型取一个新名字
>
> ```
> typedef unsigned char BYTE;  // 单字节数字定义了一个术语 BYTE
> BYTE  b1, b2;                // 标识符 BYTE 可作为类型 unsigned char 的缩写
> ```

关于：C 数据类型（菜鸟C）

**整数类型：**

![image-20211130104017894](20211130 - imu ros development (init node).assets/image-20211130104017894.png)

**浮点类型：**

![image-20211130104117639](20211130 - imu ros development (init node).assets/image-20211130104117639.png)

**void 类型：**

![image-20211130104210755](20211130 - imu ros development (init node).assets/image-20211130104210755.png)



#### 01：c99 中的 uxxx_t 类型：

`uint8_t，uint_16_t，uint32_t，uint64_t` 类型

> 1、这些类型的来源：这些数据类型中都带有`_t`, `_t` 表示这些数据类型是通过`typedef`定义的，而不是新的数据类型。也就是说，它们其实是我们已知的类型的别名。
>
> 2、使用这些类型的原因：方便代码的维护。比如，在C中没有`bool`型，于是在一个软件中，一个程序员使用`int`，一个程序员使用`short`，会比较混乱。最好用一个`typedef`来定义一个统一的`bool`：`typedef char bool;`。
>
> 3、在涉及到跨平台时，不同的平台会有不同的字长，所以利用预编译和typedef可以方便的维护代码。
>
> 4、在C99标准中定义了这些数据类型，具体定义在：`/usr/include/stdint.h`

```c++
# ifndef __int8_t_defined  
# define __int8_t_defined  
typedef signed char             int8_t;   
typedef short int               int16_t;  
typedef int                     int32_t;  
# if __WORDSIZE == 64  
typedef long int                int64_t;  
# else  
__extension__  
typedef long long int           int64_t;  
# endif  
# endif  
  
  
typedef unsigned char           uint8_t;  
typedef unsigned short int      uint16_t;  
# ifndef __uint32_t_defined  
typedef unsigned int            uint32_t;  
# define __uint32_t_defined  
#endif  
# if __WORDSIZE == 64  
typedef unsigned long int       uint64_t;  
#else  
__extension__  
typedef unsigned long long int  uint64_t;  
# endif  
```



#### 02：link12：typeid 的使用

<img src="20211130 - imu ros development (init node).assets/image-20211202154336721.png" alt="image-20211202154336721" style="zoom:35%;" align="left"/>









---



### 参考：

link00: [**undefined reference in using serial.h**](https://answers.ros.org/question/320280/undefined-reference-in-using-serialh/)

link01: [ifndef define endif的作用](https://blog.csdn.net/bad_good_man/article/details/38682033?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

link02: [ROS串口通信（1）环境搭建](https://blog.csdn.net/fb_941219/article/details/84481689)

link03: [ROS串口通信（2）以十六进制指令读取IMU数据](https://blog.csdn.net/fb_941219/article/details/84486603?utm_medium=distribute.wap_feed_404.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.nonecase&depth_1-utm_source=distribute.wap_feed_404.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.nonecase)

link04: [Ubuntu关于串口的操作(查看串口信息、串口助手、串口权限)](https://blog.csdn.net/maizousidemao/article/details/103236666)

link05: [让Typora的图片居左的两种方法](https://blog.csdn.net/sky0816/article/details/111321812)

link06: [How to restart ttyusb* -- (验证不成功)](https://superuser.com/questions/572034/how-to-restart-ttyusb)

link07: [Linux查看端口占用情况,并强制释放占用的端口 -- (验证不成功)](https://developer.aliyun.com/article/481709)

link08: [解决Linux非root用户读写串口权限问题](https://blog.csdn.net/itas109/article/details/83027431)

link09: [浅析C语言之uint8_t / uint16_t / uint32_t /uint64_t](https://blog.csdn.net/Mary19920410/article/details/71518130)

link10: [std::cout 输出 unsigned char类型数据](https://blog.csdn.net/weixin_43851636/article/details/110552887)

link11: [C++中二进制、字符串、十六进制、十进制之间的转换 --【这文章太有用了】](https://blog.csdn.net/MOU_IT/article/details/89060249)

link12: [[C++] 使用 typeid() 確認變數資料型態](https://clay-atlas.com/blog/2021/05/11/cpp-cn-typeid-check-variable-data-type/)

link13: [std::dec, std::hex, std::oct](https://en.cppreference.com/w/cpp/io/manip/hex)



---



