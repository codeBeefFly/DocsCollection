# 2021年9月13日：rfans+cartographer+rosbag

[toc]



---

日志：

2021年9月13日：cartographer_ros + rfans 编译完成，可以运行：





---

## 2021年9月13日



关于 脚本的理解，可以参考：20210821_slam_cartographer_mapping.md。



![image-20210913112518258](C:\Z_DesayWorkSpace\CodeBeefSpace\DocsCollection\VSLAM_ALL\20210913_rfans_cartographer_rosbag.assets\image-20210913112518258.png)



**node_manager_rviz.launch:**

```xml
<?xml version="1.0"?>

<launch>  

<arg name="read_fast" default="false" />
<arg name="read_once" default="false" />
<arg name="repeat_delay" default="0.0" />
<param name="frame_id" value="world" />
<param name="model" value="R-Fans-16" />

<node pkg="rfans_driver" type="driver_node" name="rfans_driver" output="screen">
  <param name="advertise_name" value="rfans_packets" />
  <param name="control_name" value="rfans_control"/>
  <param name="device_ip" value="192.168.0.3" />
  <param name="device_port" value="2014" />
  <param name="rps" value="10"/>
  <param name="readfile_path"  value=""/>
  <param name="cfg_path"  value="/home/ds18/catkin_x/rfans_ws/src/StarROS/revise.ini"/>
  <param name="save_xyz"  value="false"/>
  <param name="OutExport_path"  value=""/>
  <param name="use_double_echo" value="false"/>
  <param name="use_gps" value="false"/>
  <param name="read_fast" value="$(arg read_fast)" />
  <param name="read_once" value="$(arg read_once)" />
  <param name="repeat_delay" value="$(arg repeat_delay)" />
  <param name ="cut_angle_range" value="360.0"/>

</node>


 <!--<node pkg="rfans_driver" type="u_coordinate" name="u_coordinate" output="screen"></node>-->
  <!--<node pkg="rfans_driver" type="imuPub" name="imuPub" output="screen"></node>-->
 <node pkg="rviz" type="rviz" name="rviz" args=" -d $(find rfans_driver)/rviz/my_rfan_rviz_2.rviz"/>

</launch>
```



在 cartographer_ws_mk3 中已经集成了 StarROS（rfans 的包），在这个 workspace 下，启动 rfans。

```
ds18@ubuntu:~/catkin_x/cartographer_ws_mk3$ roslaunch rfans_driver node_manager_rviz.launch

... logging to /home/ds18/.ros/log/5c68c0ac-145d-11ec-b9a2-137a2bb433f7/roslaunch-ubuntu-16282.log
Checking log directory for disk usage. This may take a while.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://ubuntu:40871/

SUMMARY
========

PARAMETERS
 * /frame_id: world
 * /model: R-Fans-16
 * /rfans_driver/OutExport_path: 
 * /rfans_driver/advertise_name: rfans_packets
 * /rfans_driver/cfg_path: /home/ds18/catkin...
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
 * /rosdistro: melodic
 * /rosversion: 1.14.11

NODES
  /
    rfans_driver (rfans_driver/driver_node)
    rviz (rviz/rviz)

auto-starting new master
process[master]: started with pid [16292]
ROS_MASTER_URI=http://localhost:11311

setting /run_id to 5c68c0ac-145d-11ec-b9a2-137a2bb433f7
process[rosout-1]: started with pid [16303]
started core service [/rosout]
process[rfans_driver-2]: started with pid [16310]
process[rviz-3]: started with pid [16311]
QObject::connect: Cannot queue arguments of type 'QVector<int>'
(Make sure 'QVector<int>' is registered using qRegisterMetaType().)
QObject::connect: Cannot queue arguments of type 'QVector<int>'
(Make sure 'QVector<int>' is registered usi
```



查看当前正在运行的 rosnode:

```
ds18@ubuntu:~/catkin_x/cartographer_ws_mk3$ rosnode list

/rfans_driver
/rosout
/rviz
```



查看当前的正在运行的 rostopic:

```
ds18@ubuntu:~/catkin_x/cartographer_ws_mk3$ rostopic list

/clicked_point
/contrlComand
/initialpose
/lidar_points
/move_base_simple/goal
/rfans_driver/parameter_descriptions
/rfans_driver/parameter_updates
/rosout
/rosout_agg
/tf
/tf_static
```



查看当前正在运行的 rosservice:

```
ds18@ubuntu:~/catkin_x/cartographer_ws_mk3$ rosservice list 

/rfans_driver/get_loggers
/rfans_driver/set_logger_level
/rfans_driver/set_parameters
/rosout/get_loggers
/rosout/set_logger_level
/rviz/get_loggers
/rviz/load_config
/rviz/reload_shaders
/rviz/save_config
/rviz/set_logger_level
```



查看当前正在运行的 rosmsg:

有很多，截取与 rviz 与 rfans 相关的：

```
ds18@ubuntu:~/catkin_x/cartographer_ws_mk3$ rosmsg list

rfans_driver/Packet
rfans_driver/RfansPacket
rfans_driver/RfansScan
rfans_driver/command
rfans_driver/imuData
```



查看 rosnode /rfans_driver:

```
ds18@ubuntu:~/catkin_x/cartographer_ws_mk3$ rosnode info /rfans_driver 

--------------------------------------------------------------------------------
Node [/rfans_driver]
Publications: 
 * /lidar_points [sensor_msgs/PointCloud2]
 * /rfans_driver/parameter_descriptions [dynamic_reconfigure/ConfigDescription]
 * /rfans_driver/parameter_updates [dynamic_reconfigure/Config]
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /contrlComand [unknown type]

Services: 
 * /rfans_driver/get_loggers
 * /rfans_driver/set_logger_level
 * /rfans_driver/set_parameters


contacting node http://ubuntu:39889/ ...
Pid: 16310
Connections:
 * topic: /rosout
    * to: /rosout
    * direction: outbound (39497 - 127.0.0.1:57576) [13]
    * transport: TCPROS
 * topic: /lidar_points
    * to: /rviz
    * direction: outbound (39497 - 127.0.0.1:57584) [14]
    * transport: TCPROS

```

