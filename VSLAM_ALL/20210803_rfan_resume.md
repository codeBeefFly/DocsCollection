# 2021-08-05：rfan 重启 [resume]

[toc]

---

## 0. review

1. 相关 ticket：
   1. 5044：ros 建图。最后更新：使用 lslidar_C16 可以建图，但是没有办法形成闭环。
   2. 5638：rfans-32 雷达点云显示。最后更新：得到 ros 点云图，但是没有使用 hector_mapping。
2. 计划
   1. 回顾 **5044**，5638，分析 launch 文件，重新启动 lidar。
   2. 使用 lslidarC16 的 launch code（5044），启动 rfans-32。
   3. 做好资料记录，总结。



---

## 1. review 5044, catkin_make hector_slame from source

![image-20210805143453243](20210803_rfan_resume.assets/image-20210805143453243.png)

这个是 hector_slam_rfans.launch 的内容：

```xml
<!--
Maps the environment with RFANS LiDAR using Hector SLAM
No robot is necessary
-->
<launch>

    <arg name="scan_topic"         default="front/scan" />
    <arg name="use_odom"           default="false" />
    <arg name="map_frame"          default="map" />
    <arg name="base_frame"         default="base_footprint" />
    <arg name="odom_frame"         default="odom" />

    <!-- PointCloud to laserscan -->
    <node pkg="pointcloud_to_laserscan" type="pointcloud_to_laserscan_node" name="pointcloud_to_laserscan">
      <remap from="cloud_in" to="/rfans_driver/rfans_points" />
      <remap from="scan"     to="/front/scan" />

      <param name="target_frame"    value="rfans" />
      <param name="min_height"      value="0.0" />
      <param name="max_height"      value="1.0" />
      <param name="angle_min"       value="-3.14" />
      <param name="angle_max"       value="3.14" />
      <param name="angle_increment" value="0.00655" />
      <param name="scan_time"       value="0.0" />
      <param name="range_min"       value="0.45" />
      <param name="range_max"       value="100.0" />
      <param name="use_inf"         value="true" />
    </node>

    <!-- Hector SLAM -->
    <node pkg="hector_mapping" type="hector_mapping" name="hector_mapping" output="screen" >
        <param name="pub_map_odom_transform" value="true"/>
        <param name="map_frame" value="$(arg map_frame)" />
        <param name="base_frame" value="$(arg base_frame)" />
        <param name="map_size" value="2048" />
        <param if="$(arg use_odom)" name="odom_frame" value="$(arg odom_frame)" />
        <param unless="$(arg use_odom)" name="odom_frame" value="$(arg base_frame)" />
        <param name="scan_topic" value="$(arg scan_topic)" />
    </node>  

</launch>
```



这些是 hector_mapping 的一些建议值，以及 cartographer slam 的提及。

![image-20210805152351984](20210803_rfan_resume.assets/image-20210805152351984.png)



今天读3篇csdn

link1：[Hector SLAM 安装与配置](https://blog.csdn.net/i_robots/article/details/108287321)

link2：[Gmapping 之安装与配置](https://blog.csdn.net/i_robots/article/details/107884080)

link3：[ROS使用hector_slam+衫川雷达建图（一）](https://blog.csdn.net/qq_43481884/article/details/102845143?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)

link4：[资料](https://download.csdn.net/download/qq_34213260/11833334?spm=1001.2101.3001.5697)

link5：[Gmapping、hector、Cartographer三种激光SLAM算法简单对比](https://blog.csdn.net/Jeff_Lee_/article/details/77869987)

link6：[tu-darmstadt-ros-pkg](https://github.com/tu-darmstadt-ros-pkg)/**[hector_slam](https://github.com/tu-darmstadt-ros-pkg/hector_slam)**

link7：[hector-slam安装与使用 （ubuntu 16.04）（使用数据包运行hector-slam）](https://blog.csdn.net/me1171115772/article/details/105110851/?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.control&spm=1001.2101.3001.4242)



现在根据 link7 进行 hector_slam 的源码安装，安装目录 `...\slam_ws`：

![image-20210805160044868](20210803_rfan_resume.assets/image-20210805160044868.png)

![image-20210805155336367](20210803_rfan_resume.assets/image-20210805155336367.png)

![image-20210805155423255](20210803_rfan_resume.assets/image-20210805155423255.png)



使用 hector_slam 原 launch 文件，启动 rviz。

```
ds16v2@ds16v2:~/catkin_x/slam_ws$ roslaunch hector_slam_launch tutorial.launch 
```

![image-20210805163632220](20210803_rfan_resume.assets/image-20210805163632220.png)

console 中的输出，需要注意一些 parameters 和 nodes：

```
ds16v2@ds16v2:~/catkin_x/slam_ws$ roslaunch hector_slam_launch tutorial.launch 
... logging to /home/ds16v2/.ros/log/13c3e70e-f5c8-11eb-abbe-a0c589ac1e85/roslaunch-ds16v2-5323.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://ds16v2:41515/

SUMMARY
========

PARAMETERS
 * /hector_geotiff_node/draw_background_checkerboard: True
 * /hector_geotiff_node/draw_free_space_grid: True
 * /hector_geotiff_node/geotiff_save_period: 0.0
 * /hector_geotiff_node/map_file_base_name: hector_slam_map
 * /hector_geotiff_node/map_file_path: /home/ds16v2/catk...
 * /hector_geotiff_node/plugins: hector_geotiff_pl...
 * /hector_mapping/advertise_map_service: True
 * /hector_mapping/base_frame: base_footprint
 * /hector_mapping/laser_z_max_value: 1.0
 * /hector_mapping/laser_z_min_value: -1.0
 * /hector_mapping/map_frame: map
 * /hector_mapping/map_multi_res_levels: 2
 * /hector_mapping/map_resolution: 0.05
 * /hector_mapping/map_size: 2048
 * /hector_mapping/map_start_x: 0.5
 * /hector_mapping/map_start_y: 0.5
 * /hector_mapping/map_update_angle_thresh: 0.06
 * /hector_mapping/map_update_distance_thresh: 0.4
 * /hector_mapping/odom_frame: nav
 * /hector_mapping/pub_map_odom_transform: True
 * /hector_mapping/scan_subscriber_queue_size: 5
 * /hector_mapping/scan_topic: scan
 * /hector_mapping/tf_map_scanmatch_transform_frame_name: scanmatcher_frame
 * /hector_mapping/update_factor_free: 0.4
 * /hector_mapping/update_factor_occupied: 0.9
 * /hector_mapping/use_tf_pose_start_estimate: False
 * /hector_mapping/use_tf_scan_transformation: True
 * /hector_trajectory_server/source_frame_name: scanmatcher_frame
 * /hector_trajectory_server/target_frame_name: /map
 * /hector_trajectory_server/trajectory_publish_rate: 0.25
 * /hector_trajectory_server/trajectory_update_rate: 4.0
 * /rosdistro: kinetic
 * /rosversion: 1.12.17
 * /use_sim_time: True

NODES
  /
    hector_geotiff_node (hector_geotiff/geotiff_node)
    hector_mapping (hector_mapping/hector_mapping)
    hector_trajectory_server (hector_trajectory_server/hector_trajectory_server)
    rviz (rviz/rviz)

auto-starting new master
process[master]: started with pid [5333]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to 13c3e70e-f5c8-11eb-abbe-a0c589ac1e85
process[rosout-1]: started with pid [5346]
started core service [/rosout]
process[rviz-2]: started with pid [5370]
process[hector_mapping-3]: started with pid [5371]
process[hector_trajectory_server-4]: started with pid [5372]
process[hector_geotiff_node-5]: started with pid [5379]
[ INFO] [1628152524.630914828]: Creating application with offscreen platform.
[ INFO] [1628152524.643270822]: Waiting for tf transform data between frames /map and scanmatcher_frame to become available
HectorSM map lvl 0: cellLength: 0.05 res x:2048 res y: 2048
HectorSM map lvl 1: cellLength: 0.1 res x:1024 res y: 1024
[ INFO] [1628152524.704030673]: Created application
[ INFO] [1628152524.730046720]: HectorSM p_base_frame_: base_footprint
[ INFO] [1628152524.730097540]: HectorSM p_map_frame_: map
[ INFO] [1628152524.730111714]: HectorSM p_odom_frame_: nav
[ INFO] [1628152524.730123270]: HectorSM p_scan_topic_: scan
[ INFO] [1628152524.730134893]: HectorSM p_use_tf_scan_transformation_: true
[ INFO] [1628152524.730152172]: HectorSM p_pub_map_odom_transform_: true
[ INFO] [1628152524.730167099]: HectorSM p_scan_subscriber_queue_size_: 5
[ INFO] [1628152524.730187431]: HectorSM p_map_pub_period_: 2.000000
[ INFO] [1628152524.730204401]: HectorSM p_update_factor_free_: 0.400000
[ INFO] [1628152524.730222035]: HectorSM p_update_factor_occupied_: 0.900000
[ INFO] [1628152524.730248538]: HectorSM p_map_update_distance_threshold_: 0.400000 
[ INFO] [1628152524.730264965]: HectorSM p_map_update_angle_threshold_: 0.060000
[ INFO] [1628152524.730281973]: HectorSM p_laser_z_min_value_: -1.000000
[ INFO] [1628152524.730299179]: HectorSM p_laser_z_max_value_: 1.000000
[ INFO] [1628152524.748676715]: Successfully initialized hector_geotiff MapWriter plugin TrajectoryMapWriter.
[ INFO] [1628152524.748719378]: Geotiff node started
0x14b5cd0 void QWindowPrivate::setTopLevelScreen(QScreen*, bool) ( QScreen(0x947140) ): Attempt to set a screen on a child window.
0x14b7850 void QWindowPrivate::setTopLevelScreen(QScreen*, bool) ( QScreen(0x947140) ): Attempt to set a screen on a child window.
0x14b61f0 void QWindowPrivate::setTopLevelScreen(QScreen*, bool) ( QScreen(0x947140) ): Attempt to set a screen on a child window.
0x14b7380 void QWindowPrivate::setTopLevelScreen(QScreen*, bool) ( QScreen(0x947140) ): Attempt to set a screen on a child window.

```

可能需要调节的参数：

```
 * /hector_mapping/advertise_map_service: True
 * /hector_mapping/base_frame: base_footprint				# 基坐标系
 * /hector_mapping/laser_z_max_value: 1.0
 * /hector_mapping/laser_z_min_value: -1.0
 
 * /hector_mapping/map_frame: map							# 地图坐标系
 * /hector_mapping/map_multi_res_levels: 2
 * /hector_mapping/map_resolution: 0.05						# 调节地图精度
 * /hector_mapping/map_size: 2048							# 调节地图尺寸
 * /hector_mapping/map_start_x: 0.5
 * /hector_mapping/map_start_y: 0.5
 * /hector_mapping/map_update_angle_thresh: 0.06			# 很重要的一个参数
 * /hector_mapping/map_update_distance_thresh: 0.4			# 很重要的一个参数
 
 * /hector_mapping/odom_frame: nav							# 里程计坐标系
 * /hector_mapping/pub_map_odom_transform: True
 * /hector_mapping/scan_subscriber_queue_size: 5
 * /hector_mapping/scan_topic: scan
 * /hector_mapping/tf_map_scanmatch_transform_frame_name: scanmatcher_frame
 * /hector_mapping/update_factor_free: 0.4
 * /hector_mapping/update_factor_occupied: 0.9
 * /hector_mapping/use_tf_pose_start_estimate: False
 * /hector_mapping/use_tf_scan_transformation: True
```



接下来需要启动 rfans 以及修改 hector_slame 中的 launch 文件，放在下一节。



---

## 2. launch rfans and view point cloud through rviz

如果遇到在 clion 打开 ros package 失败的情况：clion find_package(catkin) failed：

![image-20210805170852331](20210803_rfan_resume.assets/image-20210805170852331.png)

可以参考 stackoverflow 这篇文章，可能的原因是 clion 没有办法调用 catkin 命令。

link: [Setting up ROS package in CLion](https://stackoverflow.com/questions/33172132/setting-up-ros-package-in-clion)

> Try this (for Linux):
>
> 1. Open a command line
> 2. Run *catkin_make* on your package.
> 3. *source* your *catkin_workspace/devel/setup.bash* file e.g. *source ~/my_dev_folder/catkin_ws/devel/setup.bash*
> 4. Start CLion from *[CLion install dir]/bin/clion.sh* e.g. *cd ~/Downloads/clion-1.2.4/bin && ./clion.sh*
>
> CLion should then start with knowledge about the packages in your catkin workspace, through the local environment variables set up by the setup.bash file.



@@@@@@@@@@@@@ 这是分隔符 @@@@@@@@@@@@@



**在 linux 下可以通过键盘来移动窗口：**

![image-20210805174911136](20210803_rfan_resume.assets/image-20210805174911136.png)



@@@@@@@@@@@@@ 这是分隔符 @@@@@@@@@@@@@



**回顾 rfans32 的 ros 配置：**

```
主机IP：192.168.0.9
掩码：255.255.255.0

rfansIP：192.168.0.3
```

![image-20210805191400669](20210803_rfan_resume.assets/image-20210805191400669.png)



```
$ ping 192.168.0.3

PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=128 time=0.411 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=128 time=0.497 ms
64 bytes from 192.168.0.3: icmp_seq=3 ttl=128 time=0.322 ms
64 bytes from 192.168.0.3: icmp_seq=4 ttl=128 time=0.248 ms
64 bytes from 192.168.0.3: icmp_seq=5 ttl=128 time=0.377 ms
64 bytes from 192.168.0.3: icmp_seq=6 ttl=128 time=0.247 ms
64 bytes from 192.168.0.3: icmp_seq=7 ttl=128 time=0.340 ms
64 bytes from 192.168.0.3: icmp_seq=8 ttl=128 time=0.303 ms
64 bytes from 192.168.0.3: icmp_seq=9 ttl=128 time=0.505 ms
64 bytes from 192.168.0.3: icmp_seq=10 ttl=128 time=0.465 ms
```



**在 linux 下可以同时使用 wifi 根 ethernet 连接。**



@@@@@@@@@@@@@ 这是分隔符 @@@@@@@@@@@@@



**启动 rfans32 + rviz：**

将 `/home/ds16v2/ros_ws/src/StarROS/` 下的 revise.ini 放入 `/home/ds16v2/ros_ws/src/StarROS/launch/`下。

在 node_manager.launch 中添加：

```
<param name="cfg_path"  value="/home/ds16v2/ros_ws/src/StarROS/launch/revise.ini"/>
```

在终端启动 rfans32，注意 **PARAMETERS** 跟 **NODES**：

```
ds16v2@ds16v2:~/ros_ws$ roslaunch rfans_driver node_manager.launch 

... logging to /home/ds16v2/.ros/log/b2f31aee-f5d1-11eb-abbe-a0c589ac1e85/roslaunch-ds16v2-18817.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://ds16v2:35185/

SUMMARY
========

PARAMETERS
 * /frame_id: world
 * /model: R-Fans-16
 * /rfans_driver/OutExport_path: 
 * /rfans_driver/advertise_name: rfans_packets
 * /rfans_driver/cfg_path: /home/ds16v2/ros_...
 * /rfans_driver/control_name: rfans_control
 * /rfans_driver/cut_angle_range: 360.0
 * /rfans_driver/device_ip: 192.168.0.3
 * /rfans_driver/device_port: 2014
 * /rfans_driver/read_fast: False
 * /rfans_driver/read_once: False
 * /rfans_driver/readfile_path: 
 * /rfans_driver/repeat_delay: 0.0
 * /rfans_driver/rps: 10
 * /rfans_driver/save_xyz: False
 * /rfans_driver/use_double_echo: False
 * /rfans_driver/use_gps: False
 * /rosdistro: kinetic
 * /rosversion: 1.12.17

NODES
  /
    rfans_driver (rfans_driver/driver_node)

auto-starting new master
process[master]: started with pid [18827]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to b2f31aee-f5d1-11eb-abbe-a0c589ac1e85
process[rosout-1]: started with pid [18840]
started core service [/rosout]
process[rfans_driver-2]: started with pid [18857]


```

在 rostopic list 中查看 rfans 发布的话题：

![image-20210805193128540](20210803_rfan_resume.assets/image-20210805193128540.png)

查看 rostopic /lidar_points 信息：

![image-20210805193321461](20210803_rfan_resume.assets/image-20210805193321461.png)

查看关于 rfans 的其他的两个 topic 信息：

![image-20210805193622584](20210803_rfan_resume.assets/image-20210805193622584.png)

![image-20210805193636034](20210803_rfan_resume.assets/image-20210805193636034.png)



启动 rviz：

```
ds16v2@ds16v2:~/ros_ws$ rviz
[ INFO] [1628156699.198379782]: rviz version 1.12.17
[ INFO] [1628156699.198434330]: compiled against Qt version 5.5.1
[ INFO] [1628156699.198450045]: compiled against OGRE version 1.9.0 (Ghadamon)
[ INFO] [1628156699.859484229]: Stereo is NOT SUPPORTED
[ INFO] [1628156699.859589670]: OpenGl version: 4.6 (GLSL 4.6).
0x2836760 void QWindowPrivate::setTopLevelScreen(QScreen*, bool) ( QScreen(0x1caee80) ): Attempt to set a screen on a child window.
```

加载 rviz 配置文件:点击 rviz 菜单栏 File 选项中的 Open Config 按钮(ctrl+O),
加载 ros_ws/src/StarROS 下的 StarROS_Rviz_cfg.rviz

![image-20210805193953982](20210803_rfan_resume.assets/image-20210805193953982.png)



尝试 直接在 launch 文件中写入 rviz node。（done）

```xml
# 在 node_manager_rviz.launch 文件中，加入
    <node pkg="rviz" type="rviz" name="rviz"
          args="-d $(find rfans_driver)/config/StarROS_Rviz_cfg.rviz" required="true" />
```

其中 find 后面接 package 名称，可以通过 rospack list 或 find 找出：

```
# 表示：package rfans_driver 的路径为 /home/ds16v2/ros_ws/src/StarROS
ds16v2@ds16v2:~/ros_ws$ rospack find rfans_driver 
/home/ds16v2/ros_ws/src/StarROS
```



link：[lauch problems; ResourceNotFound](https://answers.ros.org/question/37826/lauch-problems-resourcenotfound/)



---

## 3. hector_slam 中使用 rfans32





---

## &. reference









