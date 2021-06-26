# **processor_node** pc mode 操作记录



[toc]

---



## 1. 说明

processor_node 是用来对 apa 的 dvr 进行 pc 端模拟的程序，适合用来对 apa 的代码进行分析、调试以及修改。

具体来说，它用于：

1. 在 apa 代码更新之后，通过 pc 端模拟的结果，用于对代码进行分析、修改。
2. 在 apa 代码更新之前，可以不断的通过 pc 模拟进行功能代码的开发。



processor_node 的构建与编译流程

```
# 1. 保存 TSP (TauristarPlatform) 本地的修改
git stash

# 2. 主分支更新本地代码
git pull

# 3. 弹出保存的本地修改
git stash pop

# 4. 检查当前的版本
git rev-parse HEAD

# 5. 在 xxx/tauristar_platform/ 下创建文件夹，进入文件夹
mkdir build_[日期:MMYY]_[commit]
cd build_[日期:MMYY]_[commit]

# 6. cmake 构建与编译
cmake .. -DBUILD_WITH_ROS=OFF -DCMAKE_BUILD_TYPE=DEBUG
make -j 8
```



编译完成之后，会在 `xx/tauristar_platform/build_xxx/bin/`中生成 `processor_node` 二进制可执行程序，运行命令示例:

```
processor_node -o conf=test0f.yaml
```





## 2.  案例: 通过 processor_node 分析多步泊车 dvr



### 2.1. 准备



| 目的   | 通过 `processor_node` 生成 pc 端的 `computed_path*.txt` + `pc_log.log` 文件用于分析泊车效果。 |
| ------ | ------------------------------------------------------------ |
| 文件   | dvr_0511_mode5_0_1                                           |
| TS版本 | afaf309ee6590081f2da3d83acb97872d4109c71                     |



dvr 文件以及必须的 `processor_node_xxx`

路径: `xxx\pc\`

![image-20210512195442652](/home/ds16v2/.config/Typora/typora-user-images/image-20210512195442652.png)



环视相机的标定参数文件`xxx.dat`，`BV3D.xml`

路径: `xxx\IC421\`

![image-20210512195505862](/home/ds16v2/.config/Typora/typora-user-images/image-20210512195505862.png)





### 2.2. 配置 test0f.yaml 文件

当前的 TSP 编译出来的 `processor_node` 在 PC 上运行 apa 多步泊车时，无法通过一个 `test0f.yaml` 文件同时生成所有的 `computed_path*.txt` 文件，需要对每一个计算出来的 goal 坐标配置一个 `test0f.yaml` 文件。

![image-20210512201749311](/home/ds16v2/.config/Typora/typora-user-images/image-20210512201749311.png)



如图，在进行 apa parking middle goal mode: 5 进行模拟时，rs 算法会在 start pose 与 final goal pose 坐标之间规划出 5条路径，

文件配置如下，与对应生成的 `computed_path*.txt`：

| path 1     | start ---> goal 1      | test0f_01.yaml     | computed_path_01.txt     |
| ---------- | ---------------------- | ------------------ | ------------------------ |
| **path 2** | **goal 1 ---> goal 2** | **test0f_12.yaml** | **computed_path_12.txt** |
| **path 3** | **goal 2 ---> goal 3** | **test0f_23.yaml** | **computed_path_23.txt** |
| **path 4** | **goal 3 ---> goal 4** | **test0f_34.yaml** | **computed_path_34.txt** |
| **path 5** | **goal 4 ---> goal 5** | **test0f_45.yaml** | **computed_path_45.txt** |





### 2.3. test0f.yaml/test0f_01.yaml 文件

使用 `processor_node` 进行 pc 端模拟多步泊车时，需要使用一个 `test0f.yaml 或 test0f_01.yaml` 来生成

| 1     | **computed_path_01.txt** | **start 到 goal 1 的路径** |
| ----- | ------------------------ | -------------------------- |
| **2** | **naive_*.txt**          |                            |
| **3** | **pc_log.log**           | **完整的日志文件**         |
| **4** | **其他各种文件**         | **略**                     |



**test0f.yaml/test0f_01.yaml** 参考文件

```yaml
params:
  thread_num: 4 # Use to keep a good consistency.
  yaml_variables:
    number:  # We use double to store all number variables.
        # The variable names can be given by users, not related to the source code.
        - commonFrameRate: 100.
    string:
      - inputVideo1: ./data/Videos/200.mp4
      - inputVideo2: ./data/Videos/201.mp4
      - inputCanbusTxt: canbus.txt
      - uartAddress: /dev/Camera1
  program_params:
    # the parameters names are defined in the program already.
    carLength: 6.2
    tireRadius: 0.7157
    max_steering_wheel_velocity_delta: 0.1
    max_steer_angular_velocity_delta: 0.2
    max_tangential_velocity: 0.35
    error_feedback_gains_x: 1
    error_feedback_gains_y: 1
    error_feedback_gains_theta: 1
    error_feedback_gains_phi: 1

  system_params:
    #loglevel: 3
    #logfile: logfile.txt
    #logfile: processor.log
    max_frame_counter: -1
    log_freq_frame_counter: 1
    input_queue_size: 16
    output_queue_size: 16

  car_config_params:
    config_file: car_param.yaml
    
  odometry_params:
    use_mcu: 1
    lut:
      gen_lut: 1
      use_lut: 1
      lut_filename: sin_lut
      n: 100001
    vehicle_info:
      wheel_edge: 43
      wheeledge_max: 255
      rear_axial: 1.702
      wheel_radius: 0.7157
      wheel_speed: 0.05625
      edge2m: [0.052768199, 0.052932105, 0.05298908, 0.052932105]
      edge2m_factor: 1.0
      steer_factor: 0.001
      steer_offset: 0
      wheelbase: 2.825

threads:
  - thread_name: read_txt
    frame_memory: 5
    frame_rate: 20
    thread_procedure:
      - key: SubInputData
        output: not_used_txt_str_layer
          
      -   key: SetPose
          pose: {x: 0.0, y: 0.0, theta: 0.0}
          relative_position: 0 
          output: start_pose_layer

      -   key: SaveCurPoseIntoState
          input: start_pose_layer
          save_once: 1

      -   key: APAReadTxt
          ignore: 0  #DVR
          input: canbus.txt
          output: txt_str_layer
          accurate_time: 1

      -   key: APAInfoProcess
          ignore: 0  # DVR
          coordinate_cam_file:
            left: /home/ds16v2/Work/TS_PC/IC421/apa_left_cam.dat
            right: /home/ds16v2/Work/TS_PC/IC421/apa_right_cam.dat
            rear: /home/ds16v2/Work/TS_PC/IC421/apa_rear_cam.dat
            front: /home/ds16v2/Work/TS_PC/IC421/apa_front_cam.dat
          car_param: car_param.yaml
          video_file: 
            #left_h264: H002_DVR/DVR_2/left.h264
            #left_hdr: H002_DVR/DVR_2/left.hdr
            #right_h264: H002_DVR/DVR_2/right.h264
            #right_hdr: H002_DVR/DVR_2/right.hdr
          #input_cam: [left_cam, right_cam]
          input_txt: txt_str_layer
          pub_pose: 1
          show_image: 1
          output_resolution: 0.2
          output: grid_map_layer
          enable_tsp: 1

      - key: FetchGridMapFromApaInfoProcess
        output: grid_map_layer
        output_resolution: 0.2
        draw_parking_lines: 0
        
      - key: ComputeOdom
        #ignore: 1
        set_pose_state: 1
        use_steeringwheel: 1
        steer_lookup: 0
        #input: txt_str_layer
        output: odom_pose_layer

      - key: ApaInfoProcessPublishPose
      #  set_pose_state: 1
        output: apa_pose_layer

      - key: PrintPoseLayer
        #ignore: 1
        input: apa_pose_layer
        header: APAPose

      - key: PrintPoseLayer
        #ignore: 1
        input: odom_pose_layer
        header: Odom

      - key: LoadCurPoseFromState
        #ignore: 1
        output: cur_pose_layer
        
      - key: PrintPoseLayer
        #ignore: 1
        input: cur_pose_layer
        header: Current
        
#      - key: TrajectoryEvaluation
#        output: eval_result

      - key: ProcessRadarData
        input_map: grid_map_layer
        output_map: obstacle_map_layer
        output_points: obstacle_layer
        sweep_mode: 1
        scan_front: 1

      # - key: ParkingMiddleGoals
      #   log_level: 5
      #   output: middle_goal_pose
      #   parking_mode: 1 # 0: original 5 step method, 1: new 3 step method, 2: new 2 step method
      #   steering_angle: 0.3
      #   parking_spot_length: 4.85
      #   parking_spot_width: 2.5
      #   angle_tolerance: 0.262 # 0.524 (30 deg)
      #   distance_tolerance: 0.3
      #   iterations: 1
      #   save_to_apa: 1
      #   update_goal: 0

      - key: ParkingMiddleGoals
        log_level: 5
        output: middle_goal_pose
        parking_mode: 5 # 0: original 5 step method, 1: new 3 step method, 2: new 2 step method
        steering_angle: 0.3
        use_astar: 0  # 0422 pm5
        #parking_spot_length: 4.85
        #parking_spot_width: 2.5
        lane_width: 6
        perp_distance: 3
        para_distance: 4
        goal1_angle: 0.175
        angle_tolerance: 0.262 # 0.524 (30 deg)
        distance_tolerance: 0
        iterations: 1
        save_to_apa: 1
        update_goal: 0
        apa_mode: 1
        
      - key: ComputePath
        input: middle_goal_pose
        eval_result: eval_result
        output: expect_path
        simple_path_type: RS
        backward: 1
        max_steer: 0.42
        min_incr_dist: 0.05
        map_layer: grid_map_layer
        use_local: 0
        # time_bound: 2.0
        save_path: 1
        save_map: computepath_img_layer
        apa_mode: 1
        wait_for_map: 1
        car_scale: 0.78
        
      - key: SavePathToApaInfoProcess
        input_path: expect_path
        
      #- key: SavePoseToApaInfoProcess
      #  input_path: expect_path 
        
#      - key: DrawPathOnGridMap
#        input_map: grid_map_layer
#        input_path: expect_path
#        output: map_with_path_layer
#        draw_car_pose: 1

#      - key: OpencvSaveImage
#        ignore: 0
#        input: map_with_path_layer
#        output_file: apa_grid_map.png

     # - key: OpencvShowImage
      #  ignore: 0
       # name: computed_path
       # input: grid_map_layer

      - key: TrajectoryEngine
        path_input: expect_path
        min_dist: 0.03
        max_acc: 0.5
        obstacle_input: obstacle_layer
        sweep_area: sweep_layer
        front_ext: 0.2
        side_ext: 0.2
        time_step: 0.05
        
      - key: ComputeControlValue
        output: control_value
        expect_pose: expect_pose_layer
        traj_debug: traj_cache_layer
        delta_gain: 2
        slow_turn: 2
        catchup_cache: 1
        min_speed: 0.1
        time_step: 0.05
        
      - key: PubControl
        input: control_value
        speed_scale: 1
        steer_scale: 1
        max_steer: 0.5
        max_speed: 0.4
       # always_publish: 1 
     
     #send a const control value to make the car turn in a circle 
     # - key: PubControl
     #   input: control_value
     #   dummy_control: 1                                                                                                                                              
     #   dummy_speed: 0.5
     #   dummy_steer: 0.04
     #   speed_scale: 1
     #   steer_scale: 1
         
  - thread_name: runtime_thread
    frame_rate: 1000
    thread_procedure:
        - key: RuntimeReport
          report_dir: runtime_report.txt
```



执行命令：

```
processor_node_xxx -o conf=test0f_01.yaml >> pc_log.log
```





### 2.4. test0f_**.yaml 文件

在使用 `test0f.yaml/test0f_01.yaml`得到 `computed_path_01.txt 和 pc_log.log` 文件后，需要在日志文件 (pc_log.log) 中找到算法规划的路径目标点 (goal point)。在得到这5个目标点之后，需要准备对应的 `test0f_1*.yaml`文件，只用于生成 `computed_path_1*.txt`

![image-20210512204904807](/home/ds16v2/.config/Typora/typora-user-images/image-20210512204904807.png)



虽然可以像执行 `test0f_01.yaml` 一样执行 `test0f_1*.yaml`，但是在执行的过程中，不需要保存执行过程中生成的日子文件，也不需要等待程序读完整个 canbus.txt。可以在生成 `computed_path_1*.txt`之后 `ctrl+c`中断运行。



test0f_**.yaml

```yaml
params:
  thread_num: 4 # Use to keep a good consistency.
  yaml_variables:
    number: # We use double to store all number variables.
      # The variable names can be given by users, not related to the source code.
      - commonFrameRate: 1000.
    string:
      - dataroot: /home/ryan/working_directory/Platform/IC421/
      #- datadir: dvr_IC421_1116_GetPos/dvr_IC421_1116/
      - datadir: 3918_20210325_dvr/dvr_0325_s2_c1_1/
      - canfile: canbus.txt
  program_params:
    # the parameters names are defined in the program already.
    carLength: 6.2
    tireRadius: 0.7157
    max_steering_wheel_velocity_delta: 0.1
    max_steer_angular_velocity_delta: 0.2
    max_tangential_velocity: 0.35
    error_feedback_gains_x: 1
    error_feedback_gains_y: 1
    error_feedback_gains_theta: 1
    error_feedback_gains_phi: 1

  system_params:
    #loglevel: 2
    #logfile: logfile.txt
    #logfile: processor.log
    max_frame_counter: -1
    log_freq_frame_counter: 1
    input_queue_size: 16
    output_queue_size: 16

  car_model_params:
    #FRONT_REAR_WHEEL_LEN: 2.825
    ASTAR_CAR_LEN: 4
    ASTAR_CAR_WIDTH: 1
    REAR_WHEEL_AXIS: 0.8 #car back length

  car_config_params:
  #config_file: /home/ryan/working_directory/Platform/#4056_20210402_dvr/dvr_0402_3/car_param.yaml


threads:
  - thread_name: read_txt
    print_time: 1
    frame_memory: 5
    frame_rate: 1000
    thread_procedure:

      - key: OpencvReadImage
        ignore: 1
        input_file: APA_map_0415.png
        output: image_layer

      - key: ComputeGridMapFromImage
        ignore: 1
        input: image_layer
        output: gridmap_layer
        map_x: -12.2059  #-11.4
        map_y: -15.4215    #-16
        map_theta: 0
        map_resolution: 0.2

      - key: SetPose
        pose: { x: 5.10160, y: 0.072974, theta: -0.133572 }
        relative_position: 0
        output: start_pose_layer

      - key: SetPose
        pose: { x: 1.37758, y: 1.4432, theta: -0.571566 }
        relative_position: 0
        output: goal_pose_layer

      - key: SaveCurPoseIntoState
        input: start_pose_layer

      - key: SaveParkTargetIntoState
        input: goal_pose_layer

      - key: ComputePath
        eval_result: eval_result
        output: expect_path
        simple_path_type: RS
        astar_mode: 0
        backward: 1
        max_steer: 0.42
        min_incr_dist: 0.05
        map_layer: gridmap_layer
        use_local: 1
        #time_bound: 6
        save_path: 1
        save_map: computepath_img_layer
        apa_mode: 1
        wait_for_map: 0
        car_scale: 0.78
        smooth_path: 0

      - key: DrawPathOnGridMap
        ignore: 1
        input_map: gridmap_layer
        input_path: expect_path
        output: map_with_path_layer
        draw_car_pose: 1

      - key: OpencvShowImage
        ignore: 1
        name: car_in_map
        input: map_with_path_layer
```



执行命令

```
processor_node_xxx -o conf=test0f_**.yaml

# 在生成 computed_path_**.txt 之后
ctrl + c
```



> 注意需要修改的路径



---



## 3. 案例： 绘制路径效果

dvr_0511_mode5_0_1 的绘制效果

left: pc: computed_path only

right: pc: computed_paht_apa_odom

| ![computed_path](/home/ds16v2/Work/TS_PC/4264_20210511/pc/dvr_0511_mode5_0_1/plots_pc/computed_path.png) | ![computed_path_apa_odom](/home/ds16v2/Work/TS_PC/4264_20210511/pc/dvr_0511_mode5_0_1/plots_pc/computed_path_apa_odom.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |



对比 computed_path real car

| <img src="/home/ds16v2/Work/TS_PC/4264_20210511/dvr_0511_mode5_0_1/plots/plot1.png" alt="plot1" style="zoom:72%;" /> |
| ------------------------------------------------------------ |

