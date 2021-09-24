# 2021年9月24日：cartographer_ros + rosbag record

[toc]

---

## **target:**

1. 20210924:
   1. cartographer + rosbag + launch file
   2. cartographer + rfans + launch file (real time)
   3. office rosbag record



2. 20210925:
   1. office rosbag record
   2. underground rosbag record



---

## 1. 2021年9月24日：



### 1.1. 录制 cartographer

```
1. 验证 rosbag 工具：cartographer_rosbag_validate
	使用 cartographer_rosbag_validate 分析 rosbag 中的数据。
	(It is generally a good idea to run this tool before trying to tune Cartographer for incorrect data.)
	
---+---+---+---+---+---+---+---+---+---+---+

2. 创建 .lua 配置
	2.1. robot configuration 文件使用 .lua 格式编写。
	2.2. 示例 configuration .lua 文件 存放 路径：
			src/cartographer_ros/cartographer_ros/configuration_files
	2.3. 示例 configuration .lua 文件 安装路径：
			install_isolated/share/cartographer_ros/configuration_files/
	2.4. 快速编写自己的 .lua 文件（就是抄作业啦~~~）：
    	*. 3D Slam:
    	cp install_isolated/share/cartographer_ros/configuration_files/backpack_3d.lua install_isolated/share/cartographer_ros/configuration_files/my_robot.lua
    	*. 2D slam:
    	cp install_isolated/share/cartographer_ros/configuration_files/backpack_2d.lua install_isolated/share/cartographer_ros/configuration_files/my_robot.lua
    
    2.5. 快速修改自己的 .lua 文件
    	*. 编写 my_robot.lua.
    	*. 可修改的值位置 option 区域
    2.6. 在修改 .lua 文件中，需要修改 TF frame IDS:
    	*. map_frame
    	*. tracking_frame
    	*. published_frame
    	*. odom_frame
	2.7. You can either distribute your robot’s TF tree from a /tf topic in your bag or define it in a .urdf robot 
			definition.
		*. 使用 /tf (transfrom frame).
		*. 这是一个实时工具，观察在Ros上被发布的坐标系树，可用刷新按钮来更新树的内容。与上一个工具的区别在于：上一个工具连续采样5s获得的树的内容，
			并存成一个图片；这个工具可以连续的观察树的内容，使用起来更方便。
	2.8. 可以使用 use_landmarks & use_nav_sat 启动 地标 + GPS。option 中其他的变量最好保留不动。 
	2.9. 
	
---+---+---+---+---+---+---+---+---+---+---+



	
```

注：关于 .lua 配置，参考：[Lua configuration reference documentation](https://google-cartographer-ros.readthedocs.io/en/latest/configuration.html#lua-configuration-reference-documentation)



示例：2D SLAM：backpad_2d.lua 

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
  map_frame = "map",
  tracking_frame = "base_link",
  published_frame = "base_link",
  odom_frame = "odom",
  provide_odom_frame = true,
  publish_frame_projected_to_2d = false,
  use_pose_extrapolator = true,
  use_odometry = false,
  use_nav_sat = false,
  use_landmarks = false,
  num_laser_scans = 0,
  num_multi_echo_laser_scans = 1,
  num_subdivisions_per_laser_scan = 10,
  num_point_clouds = 0,
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

MAP_BUILDER.use_trajectory_builder_2d = true
TRAJECTORY_BUILDER_2D.num_accumulated_range_data = 10

return options
```



示例：3D SLAM：backpack_3d.lua

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
  map_frame = "map",
  tracking_frame = "base_link",
  published_frame = "base_link",
  odom_frame = "odom",
  provide_odom_frame = true,
  publish_frame_projected_to_2d = false,
  use_pose_extrapolator = true,
  use_odometry = false,
  use_nav_sat = false,
  use_landmarks = false,
  num_laser_scans = 0,
  num_multi_echo_laser_scans = 0,
  num_subdivisions_per_laser_scan = 1,
  num_point_clouds = 2,
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

TRAJECTORY_BUILDER_3D.num_accumulated_range_data = 160

MAP_BUILDER.use_trajectory_builder_3d = true
MAP_BUILDER.num_background_threads = 7
POSE_GRAPH.optimization_problem.huber_scale = 5e2
POSE_GRAPH.optimize_every_n_nodes = 320
POSE_GRAPH.constraint_builder.sampling_ratio = 0.03
POSE_GRAPH.optimization_problem.ceres_solver_options.max_num_iterations = 10
POSE_GRAPH.constraint_builder.min_score = 0.62
POSE_GRAPH.constraint_builder.global_localization_min_score = 0.66

return options
```



对比：

![image-20210924144256206](C:\Z_DesayWorkSpace\CodeBeefSpace\DocsCollection\VSLAM_ALL\20210924_rfans_cartographer_rosbag_2.assets\image-20210924144256206.png)



### 1.2. TF 命令：

```
rosrun tf view_frames          # 然后会看到一些提示，并且生成了一个frames.pdf文件。

# 1. 采用 tf_monitor，查看当前TF树中所有坐标系的发布状态。 
rosrun tf tf_monitor

# 2. 采用 rqt_tf_tree，查看当前所有坐标之间的变换关系，可通过刷新更新当前树的内容。  
rosrun rqt_tf_tree rqt_tf_tree

# 3. 采用 tf_echo，获取reference_frame和target_frame之间的坐标变换关系。 
rosrun tf tf_echo [reference_frame] [target_frame]
```









---

## &&. 参考

link01: [Running Cartographer ROS on your own bag](https://google-cartographer-ros.readthedocs.io/en/latest/your_bag.html)

link02: [ROS探索总结（十二）—— 坐标系统](https://www.guyuehome.com/265)

link03: [ROS学习记录⑤：TF工具的使用与练习](https://www.guyuehome.com/20269)





---

## 2. 2021年9月25日：





---

## &&. 参考



