# 2021年08月21日：參考相關資料，進行建圖

---

[toc]



---

日誌：

2021年08月23日：繼續 0821 閱讀完 cartographer doc. official。（linux 虛擬機）思考如何將 ros 無干擾地安裝到服務器上。這對以後的仿真很有用。

​	額外瞭解：layer 是一個 map container <key: value>，每一個執行類通過輸入 input layer key，尋找對應的 value（正確的格式），作爲程序的參數，然後將計算結果通過（全局變量的引用，指針或 layer）傳遞出去。mpc，soc？

​	看完：【cartographer】纯定位 纯本地化 pure_localization，對 cartographer 有了一點新認識，cartographer 包含 建圖 與 定位功能，現在需要熟練單獨操作 建圖 + 定位。(done)

​	接着 看 google 的 cartographer documentation。

2021年08月24日：今天建圖，離線，在線。



---

## 0. **置頂：cartographer 中文資料**

參考：[【移动机器人技术】Cartographer使用流程-建图-纯定位-导航](https://blog.csdn.net/weixin_43259286/article/details/105143605?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control#t12)

參考：[Cartographer用于机器人纯定位](https://blog.csdn.net/weixin_36976685/article/details/103834608)

參考：[Cartographer的纯定位模式在低性能处理器上的应用适配](https://blog.csdn.net/zhzwang/article/details/110197647)

參考：[第四讲 【cartographer】纯定位 纯本地化 pure_localization](https://blog.csdn.net/wesigj/article/details/111334726)

參考：[《机器人操作系统入门》课程代码示例](https://github.com/sychaichangkun/ROS-Academy-for-Beginners)

參考：[Cartographer编译方法及编译出错（glog库链接错误）解决方法](https://blog.csdn.net/mxdsdo09/article/details/103172574)

參考：[google激光雷达slam算法Cartographer的安装及bag包demo测试](https://community.bwbot.org/topic/136/google%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BEslam%E7%AE%97%E6%B3%95cartographer%E7%9A%84%E5%AE%89%E8%A3%85%E5%8F%8Abag%E5%8C%85demo%E6%B5%8B%E8%AF%95)

参考：[cartographer经典版本快速安装](https://zhuanlan.zhihu.com/p/374669589)

参考：[Cartographer编译安装 2020年12月31日亲测通过](https://blog.csdn.net/qq_41807801/article/details/112007868)

参考：[1.3 ROS-找不到launch文件的解决办法](https://zhuanlan.zhihu.com/p/94971196)







---

## 1. rosbag 建圖：bluewhale rosbag

參考：[谷歌cartographer使用速腾聚创3d激光雷达数据进行三维建图](https://community.bwbot.org/topic/523/%E8%B0%B7%E6%AD%8Ccartographer%E4%BD%BF%E7%94%A8%E9%80%9F%E8%85%BE%E8%81%9A%E5%88%9B3d%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E6%95%B0%E6%8D%AE%E8%BF%9B%E8%A1%8C%E4%B8%89%E7%BB%B4%E5%BB%BA%E5%9B%BE)

參考：[BluewhaleRobot](https://github.com/BluewhaleRobot)/**[cartographer_ros](https://github.com/BluewhaleRobot/cartographer_ros)**



**review stable cartographer commit:**

![image-20210824153832519](20210821_slam_cartographer_mapping.assets/image-20210824153832519.png)

![image-20210824153949517](20210821_slam_cartographer_mapping.assets/image-20210824153949517.png)

```
ds16v2@ds16v2:~/catkin_x/cartographer_ws/src$ tree -L 2 .
.
├── cartographer
│   ├── AUTHORS
│   ├── azure-pipelines.yml
│   ├── bazel
│   ├── BUILD.bazel
│   ├── cartographer
│   ├── cartographer-config.cmake.in
│   ├── CHANGELOG.rst
│   ├── cmake
│   ├── CMakeLists.txt
│   ├── configuration_files
│   ├── CONTRIBUTING.md
│   ├── Dockerfile.bionic
│   ├── Dockerfile.buster
│   ├── Dockerfile.focal
│   ├── Dockerfile.stretch
│   ├── Dockerfile.trusty.bazel
│   ├── docs
│   ├── LICENSE
│   ├── package.xml
│   ├── README.rst
│   ├── RELEASING.rst
│   ├── scripts
│   └── WORKSPACE
├── cartographer_ros
│   ├── AUTHORS
│   ├── azure-pipelines.yml
│   ├── cartographer_ros
│   ├── cartographer_ros_msgs
│   ├── cartographer_ros.rosinstall
│   ├── cartographer_rviz
│   ├── CONTRIBUTING.md
│   ├── Dockerfile.kinetic
│   ├── Dockerfile.melodic
│   ├── Dockerfile.noetic
│   ├── docs
│   ├── LICENSE
│   ├── README.rst
│   └── scripts
└── ceres-solver
    ├── build
    ├── cmake
    ├── CMakeLists.txt
    ├── config
    ├── data
    ├── docs
    ├── examples
    ├── git_backup
    ├── include
    ├── internal
    ├── jni
    ├── LICENSE
    ├── package.xml
    ├── README.md
    └── scripts

```



| s.no. | package_name     |                       | commit no.                               | date                                  |
| ----- | ---------------- | --------------------- | ---------------------------------------- | ------------------------------------- |
| 1     | cartographer     |                       | b8228ee6564f5a7ad0d6d0b9a30516521cff2ee9 | Date:   Fri May 7 15:52:57 2021 +0200 |
| 2     | cartographer_ros |                       | c06879b63567a78ef92b7d1fa79453839e36ffec | Date:   Tue May 4 13:43:20 2021 +0200 |
| 2.1   |                  | cartographer_ros      | c06879b63567a78ef92b7d1fa79453839e36ffec | Date:   Tue May 4 13:43:20 2021 +0200 |
| 2.2   |                  | cartographer_ros_msgs | c06879b63567a78ef92b7d1fa79453839e36ffec | Date:   Tue May 4 13:43:20 2021 +0200 |
| 2.3   |                  | cartographer_rviz     | c06879b63567a78ef92b7d1fa79453839e36ffec | Date:   Tue May 4 13:43:20 2021 +0200 |
| 3     | ceres-solver     |                       | 19333b0f55c8462381038e70d42af43b52941128 | Date:   Thu Aug 3 00:09:36 2017 -0700 |



GitHub 中查看 commit no.：

![image-20210824165206574](20210821_slam_cartographer_mapping.assets/image-20210824165206574.png)

刚才看了 commit，当前的 cartographer_ros 需要的 ceres-solver 版本是：19333b0f55c8462381038e70d42af43b52941128。

也就是说要使 cartographer，cartographer_ros 编译通过的话，需要选对版本。



**错误：**

使用 clion 打开 cartographer_ros 项目的时候（CMakelists.txt），会出现错误：

![image-20210824193539561](20210821_slam_cartographer_mapping.assets/image-20210824193539561.png)

**原因：**

可以从 functions.cmake 文件 `109-126` 中看出，如果使用 Debug 模式构建构建代码，需要以 force_debug 模式构建（110），否则会进入 else 中并报错；优先使用 release 模式。有两种解决方式：

**解决方式1：**

在 functions.cmake 文件中指定构建方式：

```
set(CMAKE_BUILD_TYPE "Release")
```

**解决方式2：**

修改 clion 默认的 cmake 构建方式：

> file --> settings --> build, execution, deployment --> cmake --> profiles --> enable profile --> build type --> release

然后 reload cmake，没有报错：

![image-20210824195523741](20210821_slam_cartographer_mapping.assets/image-20210824195523741.png)

![image-20210824195416744](20210821_slam_cartographer_mapping.assets/image-20210824195416744.png)

![image-20210824195444446](20210821_slam_cartographer_mapping.assets/image-20210824195444446.png)

从 上面的 输出 可以看见很多的库版本信息。

是否可以重复编译当前的 cartographer_ros 版本？

参考：[cmake编译Debug和Release版本的注意点](https://blog.csdn.net/Felaim/article/details/72852994)





**roscore error:**

```
roscore cannot run as another roscore/master is already running. 
Please kill other roscore/master processes before relaunching.
The ROS_MASTER_URI is http://ds16v2:11311/
The traceback for the exception was written to the log file
```

解决：

```
killall -9 roscore
killall -9 rosmaster
```





---

完成建图：

![image-20210824214416943](20210821_slam_cartographer_mapping.assets/image-20210824214416943.png)



3d_pcd

![image-20210824214501847](20210821_slam_cartographer_mapping.assets/image-20210824214501847.png)

![image-20210824214553109](20210821_slam_cartographer_mapping.assets/image-20210824214553109.png)





---



## 2. rosbag 建图：bluewhale rosbag 记录 2

记录包括：

1. 命令

2. console输出

3. 程序输出（rviz，文件）



主要参考资料：

1. [谷歌cartographer使用速腾聚创3d激光雷达数据进行三维建图<第一参考资料>](https://community.bwbot.org/topic/523/%E8%B0%B7%E6%AD%8Ccartographer%E4%BD%BF%E7%94%A8%E9%80%9F%E8%85%BE%E8%81%9A%E5%88%9B3d%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E6%95%B0%E6%8D%AE%E8%BF%9B%E8%A1%8C%E4%B8%89%E7%BB%B4%E5%BB%BA%E5%9B%BE)
2. [谷歌cartographer使用速腾聚创3d激光雷达转二维数据进行2d建图<第二参考资料，创建导航图map>](https://community.bwbot.org/topic/525/%E8%B0%B7%E6%AD%8Ccartographer%E4%BD%BF%E7%94%A8%E9%80%9F%E8%85%BE%E8%81%9A%E5%88%9B3d%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E8%BD%AC%E4%BA%8C%E7%BB%B4%E6%95%B0%E6%8D%AE%E8%BF%9B%E8%A1%8C2d%E5%BB%BA%E5%9B%BE)
3. [cartographer构建地图后保存地图的方法](https://blog.csdn.net/ABC_ORANGE/article/details/98945180)
4. [ROS坐标系统，常见的坐标系和其含义](https://community.bwbot.org/topic/227/ros%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F-%E5%B8%B8%E8%A7%81%E7%9A%84%E5%9D%90%E6%A0%87%E7%B3%BB%E5%92%8C%E5%85%B6%E5%90%AB%E4%B9%89)
5. [Exploiting the map generated by Cartographer ROS](https://google-cartographer-ros.readthedocs.io/en/latest/assets_writer.html)
6. [pcl_viewer 及程序中 改变点云显示的背景](https://blog.csdn.net/sunshinefcx/article/details/105920731?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.base&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.base)
7. [PCL可视化点云颜色特征](https://blog.csdn.net/YunLaowang/article/details/87797967?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-5.base&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-5.base)
8. [PCL学习之点云可视化：坐标字段、随机、单一颜色、法向量<通过代码实现点云的不同颜色显示>](https://blog.csdn.net/sunlin972913894/article/details/101193045)
9. [pcl 官方](https://www.pclcn.org/study/news.php?lang=cn&class1=85&class2=102&class3=105&page=1)
10. [PCL-01-点云文件读取、上色、可视化类和矩阵变换_浩浩爱学习_的博客-程序员宅基地<推荐，pcl基本操作+基本变换>](https://www.cxyzjd.com/article/weixin_45348389/115484865)



### 1. 数据包：

网址：[百度网盘](https://pan.baidu.com/s/1rBWFRS90lB9_3_OTLUaCxA)

名称：2018-08-11-13-20-34.bag (15.8 GB)

### 2. 相关文件：

蓝鲸机器人 cartographer_ros 网址：[BluewhaleRobot](https://github.com/BluewhaleRobot)/**[cartographer_ros](https://github.com/BluewhaleRobot/cartographer_ros)**

运行脚本文件（for rosbag）：

```
# 建图时启动的launch文件，负责启动xiaoqiang_3d.launch文件和rviz
cartographer_ros/launch/demo_xiaoqiang_3d.launch 

# 用来加载cartographer_ros主要的启动节点和参数文件，话题数据名字的remap也在这个文件设定。
cartographer_ros/launch/xiaoqiang_3d.launch 

# 用来将demo_xiaoqiang_3d.launch输出的pbstreamfile转换成ply点云数据
cartographer_ros/launch/assets_writer_xiaoqiang_3d.launch 

# 模型文件，用来发布3d激光雷达、小车里程计、IMU、小车本体之间的tf关系
cartographer_ros/urdf/rslidar_2d.urdf 

# cartographer_ros算法参数配置文件，优化建图效果需要调整的参数就是这个文件。
cartographer_ros/configuration_files/xiaoqiang_3d.lua 
```

脚本文件概览：

**cartographer_ros/launch/demo_xiaoqiang_3d.launch** 

```xml
<!--
  Copyright 2016 The Cartographer Authors

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

<launch>
  <param name="/use_sim_time" value="true" />

  <include file="$(find cartographer_ros)/launch/xiaoqiang_3d.launch" />
  <node name="rviz" pkg="rviz" type="rviz" required="true"
      args="-d $(find cartographer_ros)/configuration_files/rslidar_3d.rviz" />
</launch>

```

**launch:** 

**param:** 定义参数 `/use_sim_time = true`<key: value> 到参数服务器。param 在 ros 中是 节点间的共享数据，将这个共享数据存放在 ros master 中，这样所有的节点都可以访问。

**include:**  包含 xiaoqiang_3d.launch（另一个 launch 文件）。

**node:** 启动 rviz node（ros 中，节点《node》是最小的进程单元；一个软件包中可以有多个可执行文件，每个可执行文件在运行之后成为一个进程《process》，这个进程在 ros 中就是 节点《node》）。



补充：node 中常用属性：

| node |               |                                                              |
| ---- | ------------- | ------------------------------------------------------------ |
| 1    | pkg           | 节点包                                                       |
| 2    | type          | 节点类型                                                     |
| 3    | name          | 节点名称                                                     |
| 4    | args          | 将参数传递给节点【可以多个参数】                             |
| 5    | machine       | 指定机器上启动节点                                           |
| 6    | respawn       | 如果退出，则自动重新启动该节点【默认：false】                |
| 7    | respawn_delay | 如果respawn为true，请在检测到节点故障之后等待respawn_delay的时间（单位s），然后再尝试重新启动。【默认：0】 |
| 8    | required      | 如果节点死亡，则杀死整个roslaunch                            |
| 9    | ns            | 如果ns=“foo”，在“ foo”名称空间中启动节点                     |
| 10   | output        | 如果为“ screen”，则来自该节点的stdout / stderr将被发送到屏幕。如果为'log'，则stdout / stderr输出将发送到$ ROS_HOME / log中的日志文件，并且stderr将继续发送到屏幕。【默认：“ log”】 |



cartographer_ros/launch/xiaoqiang_3d.launch 

```xml
<!--
  Copyright 2016 The Cartographer Authors

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

<launch>
  <param name="robot_description"
    textfile="$(find cartographer_ros)/urdf/rslidar_2d.urdf" />

  <node name="robot_state_publisher" pkg="robot_state_publisher"
    type="robot_state_publisher" />

  <!-- <node pkg="tf" type="static_transform_publisher" name="imulink_broadcaster" args="0.07 -0.03 0 0 0 0 1 laserbase_link imu 50"/> -->

  <node name="cartographer_node" pkg="cartographer_ros"
      type="cartographer_node" args="
          -configuration_directory $(find cartographer_ros)/configuration_files
          -configuration_basename xiaoqiang_3d.lua"
      output="screen">
      <remap from="/odom" to="/xqserial_server/Odom" />
      <remap from="/imu" to="/xqserial_server/IMU" />
      <remap from="/points2" to="/rslidar_points" />
  </node>

  <node name="cartographer_occupancy_grid_node" pkg="cartographer_ros"
      type="cartographer_occupancy_grid_node" args="-resolution 0.05" />
</launch>

```



cartographer_ros/launch/assets_writer_xiaoqiang_3d.launch

```xml
<!--
  Copyright 2016 The Cartographer Authors

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

<launch>
  <node name="cartographer_assets_writer" pkg="cartographer_ros" required="true"
      type="cartographer_assets_writer" args="
          -configuration_directory $(find cartographer_ros)/configuration_files
          -configuration_basename assets_writer_xiaoqiang_3d.lua
          -urdf_filename $(find cartographer_ros)/urdf/rslidar_2d.urdf
          -bag_filenames $(arg bag_filenames)
          -pose_graph_filename $(arg pose_graph_filename)"
      output="screen">
  </node>
</launch>

```



cartographer_ros/urdf/rslidar_2d.urdf 

```xml
<robot
  name="xiaoqiang_udrf">
   <link
    name="laserbase_link">
    <inertial>
      <origin
        xyz="-0.1098 -0.0015527 0.0081894"
        rpy="0 0 0" />
      <mass
        value="4.3093" />
      <inertia
        ixx="0.024068"
        ixy="0.00014219"
        ixz="-0.0052989"
        iyy="0.052449"
        iyz="-1.328E-05"
        izz="0.070783" />
    </inertial>
    <visual>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/base_link.STL" />
      </geometry>
      <material
        name="">
        <color
          rgba="0.098039 0.098039 0.098039 1" />
      </material>
    </visual>
    <collision>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/base_link.STL" />
      </geometry>
    </collision>
  </link>

  <link name="base_footprint"/>

  <joint name="base_link_joint" type="fixed">
    <parent link="base_footprint" />
    <child link="laserbase_link" />
    <origin xyz="0 0 0.15" />
  </joint>

  <link
    name="right_wheel">
    <inertial>
      <origin
        xyz="1.2074E-15 5.2042E-16 0.040256"
        rpy="0 0 0" />
      <mass
        value="0.4651" />
      <inertia
        ixx="0.00057862"
        ixy="-9.5729E-21"
        ixz="-1.1736E-18"
        iyy="0.00057862"
        iyz="3.9119E-21"
        izz="0.00090953" />
    </inertial>
    <visual>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/right_wheel.STL" />
      </geometry>
      <material
        name="">
        <color
          rgba="0.29412 0.29412 0.29412 1" />
      </material>
    </visual>
    <collision>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/right_wheel.STL" />
      </geometry>
    </collision>
  </link>
  <joint
    name="rot1"
    type="fixed">
    <origin
      xyz="0 -0.176 -0.05489"
      rpy="1.5708 0 0" />
    <parent
      link="laserbase_link" />
    <child
      link="right_wheel" />
    <axis
      xyz="0 0 -1" />
  </joint>
  <link
    name="left_wheel">
    <inertial>
      <origin
        xyz="2.2482E-15 -1.7625E-15 -0.040256"
        rpy="0 0 0" />
      <mass
        value="0.4651" />
      <inertia
        ixx="0.00057862"
        ixy="-6.9556E-21"
        ixz="3.436E-18"
        iyy="0.00057862"
        iyz="4.1026E-21"
        izz="0.00090953" />
    </inertial>
    <visual>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/left_wheel.STL" />
      </geometry>
      <material
        name="">
        <color
          rgba="0.29412 0.29412 0.29412 1" />
      </material>
    </visual>
    <collision>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/left_wheel.STL" />
      </geometry>
    </collision>
  </link>
  <joint
    name="rot2"
    type="fixed">
    <origin
      xyz="0 0.176 -0.05489"
      rpy="-1.5708 0 3.1416" />
    <parent
      link="laserbase_link" />
    <child
      link="left_wheel" />
    <axis
      xyz="0 0 -1" />
  </joint>
  <link
    name="back_wheel">
    <inertial>
      <origin
        xyz="-0.016124 -0.038361 -1.58E-09"
        rpy="0 0 0" />
      <mass
        value="0.070437" />
      <inertia
        ixx="3.8166E-05"
        ixy="9.5646E-06"
        ixz="-2.9588E-12"
        iyy="2.3031E-05"
        iyz="-3.2194E-12"
        izz="5.0491E-05" />
    </inertial>
    <visual>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/back_wheel.STL" />
      </geometry>
      <material
        name="">
        <color
          rgba="0.098039 0.098039 0.098039 1" />
      </material>
    </visual>
    <collision>
      <origin
        xyz="0 0 0"
        rpy="0 0 0" />
      <geometry>
        <mesh
          filename="package://xiaoqiang_udrf/meshes/back_wheel.STL" />
      </geometry>
    </collision>
  </link>
  <joint
    name="rot3"
    type="fixed">
    <origin
      xyz="-0.25204 -0.00060998 -0.037"
      rpy="1.5708 0 -5.9631E-17" />
    <parent
      link="laserbase_link" />
    <child
      link="back_wheel" />
    <axis
      xyz="0 -1 0" />
  </joint>

  <link name="rslidar">
    <visual>
      <origin xyz="0 0 0" />
      <geometry>
        <cylinder length="0.05" radius="0.05" />
      </geometry>
      <material name="gray" />
    </visual>
  </link>
  <joint name="horizontal_laser_link_joint" type="fixed">
    <parent link="laserbase_link" />
    <child link="rslidar" />
    <origin xyz="-0.25 0 0.4" rpy="0 0 0" />
  </joint>

  <link name="imu">
    <visual>
      <origin xyz="0 0 0" />
      <geometry>
        <cylinder length="0.01" radius="0.01" />
      </geometry>
      <material name="gray" />
    </visual>
  </link>
  <joint name="imu_link_joint" type="fixed">
    <parent link="laserbase_link" />
    <child link="imu" />
    <origin xyz="0 0 0" rpy="0 0 0" />
  </joint>
</robot>

```



cartographer_ros/configuration_files/xiaoqiang_3d.lua 

```lua
-- Copyright 2016 The Cartographer Authors
--
-- Licensed under the Apache License, Version 2.0 (the "License");
-- you may not use this file except in compliance with the License.
-- You may obtain a copy of the License at
--
--      http://www.apache.org/licenses/LICENSE-2.0
--
-- Unless required by applicable law or agreed to in writing, software
-- distributed under the License is distributed on an "AS IS" BASIS,
-- WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
-- See the License for the specific language governing permissions and
-- limitations under the License.

include "map_builder.lua"
include "trajectory_builder.lua"

options = {
  map_builder = MAP_BUILDER,
  trajectory_builder = TRAJECTORY_BUILDER,
  map_frame = "map_laser",
  tracking_frame = "laserbase_link",
  published_frame = "base_footprint",
  odom_frame = "odom",
  provide_odom_frame = true,
  publish_frame_projected_to_2d = false,
  use_odometry = true,
  use_nav_sat = false,
  use_landmarks = false,
  num_laser_scans = 0,
  num_multi_echo_laser_scans = 0,
  num_subdivisions_per_laser_scan = 1,
  num_point_clouds = 1,
  lookup_transform_timeout_sec = 0.2,
  submap_publish_period_sec = 0.3,
  pose_publish_period_sec = 5e-3,
  trajectory_publish_period_sec = 30e-3,
  rangefinder_sampling_ratio = 1.,
  odometry_sampling_ratio = 1.,
  fixed_frame_pose_sampling_ratio = 1.,
  imu_sampling_ratio = 1.,
  landmarks_sampling_ratio = 1.,
}

TRAJECTORY_BUILDER_3D.num_accumulated_range_data = 1
TRAJECTORY_BUILDER_3D.min_range = 0.2
TRAJECTORY_BUILDER_3D.max_range = 20.
TRAJECTORY_BUILDER_2D.min_z = 0.1
TRAJECTORY_BUILDER_2D.max_z = 1.0
TRAJECTORY_BUILDER_3D.use_online_correlative_scan_matching = false

MAP_BUILDER.use_trajectory_builder_3d = true
MAP_BUILDER.num_background_threads = 4
POSE_GRAPH.optimization_problem.huber_scale = 5e2
POSE_GRAPH.optimize_every_n_nodes = 40
POSE_GRAPH.constraint_builder.sampling_ratio = 0.03
POSE_GRAPH.optimization_problem.ceres_solver_options.max_num_iterations = 20
POSE_GRAPH.constraint_builder.min_score = 0.5
POSE_GRAPH.constraint_builder.global_localization_min_score = 0.55


POSE_GRAPH.optimization_problem.odometry_translation_weight = 1e3
POSE_GRAPH.optimization_problem.odometry_rotation_weight = 1e3

return options

```





---

疑問：

1. 什麼是純定位（純本地化）（pure localization）？