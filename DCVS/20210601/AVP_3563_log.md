# AVP_3563[root]_log

[toc]



---

注：以时间线记录 AVP 的开发，更新顺序由上至下（由新到旧）



---

temp task 20210601：将document上传到 github 或 gitlab



---

**模板**

## YYYYMMDD -- template

### 结果 
$==================================================$



### 记录 
$==================================================$



---


## 20210605 

### 结果

$==================================================$





### 记录 

$==================================================$

链接: [ros-Launch启动文件的使用方法](https://blog.csdn.net/weixin_45839124/article/details/106110136)

3

![image-20210605170413688](/home/ds16v2/.config/Typora/typora-user-images/image-20210605170413688.png)



---

## 20210601

### 结果 
$==================================================$



### 记录 
$==================================================$

#### ros work-flow

问题 1：

![image-20210601131925089](/home/ds16v2/.config/Typora/typora-user-images/image-20210601131925089.png)

```
This warning is for project developers.  Use -Wno-dev to suppress it.

CMake Error at CMakeLists.txt:65 (message):
  find_package(catkin) failed.  catkin was neither found in the workspace nor
  in the CMAKE_PREFIX_PATH.  One reason may be that no ROS setup.sh was
  sourced before.


-- Configuring incomplete, errors occurred!
See also "/home/ds16v2/catkin/src/cmake-build-debug/CMakeFiles/CMakeOutput.log".
```



解决办法

```
source /opt/ros/kinetic/setup.bash
source devel/setup.bash

catkin_make -DBUILD_WITH_ROS=ON -DCMAKE_BUILD_TYPE=Release
```

![image-20210601170508496](/home/ds16v2/.config/Typora/typora-user-images/image-20210601170508496.png)

然后打开项目，在 `CMakeLists.txt`上右键 --> Reload cmake project。解决问题。或者 File --> Reload CMake project。

解决链接: [加载cmakelist的一个问题：find_package(catkin) failed.](https://blog.csdn.net/Alex_wise/article/details/105201687)

解决链接: [Setting up ROS package in CLion](https://stackoverflow.com/questions/33172132/setting-up-ros-package-in-clion)

解决链接: [Clion编译catkin_ws（ROS工作空间包的简称)加载CMakeLists.txt出现的问题](https://blog.csdn.net/qq_37611824/article/details/107792673?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242)

解决链接: []()



terminator broadcast shortcut:

![image-20210601171112451](/home/ds16v2/.config/Typora/typora-user-images/image-20210601171112451.png)



#### non-ros work-flow





---

## 20210531 

### 结果 
$==================================================$

none

### 结果 
$==================================================$

成果：完成了一次 pc 模拟：3563-89

关联：

* 3563-88 --> 3729

* 3563-64 关于start pose 的 yaml 配置 

结果：start pose 有问题，已问 ryan，未答复。

原因：地图问题，代码问题，yaml 问题



| 3563-88                                                      | 3563-68                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![Figure_1](/home/ds16v2/Work/TS_PC/3563_20210317/pc/dvr_0310_1/plots_pc/Figure_1.png) | ![Figure_2](/home/ds16v2/Work/TS_PC/3563_20210317/pc/dvr_0310_1/plots_pc/Figure_2.png) |
|                                                              |                                                              |



```
      - key: SetPose
        pose: { x: -25.9, y: -36.6, theta: -3.14 }
        relative_position: 0
        output: start_pose_layer
      - key: SetPose
        # ignore: 1
        pose: { x: -41.5, y: -37.3, theta: -3.14 }
        relative_position: 0
        output: goal_pose_layer
```



3563-64

根使用的 yaml 中的配置一样，依然无法解决 start pose 0 0 问题

![image-20210601110613382](/home/ds16v2/.config/Typora/typora-user-images/image-20210601110613382.png)