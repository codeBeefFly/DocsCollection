# 【问题】20210331 - dependencies of target lanelet2_traffic_rules

## 简述

lanelet2 依赖没有安装，导致 ts 平台 tda2 模式编译失败

```
Scanning dependencies of target lanelet2_traffic_rules
flow_processor/CMakeFiles/tauristar_flowprocessor.dir/build.make:105: recipe for target 'flow_processor/CMakeFiles/tauristar_flowprocessor.dir/__/apa_full_stack/ApaInfoProcessIF.cpp.o' failed
make[2]: *** [flow_processor/CMakeFiles/tauristar_flowprocessor.dir/__/apa_full_stack/ApaInfoProcessIF.cpp.o] Error 1
make[2]: *** Waiting for unfinished jobs....
In file included from /home/ds16/work/TauristarPlatform/src/tauristar_platform/apa_full_stack/APA_Process/ApaInfoProcess.h:3:0,
                 from /home/ds16/work/TauristarPlatform/src/tauristar_platform/apa_full_stack/apa_operations.cpp:27:
/home/ds16/work/TauristarPlatform/src/tauristar_platform/apa_full_stack/APA_ParkSpaceGridmap/APA_ParkSpaceGridmap.h:307:9: error: ‘Rect2f’ in namespace ‘cv’ does not name a type
     cv::Rect2f m_HMIRoi1;
         ^
/home/ds16/work/TauristarPlatform/src/tauristar_platform/apa_full_stack/APA_ParkSpaceGridmap/APA_ParkSpaceGridmap.h:308:9: error: ‘Rect2f’ in namespace ‘cv’ does not name a type
     cv::Rect2f m_HMIRoi2;
         ^
flow_processor/CMakeFiles/tauristar_flowprocessor.dir/build.make:128: recipe for target 'flow_processor/CMakeFiles/tauristar_flowprocessor.dir/__/apa_full_stack/apa_operations.cpp.o' failed
make[2]: *** [flow_processor/CMakeFiles/tauristar_flowprocessor.dir/__/apa_full_stack/apa_operations.cpp.o] Error 1
CMakeFiles/Makefile2:4259: recipe for target 'flow_processor/CMakeFiles/tauristar_flowprocessor.dir/all' failed
make[1]: *** [flow_processor/CMakeFiles/tauristar_flowprocessor.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[  9%] Built target grid_map_core
[ 10%] Built target lanelet2_traffic_rules
[ 17%] Built target GeographicLib_SHARED
[ 19%] Built target lanelet2_io
[ 20%] Built target lanelet2_routing
[ 22%] Built target lanelet2_core
Makefile:149: recipe for target 'all' failed
make: *** [all] Error 2

```

---



## 解决

链接：[](https://github.com/fzi-forschungszentrum-informatik/Lanelet2)

```
link: https://github.com/fzi-forschungszentrum-informatik/Lanelet2

---

for ros-dependencies

sudo apt-get install ros-melodic-rospack ros-melodic-catkin ros-melodic-mrt-cmake-modules

---

for lanelet2 dependencies

sudo apt-get install libboost-dev libeigen3-dev libgeographic-dev libpugixml-dev libpython-dev libboost-python-dev python-catkin-tools
```





