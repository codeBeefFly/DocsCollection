# 20211208_imu_ros_devel_refinement

[toc]



---

logs:



---



## 01. 2021年12月8日：imu ros node refinement



#### task00: ros devel basics



#### task01: xxx





### 参考

link00: [ROS setup tutorial](https://www.jetbrains.com/help/clion/ros-setup-tutorial.html)

---



## 02. xxx





这篇日志不做更新并保留。

在练习的过程中进行 imu node refinement。





```
ds18@ubuntu:~/Work_DC/TSP_RUN/tmp$ tree .
.
└── tauristar_platform
    ├── apa_full_stack
    │   ├── APA_CarPath
    │   │   └── APA_CarPath.h           Thu Sep 23 19:26:53 2021 +0800
    │   ├── ApaInfoProcessIF.h          Thu Dec 2 15:03:50 2021 +0800
    │   ├── APA_ParkSpaceGridmap
    │   │   ├── APA_ParkSpaceGridmap.h  Thu Dec 2 15:03:50 2021 +0800
    │   │   ├── InlineFunction.h 
    │   │   ├── Kernel_FindPoint.h
    │   │   └── RefinePoints.h
    │   ├── defish
    │   │   ├── defisher.h
    │   │   └── yuv_mat.h
    │   ├── grid_map
    │   │   ├── apa_util.h
    │   │   ├── apa_utils.h
    │   │   ├── clipper.h
    │   │   ├── data_type.h
    │   │   └── GridMapAPA.h
    │   ├── mod
    │   │   └── MOD.h
    │   ├── param
    │   │   ├── alg_param.h
    │   │   ├── apa_define.h
    │   │   ├── car_param.h
    │   │   ├── car_path_param.h
    │   │   ├── defish_param.h
    │   │   ├── grid_map_param.h
    │   │   └── log_file.h
    │   ├── path
    │   │   ├── calVehraw.h
    │   │   ├── CANBus.h
    │   │   └── ExtendKalman.h
    │   ├── spdlog
    │   │   ├── common.h
    │   │   ├── common-inl.h
    │   │   ├── details
    │   │   │   ├── backtracer.h
    │   │   │   ├── backtracer-inl.h
    │   │   │   ├── circular_q.h
    │   │   │   ├── console_globals.h
    │   │   │   ├── file_helper.h
    │   │   │   ├── file_helper-inl.h
    │   │   │   ├── fmt_helper.h
    │   │   │   ├── log_msg_buffer.h
    │   │   │   ├── log_msg_buffer-inl.h
    │   │   │   ├── log_msg.h
    │   │   │   ├── log_msg-inl.h
    │   │   │   ├── mpmc_blocking_q.h
    │   │   │   ├── null_mutex.h
    │   │   │   ├── os.h
    │   │   │   ├── os-inl.h
    │   │   │   ├── pattern_formatter.h
    │   │   │   ├── pattern_formatter-inl.h
    │   │   │   ├── periodic_worker.h
    │   │   │   ├── periodic_worker-inl.h
    │   │   │   ├── registry.h
    │   │   │   ├── registry-inl.h
    │   │   │   ├── synchronous_factory.h
    │   │   │   ├── thread_pool.h
    │   │   │   └── thread_pool-inl.h
    │   │   ├── fmt
    │   │   │   ├── bin_to_hex.h
    │   │   │   ├── bundled
    │   │   │   │   ├── chrono.h
    │   │   │   │   ├── color.h
    │   │   │   │   ├── compile.h
    │   │   │   │   ├── core.h
    │   │   │   │   ├── format.h
    │   │   │   │   ├── format-inl.h
    │   │   │   │   ├── LICENSE.rst
    │   │   │   │   ├── locale.h
    │   │   │   │   ├── ostream.h
    │   │   │   │   ├── posix.h
    │   │   │   │   ├── printf.h
    │   │   │   │   ├── ranges.h
    │   │   │   │   └── safe-duration-cast.h
    │   │   │   ├── fmt.h
    │   │   │   └── ostr.h
    │   │   ├── formatter.h
    │   │   ├── logger.h
    │   │   ├── logger-inl.h
    │   │   ├── sinks
    │   │   │   ├── android_sink.h
    │   │   │   ├── ansicolor_sink.h
    │   │   │   ├── ansicolor_sink-inl.h
    │   │   │   ├── base_sink.h
    │   │   │   ├── base_sink-inl.h
    │   │   │   ├── basic_file_sink.h
    │   │   │   ├── basic_file_sink-inl.h
    │   │   │   ├── daily_file_sink.h
    │   │   │   ├── dist_sink.h
    │   │   │   ├── dup_filter_sink.h
    │   │   │   ├── msvc_sink.h
    │   │   │   ├── null_sink.h
    │   │   │   ├── ostream_sink.h
    │   │   │   ├── rotating_file_sink.h
    │   │   │   ├── rotating_file_sink-inl.h
    │   │   │   ├── sink.h
    │   │   │   ├── sink-inl.h
    │   │   │   ├── stdout_color_sinks.h
    │   │   │   ├── stdout_color_sinks-inl.h
    │   │   │   ├── stdout_sinks.h
    │   │   │   ├── stdout_sinks-inl.h
    │   │   │   ├── syslog_sink.h
    │   │   │   ├── systemd_sink.h
    │   │   │   ├── wincolor_sink.h
    │   │   │   └── wincolor_sink-inl.h
    │   │   ├── spdlog.h
    │   │   ├── spdlog-inl.h
    │   │   ├── tweakme.h
    │   │   └── version.h
    │   └── yaml_util.h
    ├── avm_algo_base
    │   ├── CameraTransform.h
    │   ├── dlttransform.h
    │   ├── DLTtransform_LMS.h
    │   ├── doublematrix.h
    │   ├── eyexmap.h
    │   ├── fishrectify.h
    │   ├── lsd.h
    │   ├── Math_matrix.h
    │   ├── multicamera.h
    │   ├── nearRangeTransform.h
    │   ├── point2D.h
    │   ├── polynomialfitting.h
    │   ├── SingleCameraTransform.h
    │   ├── templatematrix.h
    │   └── vehicleline.h
    ├── avm_bvclib
    │   ├── avm_bvc_engine.h
    │   └── bright_alg.h
    ├── avm_calib_lib
    │   ├── bvsCalibAlg.h
    │   ├── deavm_edgelet.h
    │   └── preprocessing.h
    ├── avm_dcal_lib
    │   ├── bev_image_gen.h
    │   ├── dcal_lines.h
    │   ├── dcal_types.h
    │   ├── DynamicCalibEngine.h
    │   └── DynamicCalibEngineIF.h
    ├── avm_lib
    │   ├── apa_data.h
    │   ├── camerainit.h
    │   ├── gl3DModel.h
    │   ├── lut_fixedgrid_2d.h
    │   ├── ResidueImageStitch.h
    │   ├── rv3dmodel.h
    │   ├── single3dmodel.h
    │   └── singleudis.h
    └── avm_util
        ├── basic_log.h
        ├── bv3d_xml.h
        ├── lens.h
        ├── log_util.h
        └── xmlResolve.h

20 directories, 137 files

```

