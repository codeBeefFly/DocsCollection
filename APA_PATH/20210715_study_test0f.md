# test0f.yaml analysis

[toc]



---



## yaml code



```yaml

params:
  thread_num: 4 # Use to keep a good consistency.
  yaml_variables:
    number:  # We use double to store all number variables.
        # The variable names can be given by users, not related to the source code.
        - commonFrameRate: 20.
    string:
      - inputVideo1: ./data/Videos/200.mp4
      - inputVideo2: ./data/Videos/201.mp4
      - inputCanbusTxt: /data/canbus.txt
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
      wheel_edgconfig_filee: 43
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
    	
    	#---## control_operations.h/.cpp
    	
    	#---#
    	# * @brief The operation generates a string layer from input data queue
    	# * 从 输入数据列总 生成 string layer
    	# ouput: boost::shared_ptr<tauristar::LayerWrapper> out_layer 
    	# = m_holder_seq->c(m_output_name);
    	# 通过 createCurrentLayer 作为 当前 layer 的 output
    	# 通过 LayerWrapper 查看 layer 类型，一个 layer 只有一种类型，enum LayerType
    	#---#
      - key: SubInputData
        output: not_used_txt_str_layer  # createCurrentLayer
        
        #---#
    	# * @brief A SaveParkTargetIntoState operation
 		# * if no target_pose is specified or input_data: 1,  				
 		# * read from sharedstate
        # * Save a new target into State to guide path computing.
        # 将泊车目标保存到 stat 中，当条件需要，从 sharedstate 读取泊车坐标
        # ouput: boost::shared_ptr<tauristar::LayerWrapper> out_layer 
    	# = m_holder_seq->createCurrentLayer(m_output_name);
    	# 通过 createCurrentLayer 作为 当前 layer 的 output
    	#---#
      - key: SetPose
        pose: {x: 0.0, y: 0.0, theta: 0.0}
        relative_position: 0  # bool: 0-相机坐标系/1-汽车坐标系(相对坐标系)
        output: start_pose_layer  # 这个是 output layer 的名称值 createCurrentLayer

		#---#
		# * @brief A SaveCurPoseIntoState operation
 		# * Save current pose into State
  		# * Used in the end of localization modules.
  		# 将当前位姿保存到 state 中
  		# 这个 layer 需要 上一个 layer 的输出（start_pose_layer）作为输入
  		# * boost::shared_ptr<tauristar::LayerWrapper> in_layer
        # * = m_holder_seq->getCurrentLayer(m_input_name);
		#---#
      - key: SaveCurPoseIntoState
        input: start_pose_layer  # input layer name,从上一个 layer 读取数据 getCurrentLayer
        save_once: 1  # Save only once for set start pose

      - key: APAReadTxt
        ignore: 1  #DVR
        input: H002_DVR/DVR_2/left.txt
        output: txt_str_layer
        accurate_time: 1

      - key: APAInfoProcess
        ignore: 1  # DVR
        coordinate_cam_file:
          left: H002_DVR/apa_left_cam.dat
          right: H002_DVR/apa_right_cam.dat
          rearconfig_file: H002_DVR/apa_rear_cam.dat
          front: H002_DVR/apa_front_cam.dat
        car_param: H002_DVR/car_param.yaml
        video_file: 
          left_h264: H002_DVR/DVR_2/left.h264
          left_hdr: H002_DVR/DVR_2/left.hdr
          right_h264: H002_DVR/DVR_2/right.h264
          right_hdr: H002_DVR/DVR_2/right.hdr
        #input_cam: [left_cam, right_cam]
        input_txt: txt_str_layer
        pub_pose: 1
        show_image: 1
        output_resolution: 0.2
        output: grid_map_layer
        enable_tsp: 1

		#---## apa_operations.h/.cpp
		
		#---#
		# * @brief An operation to save ApaInfoProcess gridmap as Occupancy grid 
		# * in image layer.
		#---#
      - key: FetchGridMapFromApaInfoProcess
        output: grid_map_layer  # output as layer name, createCurrentLayer
        output_resolution: 0.2
        draw_parking_lines: 0  # 0: lines, 2: corners
        
        #---## odom_operations.h/.cpp
        
        #---#
        # * @brief A ComputeOdom operation
 		# * Feed current pose to run odometry localization 
        #---#
      - key: ComputeOdom
        #ignoconfig_filere: 1
        set_pose_state: 1
        use_steeringwheel: 1
        steer_lookup: 0
        #input: txt_str_layer
        output: odom_pose_layer  # createCurrentLayer

		#---## apa_operations.h
		
        #---#
        # * @brief An operation to publish pose from APAInfoProcess
        #---#
      - key: ApaInfoProcessPublishPose
      	# set_pose_state: 1
        output: apa_pose_layer  # createCurrentLayer

		#---## control_operations.h
		
		# 1
        #---#
        # * @brief A PrintPoseLayer operation
 		# * Print the pose in the input layer.
 		# 在输入层打印位姿
        #---#
      - key: PrintPoseLayer
        #ignore: 1
        input: apa_pose_layer  # getCurrentLayer(m_input_name)
        header: APAPose  # 

        # 2
        #---#
        # 同上
        #---#
      - key: PrintPoseLayer
        #ignore: 1
        input: odom_pose_layer
        header: Odom

        # 3
        #---#
        # * @brief A LoadCurPoseFromState operation
		# * Load Current State Pose in to a pose layer
		# 将当前状态位姿载入到pose层
        #---#
      - key: LoadCurPoseFromState
        #ignore: 1
        output: cur_pose_layer  # 创建层并输出此层，createCurrentLayer(m_output_name)

        # 4
        #---#
        # 同前
        #---#
      - key: PrintPoseLayer
        #ignore: 1
        input: cur_pose_layer
        header: Current

		#---## obstacle_operations.h/.cpp

        # 5
        #---#
        # * @brief An operation to get radar data from sharedstate and convert
        # * to point layer
        # 从 sharedstate 获取雷达数据并将之转换到 point layer
        #---#
      - key: ProcessRadarData
        input_map: grid_map_layer
        output_map: obstacle_map_layer
        output_points: obstacle_layer
        sweep_mode: 1
        scan_front: 1

		#---## parkingspot_ops.h/.cpp
		# 没有看懂 ProcedureFactory.h + OperationsStep.h 关于 node 中 key class 的调用。
		# 问题： yaml 中的 key 是如何被调用的？

        # 6
        #---#
        # * @brief An operation to maintain middle targets for parking
        # 获取泊车中中间点的操作
        #---#
      - key: ParkingMiddleGoals
        log_level: 5
        output: middle_goal_pose
        output_path: expect_path
        parking_mode: 5
        steering_angle: 0.35
        #parking_spot_length: 4.85
        #parking_spot_width: 6.1
        lane_width: 6
        perp_distance: 3
        para_distance: 4
        goal1_angle: 0.262
        angle_tolerance: 0.262 # 15 deg
        distance_tolerance: 0.3
        iterations: 1
        save_to_apa: 1
        update_goal: 1
        apa_mode: 1
        use_astar: 0

        # 7
        #---#
        # * @brief A ComputePath operation
 		# * Need map, obstacles, cur_pose, target_pose info.
 		# * If all input information is not changing compared to previous_frame,
 		# * then directly re-use the previous_step result.
 		# * This is a full-path "from starting to target" operation.
 		# * We will also have ShortTermPathStep for middle changing path.
 		# 计算路线（从起始到目标点）。
 		# **********
 		# -key: xxx
 		# $$===> OperationProcedure.cpp
        # if (one_step_node.IsMap() && checkKey(one_step_node, "key")) {
        # 	try {
        # 		m_operation_steps.push_back(StepRegistry::CreateStep(one_step_node));
 		# 		...
 		#	}
 		# }
 		# $$===> 此代码用来注册 yaml 中 （thread or service） procedure 的 key-steps，
 		# one step a head，但是 ignore 参数不是在 key-step类 中获取的，是在 OperationProcedure
 		# 类 中获取的；同时，key-step类 是在 此类中推入 m_operation_steps 中，类型为
 		# std::vector<boost::shared_ptr<OperationStep> >
 		# The parameter will be used in setup function. 
        #---#
      - key: ComputePath  # todox2
        ignore: 1
        input: middle_goal_pose  # getCurrentLayer
        eval_result: eval_result
        output: expect_path  # createCurrentLayer
        simple_path_type: RS
        backward: 1
        max_steer: 0.42
        #check_collision: 2
        min_incr_dist: 0.05
        map_layer: grid_map_layer
        use_local: 0
        time_bound: 10.0
        save_path: 1
        save_map: computepath_img_layer
        apa_mode: 1
        wait_for_map: 1
        car_scale: 0.78

		# apa_operations.h/.cpp
		
		# 8
        #---#
        # * @brief An operation to save computed path to APAInfoProcess
        # 将 computed path 保存到 apainfoprocess
        # PointsLayerPtr in_path_ptr = in_path_layer->getPointsLayerPtr();
        # 一个指向 PointsLayer 的智能指针，每一个 xxxlayer 都是一个独立的 layer类，可以简单的
        # 看成一个公共的数据结构，在不同的 step 之间进行传递。
        #
        # $$===》layer 类型
        # __ImageLayer,  		// A sequnce of pixels.
        # __PointsLayer,  		// A sequence of points.
        # __RectanglesLayer, 	// A sequence of rectangles.
        # __CameraLayer, 		// A layer to hold CameraInfo (internal params) 
        # 						// and Pose3D(CameraExtenalParams).
        # __StringsLayer,    	// A sequence of strings.
        # __ContourPtsLayer, 	// A sequence of points with contour_info.
        # __PolygonsLayer,  	// A sequence of Rings
    	#
    	# boost::shared_ptr<ImageLayer> m_image_layer;
    	# boost::shared_ptr<PointsLayer> m_points_layer;
    	# boost::shared_ptr<PolygonsLayer> m_polygons_layer;
    	# boost::shared_ptr<CameraLayer> m_camera_layer;
    	# boost::shared_ptr<StringsLayer> m_strings_layer; 
        #
        # $$===> transformFrame() 这个函数坐标系输入输出是是什么
        # 问题见截图
        #---#
      - key: SavePathToApaInfoProcess
        input_path: expect_path  # getCurrentLayer(m_input_path_name)

        #---## trajectory_operation.h/.cpp
        
        # 9
        #---#
        # * @brief A TrajectoryEngine operation
 		# * It is doing computeTrajectory, and handle the vehicle state logic.
        # * Based on vehicle State, the engine will computeTraj suitably, 
        # * and handle SendingTrajectory suitably.
 		# * The vehicle state may be 
 		# *  waitingForTask, 
 		# *  Driving, 
 		# *  TaskFailed, 
 		# *  WaitingForTaskInternal   (Maybe some special interact.)
 		# *  DrivingSlowDown  (Even Slower than normal slow_speed due to obstacle nearby.)
 		# *  Braking          (Due to obstacle on the path.)
        # 处理 轨迹计算，与车体状态逻辑。
        # 车体状态参考 vehicle state
        # 
        # $$===> vip: UniqueVehicleStatus: 是否可以通过状态值确定停止标志
        # $$===> vip: TrajectoryEngine: adding correction trajecotory 是否需要条件启动
        #---#
      - key: TrajectoryEngine
        path_input: expect_path
        min_dist: 0.03
        max_acc: 0.5
        obstacle_input: obstacle_layer
        sweep_area: sweep_layer
        front_ext: 0.2
        side_ext: 0.2
        time_step: 0.05

        # 10
        #---#
        # * @brief A ComputeControlValue operation
 		# * 
 		# * based on the CurPose and TrajectoryPool from sharedState.
 		# * Use qp_solver to obtain an expected control_value.
        # 计算控制值，基于 curpose + trajecotrypool
        #---#
      - key: ComputeControlValue
        log_level: 4
        output: control_value
        expect_pose: expect_pose_layer
        traj_debug: traj_cache_layer
        delta_gain: 2
        slow_turn: 2
        catchup_cache: 1
        min_speed: 0.1
        time_step: 0.05
        goal_correction: 1 #correction

        # 11
        #---#
        #
        #---#
      - key: PubControl
        input: control_value
        speed_scale: 1
        steer_scale: 1
        max_steer: 0.5
        max_speed: 0.4
       # always_publish: 1 
  
         
  - thread_name: runtime_thread
    frame_rate: 0.1
    thread_procedure:
        - key: RuntimeReport
          report_dir: runtime_report.txt
```



SavePathtoApaInfoProcessStep::Forward_cpu

![image-20210717132438001](20210715_study_test0f.assets/image-20210717132438001.png)

这三个坐标系中分别是什么？（SavePathtoApaInfoProcessStep::Forward_cpu）



ComputeControlValueStep::computeControlCommand

![image-20210717140229453](20210715_study_test0f.assets/image-20210717140229453.png)



还是在 ComputeControlValueStep::Forward_CPU 中

![image-20210717162818191](20210715_study_test0f.assets/image-20210717162818191.png)

这段代码可以将控制信号转换成 ts 状态信号。



UniqueVehhicleState:

![image-20210717140310322](20210715_study_test0f.assets/image-20210717140310322.png)



通过 trajectory cache 可以看到车位坐标没有变动，说明泊车完成，但是有没有泊车完成的标识呢？

有，在 PubControlStep::Forward 中。

```
[1970-01-01 00:01:47.645] [info] PubControl speed=0.0 steer=0.344179 expected_distance=-1.0 status=1

这段日志是通过这段代码得到：

    if (m_log_level>=1) PRINT_LOG("PubControl speed={} steer={} expected_distance={} status={}", speed_pub_data, steer_pub_data, expected_dist, status_pub_data);

关于 status_pub_data 的值：

    int8_t status_pub_data = 0;
    // goal reached
    if (controller_status == (int8_t)UniqueVehicleState::Controller_FINALIZED) {
        status_pub_data = 3;
    }
    // waiting for task
    if (planner_status == (int8_t)UniqueVehicleState::V_WAITING_FOR_TASK) {
        status_pub_data = 1;
    }
    // driving
    else if (planner_status == (int8_t)UniqueVehicleState::V_DRIVING) {
        status_pub_data = 2;
    }
    // braking / failure
    if (controller_status == (int8_t)UniqueVehicleState::Controller_ERROR) {
        status_pub_data = 4;
    }

```

关于 status_pub_data，当前算法有两个值：2：driving；1：waiting for task。需要将第二个1：waiting for task 改成3：goal reached，并传送到sharedstate。

是否可以通过这段代码来思考停止信号？

![image-20210717155517986](20210715_study_test0f.assets/image-20210717155517986.png)





![image-20210717140417277](20210715_study_test0f.assets/image-20210717140417277.png)





---



## cpp keyword



### 1. define

```c++
#define REGISTER_STEP_CREATOR(key, creator)                     \
    static StepRegisterer g_creator_f_##key(#key, creator);     \


#define REGISTER_STEP_CLASS(key)                                               \
    boost::shared_ptr<OperationStep> Creator_##key##Step(const StepParameter& param)    																	 \
    {                                                                          \
        return boost::shared_ptr<OperationStep>(new key##Step(param));         \
    }                                                                          \
    REGISTER_STEP_CREATOR(key, Creator_##key##Step)

```





### 2. 函数的引用

>函数其实也可以看作一种类型， 由返回类型和形参表确定的类型， 故函数也向普通类型一样有函数名，函数指针，函数引用三种使用方式。
>
>参考：[C++：函数指针与函数引用](https://blog.csdn.net/xiaoyaohuqijun/article/details/48771071)

![image-20210717115941037](20210715_study_test0f.assets/image-20210717115941037.png)



### 3. TBC



