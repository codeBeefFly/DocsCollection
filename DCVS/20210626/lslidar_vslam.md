# lslidar_vslam

[toc]



---

## 20210628

目标：

MOOC ros

hector_mapping

chinese-english dic (done): [1.17 ubuntu下安装有道词典](https://porter.gitbook.io/deep-learning-series/index/1.17-ubuntu-xia-an-zhuang-you-dao-ci-dian)



### 安装 ros-kinetic package 流程

1. 查看 ros-kinetic package 的内容：通过网址 [ROS packages for Kinetic](http://repositories.ros.org/status_page/ros_kinetic_default.html) 找出目标 package，比如 pointcloud_to_laserscan。

   ![image-20210628134322231](lslidar_vslam.assets/image-20210628134322231.png)

2. 在 CLI中通过 `sudo apt install ros-kinetic-[TAB]-[TAB]` 来安装 package，这样可以避免横线根下划线问题。

   ```
   ds16v2@ds16v2:~$ sudo apt install ros-kinetic-pointcloud-to-laserscan 
   ```

3. 有时候需要更新 ubuntu package list 才能找到目标package

   ![image-20210628134822034](lslidar_vslam.assets/image-20210628134822034.png)

   通过 `sudo apt update 与 sudo apt upgrade`进行。



### 回顾 5044

reference to 5044

![image-20210628100958410](lslidar_vslam.assets/image-20210628100958410.png)



study this yaml file

```xml
<!--
Maps the environment with RFANS LiDAR using Hector SLAM
No robot is necessary
-->
<launch>

    <arg name="scan_topic"         default="front/scan" />			# 在 launch 文件中定义参数
    <arg name="use_odom"           default="false" />				# ...
    <arg name="map_frame"          default="map" />					# ...
    <arg name="base_frame"         default="base_footprint" />		# ...
    <arg name="odom_frame"         default="odom" />				# ...

    <!-- PointCloud to laserscan -->
    <node pkg="pointcloud_to_laserscan" 							# 要启动 pkg 名称 
    	  type="pointcloud_to_laserscan_node" 						# 节点可执行文件名称 
    	  name="pointcloud_to_laserscan">							# 可执行文件启动后节点名称 
      
      	<remap from="cloud_in" to="/rfans_driver/rfans_points" />	# 将功能包接口重命名
      	<remap from="scan"     to="/front/scan" />					# ...

      	<param name="target_frame"    value="rfans" />				# ROS 系统加载参数
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
    <node pkg="hector_mapping" 
    	  type="hector_mapping" 
    	  name="hector_mapping" 
    	  output="screen" >											# 输出在控制台，而不是日志
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



ros launch 文件标签：

![image-20210628142620782](lslidar_vslam.assets/image-20210628142620782.png)

![image-20210628143048611](lslidar_vslam.assets/image-20210628143048611.png)

![image-20210628143302909](lslidar_vslam.assets/image-20210628143302909.png)

![image-20210628144213073](lslidar_vslam.assets/image-20210628144213073.png)

![image-20210628145829859](lslidar_vslam.assets/image-20210628145829859.png)

![image-20210628150226907](lslidar_vslam.assets/image-20210628150226907.png)



分清 node 与 package。

ros package: ros 工程结构中

![image-20210628135948353](lslidar_vslam.assets/image-20210628135948353.png)

ros node: ros 通信结构中

![image-20210628135759334](lslidar_vslam.assets/image-20210628135759334.png)



### 参考：

link: [深耕系列之深度学习笔记(非常好的深度学习总结博客)](https://porter.gitbook.io/deep-learning-series/)

link: [使用pointcloud_to_laserscan包将速腾聚创3d激光雷达转换成高质量2d激光雷达](http://community.bwbot.org/topic/521/%E4%BD%BF%E7%94%A8pointcloud_to_laserscan%E5%8C%85%E5%B0%86%E9%80%9F%E8%85%BE%E8%81%9A%E5%88%9B3d%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E8%BD%AC%E6%8D%A2%E6%88%90%E9%AB%98%E8%B4%A8%E9%87%8F2d%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE)

link: [Ubuntu install of ROS Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu)

link: [ROS基础总结🤖🤖🤖](https://zhuanlan.zhihu.com/p/123240150)

link: [ROS学习笔记六：xxx.launch文件详解](https://www.cnblogs.com/linuxAndMcu/p/10648577.html)





---

## 20210626

todo



