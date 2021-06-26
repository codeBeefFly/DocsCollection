# 20210423: ros-env collection

[toc]



---

## 问题 1：unable to locate package ***

```
ds16v2@ds16v2:~$ sudo apt install ros-kinetic-urdf  ros-kinetic-controller-interface  ros-kinetic-base-local-planner  ros-kinetic-cv-bridge  ros-kinetic-image-geometry  ros-kinetic-image-transport  ros-kinetic-image-view  ros-kinetic-xacro  ros-kinetic-navfn  ros-kinetic-teb-local-planner ros-kinetic-fake-localization ros-kinetic-joy ros-kinetic-joint-state-controller ros-kinetic-pcl-ros ros-kinetic-pcl-conversions ros-kinetic-tf2-geometry-msgs ros-kinetic-cmake-modules ros-kinetic-rqt-gui-py ros-kinetic-rqt-gui ros-kinetic-rosbridge-server ros-kinetic-image-transport-plugins ros-kinetic-image-transport
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package ros-kinetic-urdf
E: Unable to locate package ros-kinetic-controller-interface
E: Unable to locate package ros-kinetic-base-local-planner
E: Unable to locate package ros-kinetic-cv-bridge
E: Unable to locate package ros-kinetic-image-geometry
E: Unable to locate package ros-kinetic-image-transport
E: Unable to locate package ros-kinetic-image-view
E: Unable to locate package ros-kinetic-xacro
E: Unable to locate package ros-kinetic-navfn
E: Unable to locate package ros-kinetic-teb-local-planner
E: Unable to locate package ros-kinetic-fake-localization
E: Unable to locate package ros-kinetic-joy
E: Unable to locate package ros-kinetic-joint-state-controller
E: Unable to locate package ros-kinetic-pcl-ros
E: Unable to locate package ros-kinetic-pcl-conversions
E: Unable to locate package ros-kinetic-tf2-geometry-msgs
E: Unable to locate package ros-kinetic-cmake-modules
E: Unable to locate package ros-kinetic-rqt-gui-py
E: Unable to locate package ros-kinetic-rqt-gui
E: Unable to locate package ros-kinetic-rosbridge-server
E: Unable to locate package ros-kinetic-image-transport-plugins
E: Unable to locate package ros-kinetic-image-transport

```



### solution:

```
ds16v2@ds16v2:~$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu &(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
[sudo] password for ds16v2: 

ds16v2@ds16v2:~$ cat /etc/apt/sources.list.d/ros-latest.list
deb http://packages.ros.org/ros/ubuntu &(lsb_release -sc) main
ds16v2@ds16v2:~$ sudo apt-get update
Ign:1 http://packages.ros.org/ros/ubuntu &(lsb_release InRelease
Hit:2 http://ppa.launchpad.net/graphics-drivers/ppa/ubuntu xenial InRelease
Ign:3 http://packages.ros.org/ros/ubuntu &(lsb_release Release                                      
Ign:4 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 Packages                          
Hit:5 http://ppa.launchpad.net/nilarimogard/webupd8/ubuntu xenial InRelease
Ign:6 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) i386 Packages                           
Ign:7 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) all Packages                            
Hit:8 http://ppa.launchpad.net/ubuntu-toolchain-r/test/ubuntu xenial InRelease
Ign:9 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en_US
Ign:10 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en
Ign:11 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 DEP-11 Metadata
Ign:12 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) DEP-11 64x64 Icons
Hit:13 https://typora.io/linux ./ InRelease
Ign:14 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 Packages
Ign:15 http://packages.ros.org/ros/ubuntu &(lsb_release/main i386 Packages
Ign:16 http://packages.ros.org/ros/ubuntu &(lsb_release/main all Packages
Ign:17 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en_US
Ign:18 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en
Ign:19 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 DEP-11 Metadata
Ign:20 http://packages.ros.org/ros/ubuntu &(lsb_release/main DEP-11 64x64 Icons
Ign:4 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 Packages
Ign:6 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) i386 Packages
Ign:7 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) all Packages
Ign:9 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en_US
Ign:10 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en
Ign:11 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 DEP-11 Metadata
Hit:21 http://mirrors.ustc.edu.cn/ubuntu xenial InRelease
Hit:22 http://mirrors.ustc.edu.cn/ubuntu xenial-updates InRelease        
Hit:23 http://mirrors.ustc.edu.cn/ubuntu xenial-backports InRelease      
Hit:24 http://mirrors.ustc.edu.cn/ubuntu xenial-security InRelease
Ign:12 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) DEP-11 64x64 Icons
Ign:14 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 Packages
Ign:15 http://packages.ros.org/ros/ubuntu &(lsb_release/main i386 Packages
Ign:16 http://packages.ros.org/ros/ubuntu &(lsb_release/main all Packages
Ign:17 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en_US
Ign:18 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en
Ign:19 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 DEP-11 Metadata
Ign:20 http://packages.ros.org/ros/ubuntu &(lsb_release/main DEP-11 64x64 Icons
Ign:4 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 Packages
Ign:6 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) i386 Packages
Ign:7 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) all Packages
Ign:9 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en_US
Ign:10 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en
Ign:11 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 DEP-11 Metadata
Ign:12 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) DEP-11 64x64 Icons
Ign:14 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 Packages
Ign:15 http://packages.ros.org/ros/ubuntu &(lsb_release/main i386 Packages
Ign:16 http://packages.ros.org/ros/ubuntu &(lsb_release/main all Packages
Ign:17 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en_US
Ign:18 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en
Ign:19 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 DEP-11 Metadata
Ign:20 http://packages.ros.org/ros/ubuntu &(lsb_release/main DEP-11 64x64 Icons
Ign:4 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 Packages
Ign:6 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) i386 Packages
Ign:7 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) all Packages
Ign:9 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en_US
Ign:10 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en
Ign:11 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 DEP-11 Metadata
Ign:12 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) DEP-11 64x64 Icons
Ign:14 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 Packages
Ign:15 http://packages.ros.org/ros/ubuntu &(lsb_release/main i386 Packages
Ign:16 http://packages.ros.org/ros/ubuntu &(lsb_release/main all Packages
Ign:17 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en_US
Ign:18 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en
Ign:19 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 DEP-11 Metadata
Ign:20 http://packages.ros.org/ros/ubuntu &(lsb_release/main DEP-11 64x64 Icons
Ign:4 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 Packages
Ign:6 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) i386 Packages
Ign:7 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) all Packages
Ign:9 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en_US
Ign:10 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en
Ign:11 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 DEP-11 Metadata
Ign:12 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) DEP-11 64x64 Icons
Ign:14 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 Packages
Ign:15 http://packages.ros.org/ros/ubuntu &(lsb_release/main i386 Packages
Ign:16 http://packages.ros.org/ros/ubuntu &(lsb_release/main all Packages
Ign:17 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en_US
Ign:18 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en
Ign:19 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 DEP-11 Metadata
Ign:20 http://packages.ros.org/ros/ubuntu &(lsb_release/main DEP-11 64x64 Icons
Err:4 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 Packages
  404  Not Found [IP: 140.211.166.134 80]
Ign:6 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) i386 Packages
Ign:7 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) all Packages
Ign:9 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en_US
Ign:10 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) Translation-en
Ign:11 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) amd64 DEP-11 Metadata
Ign:12 http://packages.ros.org/ros/ubuntu &(lsb_release/-sc) DEP-11 64x64 Icons
Ign:14 http://packages.ros.org/ros/ubuntu &(lsb_release/main amd64 Packages
Ign:15 http://packages.ros.org/ros/ubuntu &(lsb_release/main i386 Packages
Ign:16 http://packages.ros.org/ros/ubuntu &(lsb_release/main all Packages
Ign:17 http://packages.ros.org/ros/ubuntu &(lsb_release/main Translation-en_US
Reading package lists... Done
W: The repository 'http://packages.ros.org/ros/ubuntu &(lsb_release Release' does not have a Release file.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
E: Failed to fetch http://packages.ros.org/ros/ubuntu/dists/&(lsb_release/-sc)/binary-amd64/Packages  404  Not Found [IP: 140.211.166.134 80]
E: Some index files failed
```

results

```
ds16v2@ds16v2:~$ sudo apt install ros-kinetic-urdf  ros-kinetic-controller-interface  ros-kinetic-base-local-planner  ros-kinetic-cv-bridge  ros-kinetic-image-geometry  ros-kinetic-image-transport  ros-kinetic-image-view  ros-kinetic-xacro  ros-kinetic-navfn  ros-kinetic-teb-local-planner ros-kinetic-fake-localization ros-kinetic-joy ros-kinetic-joint-state-controller ros-kinetic-pcl-ros ros-kinetic-pcl-conversions ros-kinetic-tf2-geometry-msgs ros-kinetic-cmake-modules ros-kinetic-rqt-gui-py ros-kinetic-rqt-gui ros-kinetic-rosbridge-server ros-kinetic-image-transport-plugins ros-kinetic-image-transport
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package ros-kinetic-urdf
E: Unable to locate package ros-kinetic-controller-interface
E: Unable to locate package ros-kinetic-base-local-planner
E: Unable to locate package ros-kinetic-cv-bridge
E: Unable to locate package ros-kinetic-image-geometry
E: Unable to locate package ros-kinetic-image-transport
E: Unable to locate package ros-kinetic-image-view
E: Unable to locate package ros-kinetic-xacro
E: Unable to locate package ros-kinetic-navfn
E: Unable to locate package ros-kinetic-teb-local-planner
E: Unable to locate package ros-kinetic-fake-localization
E: Unable to locate package ros-kinetic-joy
E: Unable to locate package ros-kinetic-joint-state-controller
E: Unable to locate package ros-kinetic-pcl-ros
E: Unable to locate package ros-kinetic-pcl-conversions
E: Unable to locate package ros-kinetic-tf2-geometry-msgs
E: Unable to locate package ros-kinetic-cmake-modules
E: Unable to locate package ros-kinetic-rqt-gui-py
E: Unable to locate package ros-kinetic-rqt-gui
E: Unable to locate package ros-kinetic-rosbridge-server
E: Unable to locate package ros-kinetic-image-transport-plugins
E: Unable to locate package ros-kinetic-image-transport

```

### solution2: （ok）

```
ds16v2@ds16v2:~$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
[sudo] password for ds16v2: 
ds16v2@ds16v2:~$ 
ds16v2@ds16v2:~$ 
ds16v2@ds16v2:~$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
Executing: /tmp/tmp.vygvtH0i7n/gpg.1.sh --keyserver
hkp://keyserver.ubuntu.com:80
--recv-key
C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
gpg: requesting key AB17C654 from hkp server keyserver.ubuntu.com
gpg: key AB17C654: public key "Open Robotics <info@osrfoundation.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
ds16v2@ds16v2:~$ sudo apt-get update
Hit:1 http://mirrors.ustc.edu.cn/ubuntu xenial InRelease
Hit:2 http://mirrors.ustc.edu.cn/ubuntu xenial-updates InRelease
Hit:3 http://mirrors.ustc.edu.cn/ubuntu xenial-backports InRelease
Hit:4 http://mirrors.ustc.edu.cn/ubuntu xenial-security InRelease              
Get:5 http://packages.ros.org/ros/ubuntu xenial InRelease [4,692 B]            
Get:6 http://packages.ros.org/ros/ubuntu xenial/main amd64 Packages [855 kB]
Hit:7 http://ppa.launchpad.net/graphics-drivers/ppa/ubuntu xenial InRelease
Hit:8 https://typora.io/linux ./ InRelease                                                                   
Hit:9 http://ppa.launchpad.net/nilarimogard/webupd8/ubuntu xenial InRelease                                  
Hit:10 http://ppa.launchpad.net/ubuntu-toolchain-r/test/ubuntu xenial InRelease                              
Get:11 http://packages.ros.org/ros/ubuntu xenial/main i386 Packages [626 kB]                                      
Fetched 1,486 kB in 27s (53.6 kB/s)                                                                               
Reading package lists... Done
ds16v2@ds16v2:~$ 

```



---



## 问题 2：catkin_make not installed, use apt install

### solution:

如果已经安装了 ros 环境，那么就需要 source 一下

```
source /opt/ros/kinetic/setup.bash
```



---



## 问题3：find_package(catkin) failed.

> catkin was neither found in the workspace nor in the CMAKE_PREFIX_PATH. One reason may be that no ROS setup.sh was sourced before



### solution:

1. 在 `~/catkin/` 目录下 `catkin_make -DBUILD_WITH_ROS=ON -DCMAKE_BUILD_TYPE=Release`, 编译 ros 项目，这个目录中有 `src/`，会生成 `devel/, build/`。

2. 在 `~/catkin/` 目录下 需要 source 一下 devel/ 目录下的 setup.bash

   `source ~/catkin/devel/setup.bash` 

3. 在 clion.sh 目录下（我的安装路径：`~/Software/clion-2020.3.2/bin`）使用 shell 脚本启动 clion

   `cd ~/Software/clion-2020.3.2/bin && ./clion.sh`

4. 在打开的 clion 中找到 top-level cmake for catkin (ros 项目的 cmakeLists.txt)

   ![image-20210423143331740](/home/ds16v2/.config/Typora/typora-user-images/image-20210423143331740.png)

   右键 CMakeLists.txt --> Reload cmake project (重新载入 cmake 项目)

   ```
   /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/bin/cmake -DCMAKE_BUILD_TYPE=Debug -G "CodeBlocks - Unix Makefiles" /home/ds16v2/catkin/src
   CMake Warning (dev) in CMakeLists.txt:
     No project() command is present.  The top-level CMakeLists.txt file must
     contain a literal, direct call to the project() command.  Add a line of
     code such as
   
       project(ProjectName)
   
     near the top of the file, but after cmake_minimum_required().
   
     CMake is pretending there is a "project(Project)" command on the first
     line.
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Using CATKIN_DEVEL_PREFIX: /home/ds16v2/catkin/src/cmake-build-debug/devel
   -- Using CMAKE_PREFIX_PATH: /home/ds16v2/catkin/devel;/opt/ros/kinetic
   -- This workspace overlays: /home/ds16v2/catkin/devel;/opt/ros/kinetic
   -- Found PythonInterp: /usr/bin/python2 (found suitable version "2.7.12", minimum required is "2") 
   -- Using PYTHON_EXECUTABLE: /usr/bin/python2
   -- Using Debian Python package layout
   -- Using empy: /usr/bin/empy
   -- Using CATKIN_ENABLE_TESTING: ON
   -- Call enable_testing()
   -- Using CATKIN_TEST_RESULTS_DIR: /home/ds16v2/catkin/src/cmake-build-debug/test_results
   -- Found gtest sources under '/usr/src/gmock': gtests will be built
   -- Found gmock sources under '/usr/src/gmock': gmock will be built
   -- Found PythonInterp: /usr/bin/python2 (found version "2.7.12") 
   -- Looking for pthread.h
   -- Looking for pthread.h - found
   -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
   -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
   -- Looking for pthread_create in pthreads
   -- Looking for pthread_create in pthreads - not found
   -- Looking for pthread_create in pthread
   -- Looking for pthread_create in pthread - found
   -- Found Threads: TRUE  
   -- Using Python nosetests: /usr/bin/nosetests-2.7
   -- catkin 0.7.29
   -- BUILD_SHARED_LIBS is on
   -- BUILD_SHARED_LIBS is on
   -- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   -- ~~  traversing 32 packages in topological order:
   -- ~~  - developer_launch
   -- ~~  - ouster_client
   -- ~~  - ouster_viz
   -- ~~  - platform_launch
   -- ~~  - project_gazebo
   -- ~~  - robot_sensors
   -- ~~  - prius_msgs
   -- ~~  - tauristar_msgs
   -- ~~  - ugv_course_gazebo
   -- ~~  - ugv_course_launch
   -- ~~  - audibot_control
   -- ~~  - ouster_description
   -- ~~  - autoware_msgs
   -- ~~  - calibration_gazebo
   -- ~~  - lgsvl_msgs
   -- ~~  - master_control
   -- ~~  - lgsvl_simulator_bridge
   -- ~~  - audibot_gazebo
   -- ~~  - img_transform
   -- ~~  - ouster_gazebo_plugins
   -- ~~  - spot_detection
   -- ~~  - tauristar_platform
   -- ~~  - ouster_ros
   -- ~~  - ugv_course_libs
   -- ~~  - bicycle_state_space
   -- ~~  - set_nav_point
   -- ~~  - ugv_course_gazebo_plugins
   -- ~~  - prius_description
   -- ~~  - car_nav
   -- ~~  - ros_teb_test
   -- ~~  - audibot_description
   -- ~~  - car_demo
   -- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   -- +++ processing catkin package: 'developer_launch'
   -- ==> add_subdirectory(TauristarPlatformRecipe/developer_launch)
   -- +++ processing catkin package: 'ouster_client'
   -- ==> add_subdirectory(car_demo/ouster_example/ouster_client)
   -- Found PkgConfig: /usr/bin/pkg-config (found version "0.29.1") 
   -- Checking for module 'jsoncpp'
   --   Found jsoncpp, version 1.7.2
   -- Catkin env set, compiling ouster_client with ROS
   -- +++ processing catkin package: 'ouster_viz'
   -- ==> add_subdirectory(car_demo/ouster_example/ouster_viz)
   -- The imported target "vtkRenderingPythonTkWidgets" references the file
      "/usr/lib/x86_64-linux-gnu/libvtkRenderingPythonTkWidgets.so"
   but this file does not exist.  Possible reasons include:
   * The file was deleted, renamed, or moved to another location.
   * An install or uninstall procedure did not complete successfully.
   * The installation package was faulty and contained
      "/usr/lib/cmake/vtk-6.2/VTKTargets.cmake"
   but not all the files it references.
   
   -- The imported target "vtk" references the file
      "/usr/bin/vtk"
   but this file does not exist.  Possible reasons include:
   * The file was deleted, renamed, or moved to another location.
   * An install or uninstall procedure did not complete successfully.
   * The installation package was faulty and contained
      "/usr/lib/cmake/vtk-6.2/VTKTargets.cmake"
   but not all the files it references.
   
   -- Catkin env set, compiling ouster_viz with ROS
   -- +++ processing catkin package: 'platform_launch'
   -- ==> add_subdirectory(TauristarPlatformRecipe/platform_launch)
   -- +++ processing catkin package: 'project_gazebo'
   -- ==> add_subdirectory(autonomous_park_ros/project_gazebo)
   -- +++ processing catkin package: 'robot_sensors'
   -- ==> add_subdirectory(autonomous_park_ros/robot_sensors)
   -- Extracting file: hokuyo.dae.tar.gz
   -- +++ processing catkin package: 'prius_msgs'
   -- ==> add_subdirectory(car_demo/prius_msgs)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- prius_msgs: 1 messages, 0 services
   -- +++ processing catkin package: 'tauristar_msgs'
   -- ==> add_subdirectory(tauristar_msgs)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- tauristar_msgs: 38 messages, 1 services
   -- +++ processing catkin package: 'ugv_course_gazebo'
   -- ==> add_subdirectory(autonomous_park_ros/ugv_course_gazebo)
   -- Extracting file: models/curved_road_100m/meshes/curved_road_100m.dae.tar.gz
   -- Extracting file: models/curved_road_50m/meshes/curved_road_50m.dae.tar.gz
   -- Extracting file: models/intersection/meshes/intersection.dae.tar.gz
   -- Extracting file: models/straight_road_100m/meshes/straight_road_100m.dae.tar.gz
   -- Extracting file: models/straight_road_200m/meshes/straight_road_200m.dae.tar.gz
   -- Extracting file: models/straight_road_50m/meshes/straight_road_50m.dae.tar.gz
   -- +++ processing catkin package: 'ugv_course_launch'
   -- ==> add_subdirectory(autonomous_park_ros/ugv_course_launch)
   -- +++ processing catkin package: 'audibot_control'
   -- ==> add_subdirectory(autonomous_park_ros/audibot_control)
   -- +++ processing catkin package: 'ouster_description'
   -- ==> add_subdirectory(car_demo/ouster_example/ouster_description)
   -- +++ processing catkin package: 'autoware_msgs'
   -- ==> add_subdirectory(autonomous_park_ros/autoware_msgs)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- autoware_msgs: 52 messages, 1 services
   -- +++ processing catkin package: 'calibration_gazebo'
   -- ==> add_subdirectory(car_demo/calibration_gazebo)
   -- +++ processing catkin package: 'lgsvl_msgs'
   -- ==> add_subdirectory(autonomous_park_ros/lgsvl_msgs)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- lgsvl_msgs: 14 messages, 0 services
   -- +++ processing catkin package: 'master_control'
   -- ==> add_subdirectory(autonomous_park_ros/master_control)
   -- +++ processing catkin package: 'lgsvl_simulator_bridge'
   -- ==> add_subdirectory(autonomous_park_ros/lgsvl_simulator_bridge)
   -- +++ processing catkin package: 'audibot_gazebo'
   -- ==> add_subdirectory(autonomous_park_ros/audibot_gazebo)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- +++ processing catkin package: 'img_transform'
   -- ==> add_subdirectory(autonomous_park_ros/img_transform)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- Found OpenCV: /opt/ros/kinetic (found version "3.3.1") 
   -- +++ processing catkin package: 'ouster_gazebo_plugins'
   -- ==> add_subdirectory(car_demo/ouster_example/ouster_gazebo_plugins)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   CMake Warning (dev) at /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindPackageHandleStandardArgs.cmake:272 (message):
     The package name passed to `find_package_handle_standard_args` (PkgConfig)
     does not match the name of the calling package (gazebo).  This can lead to
     problems in calling code that expects `find_package` result variables
     (e.g., `_FOUND`) to follow a certain pattern.
   Call Stack (most recent call first):
     /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindPkgConfig.cmake:45 (find_package_handle_standard_args)
     /usr/lib/x86_64-linux-gnu/cmake/gazebo/gazebo-config.cmake:30 (include)
     car_demo/ouster_example/ouster_gazebo_plugins/CMakeLists.txt:10 (find_package)
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Checking for module 'bullet>=2.82'
   --   Found bullet, version 2.83
   -- Found Simbody: /usr/include/simbody  
   -- Found Boost: /usr/include (found suitable version "1.58.0", minimum required is "1.40.0") found components: thread system filesystem program_options regex iostreams date_time chrono atomic 
   -- Found Protobuf: /usr/lib/x86_64-linux-gnu/libprotobuf.so;-lpthread (found version "2.6.1") 
   -- Found Boost: /usr/include (found version "1.58.0")  
   -- Looking for OGRE...
   -- OGRE_PREFIX_WATCH changed.
   -- Checking for module 'OGRE'
   --   Found OGRE, version 1.9.0
   -- Found Ogre Ghadamon (1.9.0)
   -- Found OGRE: optimized;/usr/lib/x86_64-linux-gnu/libOgreMain.so;debug;/usr/lib/x86_64-linux-gnu/libOgreMain.so
   CMake Warning (dev) at /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindBoost.cmake:1311 (if):
     Policy CMP0054 is not set: Only interpret if() arguments as variables or
     keywords when unquoted.  Run "cmake --help-policy CMP0054" for policy
     details.  Use the cmake_policy command to set the policy and suppress this
     warning.
   
     Quoted variables like "chrono" will no longer be dereferenced when the
     policy is set to NEW.  Since the policy is not set the OLD behavior will be
     used.
   Call Stack (most recent call first):
     /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindBoost.cmake:1908 (_Boost_MISSING_DEPENDENCIES)
     /usr/share/OGRE/cmake/modules/FindOGRE.cmake:318 (find_package)
     /usr/lib/x86_64-linux-gnu/cmake/gazebo/gazebo-config.cmake:175 (find_package)
     car_demo/ouster_example/ouster_gazebo_plugins/CMakeLists.txt:10 (find_package)
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Looking for OGRE_Paging...
   -- Found OGRE_Paging: optimized;/usr/lib/x86_64-linux-gnu/libOgrePaging.so;debug;/usr/lib/x86_64-linux-gnu/libOgrePaging.so
   -- Looking for OGRE_Terrain...
   -- Found OGRE_Terrain: optimized;/usr/lib/x86_64-linux-gnu/libOgreTerrain.so;debug;/usr/lib/x86_64-linux-gnu/libOgreTerrain.so
   -- Looking for OGRE_Property...
   -- Found OGRE_Property: optimized;/usr/lib/x86_64-linux-gnu/libOgreProperty.so;debug;/usr/lib/x86_64-linux-gnu/libOgreProperty.so
   -- Looking for OGRE_RTShaderSystem...
   -- Found OGRE_RTShaderSystem: optimized;/usr/lib/x86_64-linux-gnu/libOgreRTShaderSystem.so;debug;/usr/lib/x86_64-linux-gnu/libOgreRTShaderSystem.so
   -- Looking for OGRE_Volume...
   -- Found OGRE_Volume: optimized;/usr/lib/x86_64-linux-gnu/libOgreVolume.so;debug;/usr/lib/x86_64-linux-gnu/libOgreVolume.so
   -- Looking for OGRE_Overlay...
   -- Found OGRE_Overlay: optimized;/usr/lib/x86_64-linux-gnu/libOgreOverlay.so;debug;/usr/lib/x86_64-linux-gnu/libOgreOverlay.so
   -- Found Protobuf: /usr/lib/x86_64-linux-gnu/libprotobuf.so;-lpthread (found suitable version "2.6.1", minimum required is "2.3.0") 
   -- Config-file not installed for ZeroMQ -- checking for pkg-config
   -- Checking for module 'libzmq >= 4'
   --   Found libzmq , version 4.1.4
   -- Found ZeroMQ: TRUE (Required is at least version "4") 
   -- Checking for module 'uuid'
   --   Found uuid, version 2.27.0
   -- Found UUID: TRUE  
   -- Checking for module 'tinyxml2'
   --   Found tinyxml2, version 2.2.0
   -- Looking for dlfcn.h - found
   -- Looking for libdl - found
   -- Found DL: TRUE  
   -- FreeImage.pc not found, we will search for FreeImage_INCLUDE_DIRS and FreeImage_LIBRARIES
   -- Found UUID: TRUE  
   -- Checking for module 'gts'
   --   Found gts, version 0.7.6
   -- Found GTS: TRUE  
   -- Checking for module 'libswscale'
   --   Found libswscale, version 3.1.101
   -- Found SWSCALE: TRUE  
   -- Checking for module 'libavdevice >= 56.4.100'
   --   Found libavdevice , version 56.4.100
   -- Found AVDEVICE: TRUE (Required is at least version "56.4.100") 
   -- Checking for module 'libavformat'
   --   Found libavformat, version 56.40.101
   -- Found AVFORMAT: TRUE  
   -- Checking for module 'libavcodec'
   --   Found libavcodec, version 56.60.100
   -- Found AVCODEC: TRUE  
   -- Checking for module 'libavutil'
   --   Found libavutil, version 54.31.100
   -- Found AVUTIL: TRUE  
   -- Found CURL: /usr/lib/x86_64-linux-gnu/libcurl.so (found version "7.47.0")  
   -- Checking for module 'jsoncpp'
   --   Found jsoncpp, version 1.7.2
   -- Found JSONCPP: TRUE  
   -- Checking for module 'yaml-0.1'
   --   Found yaml-0.1, version 0.1.6
   -- Found YAML: TRUE  
   -- Checking for module 'libzip'
   --   Found libzip, version 1.0.1
   -- Found ZIP: TRUE  
   -- +++ processing catkin package: 'spot_detection'
   -- ==> add_subdirectory(autonomous_park_ros/spot_detection)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- +++ processing catkin package: 'tauristar_platform'
   -- ==> add_subdirectory(tauristar_platform)
   -- CMAKE_MODULE_PATH: /home/ds16v2/catkin/src/tauristar_platform/cmake
   -- CMAKE_PREFIX_PATH: /home/ds16v2/catkin/src/cmake-build-debug/devel;/home/ds16v2/catkin/devel;/opt/ros/kinetic;/home/ds16v2/catkin/src/tauristar_platform/third_party/g2o_9b41a4e/cmake_modules
   -- TAURISTAR_SRC_DIR: /home/ds16v2/catkin/src/tauristar_platform
   -- CMAKE_BINARY_DIR:  /home/ds16v2/catkin/src/cmake-build-debug
   -- TAURISTAR_THIRD_PARTY: /home/ds16v2/catkin/src/tauristar_platform/third_party
   -- TAURISTAR_CROSS_COMPONENTS_DIR: /home/ds16v2/catkin/src/tauristar_platform/cross_components
   -- Found OpenSSL: /usr/lib/x86_64-linux-gnu/libcrypto.so (found version "1.0.2g")  
   -- Found OpenCV: /usr/local/opencv/opencv-3.4.5 (found version "3.4.5") 
   -- OpenCV_DIR = /usr/local/opencv/opencv-3.4.5/share/OpenCV
   -- OpenCV_INCLUDE_DIRS = /usr/local/opencv/opencv-3.4.5/include;/usr/local/opencv/opencv-3.4.5/include/opencv
   -- OpenCV_LIBS_PATH = 
   -- OpenCV_LIBS = opencv_videoio;opencv_highgui;opencv_photo;opencv_imgcodecs;opencv_calib3d;opencv_shape;opencv_ml;opencv_imgproc;opencv_objdetect;opencv_video;opencv_videostab;opencv_stitching;opencv_flann;opencv_superres;opencv_features2d;opencv_dnn;opencv_core
   -- Found Boost: /usr/include (found version "1.58.0") found components: serialization system program_options thread filesystem regex chrono date_time atomic 
   -- option: BUILD_WITH_ROS=OFF
   -- option: BUILD_WITH_OPENCV=ON
   -- option: BUILD_WITH_HDMAP=ON
   -- option: BUILD_WITH_DATABASE=OFF
   -- option: BUILD_WITH_LIBTORCH=OFF
   -- option: BUILD_WITH_SOFARPC=OFF
   -- option: BUILD_WITH_ROSBRIDGECPP=ON
   -- option: BUILD_WITH_CAFFE=OFF
   -- option: BUILD_WITH_BSD=OFF
   -- option: BUILD_WITH_RINGBUFFER=OFF
   -- option: BUILD_WITH_NEW_UI=OFF
   -- option: BUILD_CROSS_MAKE=OFF
   -- option: BUILD_WITH_MCU=OFF
   -- option: BUILD_WITH_VSLAM=OFF
   -- option: USE_PANGOLIN_VIEWER=OFF
   -- option: BUILD_WITH_ZEROMQ=OFF
   -- EIGEN3_INCLUDE_DIRS = /home/ds16v2/catkin/src/tauristar_platform/third_party/eigen3
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/third_party
   CMake Deprecation Warning at tauristar_platform/third_party/yaml-cpp/CMakeLists.txt:10 (cmake_policy):
     The OLD behavior for policy CMP0012 will be removed from a future version
     of CMake.
   
     The cmake-policies(7) manual explains that the OLD behaviors of all
     policies are deprecated and that a policy should be set to OLD only under
     specific short-term circumstances.  Projects should be ported to the NEW
     behavior and not rely on setting a policy to OLD.
   
   
   CMake Deprecation Warning at tauristar_platform/third_party/yaml-cpp/CMakeLists.txt:14 (cmake_policy):
     The OLD behavior for policy CMP0015 will be removed from a future version
     of CMake.
   
     The cmake-policies(7) manual explains that the OLD behaviors of all
     policies are deprecated and that a policy should be set to OLD only under
     specific short-term circumstances.  Projects should be ported to the NEW
     behavior and not rely on setting a policy to OLD.
   
   
   -- Performing Test FLAG_WEXTRA
   -- Performing Test FLAG_WEXTRA - Success
   -- a--- Build type: Debug
   -- Build spdlog: 1.4.2
   -- Build type: Debug
   -- rosbridgecpp PRINT_DEBUG: ON
   -- rosbridgecpp add_subdirectory: Simple-WebSocket-Server
   boost: /usr/lib/x86_64-linux-gnu/libboost_serialization.so;/usr/lib/x86_64-linux-gnu/libboost_system.so;/usr/lib/x86_64-linux-gnu/libboost_program_options.so;/usr/lib/x86_64-linux-gnu/libboost_thread.so;-lpthread;/usr/lib/x86_64-linux-gnu/libboost_filesystem.so;/usr/lib/x86_64-linux-gnu/libboost_regex.so;/usr/lib/x86_64-linux-gnu/libboost_chrono.so;/usr/lib/x86_64-linux-gnu/libboost_date_time.so;/usr/lib/x86_64-linux-gnu/libboost_atomic.so
   -- Found Boost: /usr/include (found suitable version "1.58.0", minimum required is "1.54.0") found components: system thread coroutine context chrono date_time atomic 
   -- rosbridgecpp add_executable rosbridge_ws_client
   -- Check size of long double
   -- Check size of long double - done
   -- Check size of double
   -- Check size of double - done
   -- Check if the system is big endian
   -- Searching 16 bit integer
   -- Looking for sys/types.h
   -- Looking for sys/types.h - found
   -- Looking for stdint.h
   -- Looking for stdint.h - found
   -- Looking for stddef.h
   -- Looking for stddef.h - found
   -- Check size of unsigned short
   -- Check size of unsigned short - done
   -- Searching 16 bit integer - Using unsigned short
   -- Check if the system is big endian - little endian
   -- Performing Test FLOAT_CONVERSION
   -- Performing Test FLOAT_CONVERSION - Success
   -- Performing Test CXX11TEST14
   -- Performing Test CXX11TEST14 - Success
   -- Performing Test CXX11_MATH
   -- Performing Test CXX11_MATH - Success
   -- Performing Test CXX11_STATIC_ASSERT
   -- Performing Test CXX11_STATIC_ASSERT - Success
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/tauristar_localization
   -- Project binary dir: /home/ds16v2/catkin/src/cmake-build-debug/tauristar_platform/tauristar_localization
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/tauristar_mpc_solver
   -- Found Boost: /usr/include (found version "1.58.0") found components: system 
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/tauristar_geometry
   -- Found Boost: /usr/include (found version "1.58.0") found components: serialization 
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/tauristar_motion_planner
   -- Found Boost: /usr/include (found version "1.58.0") found components: system program_options thread chrono date_time atomic 
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/simple_path_planner
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/tauristar_perception
   -- CMAKE_BINARY_DIR:  /home/ds16v2/catkin/src/cmake-build-debug
   PROJECT_SOURCE_DIR /home/ds16v2/catkin/src/tauristar_platform/tauristar_perception/tauristar_avm
   -- CMAKE_BINARY_DIR:  /home/ds16v2/catkin/src/cmake-build-debug
   PROJECT_SOURCE_DIR /home/ds16v2/catkin/src/tauristar_platform/tauristar_perception/tauristar_image_pipeline/ts_vision_opencv/chess_detector
   -- CMAKE_BINARY_DIR:  /home/ds16v2/catkin/src/cmake-build-debug
   PROJECT_SOURCE_DIR /home/ds16v2/catkin/src/tauristar_platform/tauristar_perception/tauristar_image_pipeline/ts_vision_opencv/image_geometry
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/apa_full_stack
   opencv: opencv_videoio;opencv_highgui;opencv_photo;opencv_imgcodecs;opencv_calib3d;opencv_shape;opencv_ml;opencv_imgproc;opencv_objdetect;opencv_video;opencv_videostab;opencv_stitching;opencv_flann;opencv_superres;opencv_features2d;opencv_dnn;opencv_core
   boost: /usr/lib/x86_64-linux-gnu/libboost_serialization.so;/usr/lib/x86_64-linux-gnu/libboost_system.so;/usr/lib/x86_64-linux-gnu/libboost_program_options.so;/usr/lib/x86_64-linux-gnu/libboost_thread.so;-lpthread;/usr/lib/x86_64-linux-gnu/libboost_filesystem.so;/usr/lib/x86_64-linux-gnu/libboost_regex.so;/usr/lib/x86_64-linux-gnu/libboost_chrono.so;/usr/lib/x86_64-linux-gnu/libboost_date_time.so;/usr/lib/x86_64-linux-gnu/libboost_atomic.so
   -- add_subdirectory: /home/ds16v2/catkin/src/tauristar_platform/tauristar_operations
   TAURISTAR_BASE_LIBS:  tauristar_flowprocessor;-Wl,--whole-archive;tauristar_operations;-Wl,--no-whole-archive;tauristar_hd_gridmap;grid_map_core;tauristar_gridmap;apa_info_process;fcw;lib_avm_dcal;lib_avm;lib_avm_bvc;lib_avm_calib;lib_avm_algobase;lib_avm_util;motion_planner;tauristar_geometry;mpc_controller_lib;tauristar_procedure;drlib;basic_util;state_machine_lib;simple_path_planner;lanelet2_extension_lib;lanelet2_core;lanelet2_io;lanelet2_projection;lanelet2_routing;lanelet2_traffic_rules;GeographicLib_SHARED;pugixml
   Boost libraries:  /usr/lib/x86_64-linux-gnu/libboost_serialization.so;/usr/lib/x86_64-linux-gnu/libboost_system.so;/usr/lib/x86_64-linux-gnu/libboost_program_options.so;/usr/lib/x86_64-linux-gnu/libboost_thread.so;-lpthread;/usr/lib/x86_64-linux-gnu/libboost_filesystem.so;/usr/lib/x86_64-linux-gnu/libboost_regex.so;/usr/lib/x86_64-linux-gnu/libboost_chrono.so;/usr/lib/x86_64-linux-gnu/libboost_date_time.so;/usr/lib/x86_64-linux-gnu/libboost_atomic.so
   -- link binary libraries
   -- +++ processing catkin package: 'ouster_ros'
   -- ==> add_subdirectory(car_demo/ouster_example/ouster_ros)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- ouster_ros: 1 messages, 1 services
   -- +++ processing catkin package: 'ugv_course_libs'
   -- ==> add_subdirectory(autonomous_park_ros/ugv_course_libs)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- +++ processing catkin package: 'bicycle_state_space'
   -- ==> add_subdirectory(autonomous_park_ros/bicycle_state_space)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- +++ processing catkin package: 'set_nav_point'
   -- ==> add_subdirectory(autonomous_park_ros/set_nav_point)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- +++ processing catkin package: 'ugv_course_gazebo_plugins'
   -- ==> add_subdirectory(autonomous_park_ros/ugv_course_gazebo_plugins)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   CMake Warning (dev) at /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindPackageHandleStandardArgs.cmake:272 (message):
     The package name passed to `find_package_handle_standard_args` (PkgConfig)
     does not match the name of the calling package (gazebo).  This can lead to
     problems in calling code that expects `find_package` result variables
     (e.g., `_FOUND`) to follow a certain pattern.
   Call Stack (most recent call first):
     /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindPkgConfig.cmake:45 (find_package_handle_standard_args)
     /usr/lib/x86_64-linux-gnu/cmake/gazebo/gazebo-config.cmake:30 (include)
     autonomous_park_ros/ugv_course_gazebo_plugins/CMakeLists.txt:12 (find_package)
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Found Boost: /usr/include (found suitable version "1.58.0", minimum required is "1.40.0") found components: thread system filesystem program_options regex iostreams date_time chrono atomic 
   -- Found Protobuf: /usr/lib/x86_64-linux-gnu/libprotobuf.so;-lpthread (found version "2.6.1") 
   -- Found Boost: /usr/include (found version "1.58.0")  
   -- Looking for OGRE...
   -- Found Ogre Ghadamon (1.9.0)
   -- Found OGRE: optimized;/usr/lib/x86_64-linux-gnu/libOgreMain.so;debug;/usr/lib/x86_64-linux-gnu/libOgreMain.so
   CMake Warning (dev) at /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindBoost.cmake:1311 (if):
     Policy CMP0054 is not set: Only interpret if() arguments as variables or
     keywords when unquoted.  Run "cmake --help-policy CMP0054" for policy
     details.  Use the cmake_policy command to set the policy and suppress this
     warning.
   
     Quoted variables like "chrono" will no longer be dereferenced when the
     policy is set to NEW.  Since the policy is not set the OLD behavior will be
     used.
   Call Stack (most recent call first):
     /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindBoost.cmake:1908 (_Boost_MISSING_DEPENDENCIES)
     /usr/share/OGRE/cmake/modules/FindOGRE.cmake:318 (find_package)
     /usr/lib/x86_64-linux-gnu/cmake/gazebo/gazebo-config.cmake:175 (find_package)
     autonomous_park_ros/ugv_course_gazebo_plugins/CMakeLists.txt:12 (find_package)
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Looking for OGRE_Paging...
   -- Found OGRE_Paging: optimized;/usr/lib/x86_64-linux-gnu/libOgrePaging.so;debug;/usr/lib/x86_64-linux-gnu/libOgrePaging.so
   -- Looking for OGRE_Terrain...
   -- Found OGRE_Terrain: optimized;/usr/lib/x86_64-linux-gnu/libOgreTerrain.so;debug;/usr/lib/x86_64-linux-gnu/libOgreTerrain.so
   -- Looking for OGRE_Property...
   -- Found OGRE_Property: optimized;/usr/lib/x86_64-linux-gnu/libOgreProperty.so;debug;/usr/lib/x86_64-linux-gnu/libOgreProperty.so
   -- Looking for OGRE_RTShaderSystem...
   -- Found OGRE_RTShaderSystem: optimized;/usr/lib/x86_64-linux-gnu/libOgreRTShaderSystem.so;debug;/usr/lib/x86_64-linux-gnu/libOgreRTShaderSystem.so
   -- Looking for OGRE_Volume...
   -- Found OGRE_Volume: optimized;/usr/lib/x86_64-linux-gnu/libOgreVolume.so;debug;/usr/lib/x86_64-linux-gnu/libOgreVolume.so
   -- Looking for OGRE_Overlay...
   -- Found OGRE_Overlay: optimized;/usr/lib/x86_64-linux-gnu/libOgreOverlay.so;debug;/usr/lib/x86_64-linux-gnu/libOgreOverlay.so
   -- Found Protobuf: /usr/lib/x86_64-linux-gnu/libprotobuf.so;-lpthread (found suitable version "2.6.1", minimum required is "2.3.0") 
   -- Config-file not installed for ZeroMQ -- checking for pkg-config
   -- Checking for module 'libzmq >= 4'
   --   Found libzmq , version 4.1.4
   -- Checking for module 'uuid'
   --   Found uuid, version 2.27.0
   -- Found UUID: TRUE  
   -- Checking for module 'tinyxml2'
   --   Found tinyxml2, version 2.2.0
   -- Looking for dlfcn.h - found
   -- Looking for libdl - found
   -- FreeImage.pc not found, we will search for FreeImage_INCLUDE_DIRS and FreeImage_LIBRARIES
   -- Found UUID: TRUE  
   -- Checking for module 'gts'
   --   Found gts, version 0.7.6
   -- Checking for module 'libswscale'
   --   Found libswscale, version 3.1.101
   -- Checking for module 'libavdevice >= 56.4.100'
   --   Found libavdevice , version 56.4.100
   -- Checking for module 'libavformat'
   --   Found libavformat, version 56.40.101
   -- Checking for module 'libavcodec'
   --   Found libavcodec, version 56.60.100
   -- Checking for module 'libavutil'
   --   Found libavutil, version 54.31.100
   -- Checking for module 'jsoncpp'
   --   Found jsoncpp, version 1.7.2
   -- Checking for module 'yaml-0.1'
   --   Found yaml-0.1, version 0.1.6
   -- Checking for module 'libzip'
   --   Found libzip, version 1.0.1
   CMake Warning at /opt/ros/kinetic/share/catkin/cmake/catkin_package.cmake:166 (message):
     catkin_package() DEPENDS on 'gazebo' but neither 'gazebo_INCLUDE_DIRS' nor
     'gazebo_LIBRARIES' is defined.
   Call Stack (most recent call first):
     /opt/ros/kinetic/share/catkin/cmake/catkin_package.cmake:102 (_catkin_package)
     autonomous_park_ros/ugv_course_gazebo_plugins/CMakeLists.txt:21 (catkin_package)
   
   
   -- +++ processing catkin package: 'prius_description'
   -- ==> add_subdirectory(car_demo/prius_description)
   -- +++ processing catkin package: 'car_nav'
   -- ==> add_subdirectory(autonomous_park_ros/car_nav)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- Performing Test COMPILER_SUPPORTS_CXX11
   -- Performing Test COMPILER_SUPPORTS_CXX11 - Success
   -- Performing Test COMPILER_SUPPORTS_CXX0X
   -- Performing Test COMPILER_SUPPORTS_CXX0X - Success
   -- +++ processing catkin package: 'ros_teb_test'
   -- ==> add_subdirectory(ros_teb_test)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   -- +++ processing catkin package: 'audibot_description'
   -- ==> add_subdirectory(autonomous_park_ros/audibot_description)
   -- +++ processing catkin package: 'car_demo'
   -- ==> add_subdirectory(car_demo/car_demo)
   -- Using these message generators: gencpp;geneus;genlisp;gennodejs;genpy
   CMake Warning (dev) at /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindPackageHandleStandardArgs.cmake:272 (message):
     The package name passed to `find_package_handle_standard_args` (PkgConfig)
     does not match the name of the calling package (gazebo).  This can lead to
     problems in calling code that expects `find_package` result variables
     (e.g., `_FOUND`) to follow a certain pattern.
   Call Stack (most recent call first):
     /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindPkgConfig.cmake:45 (find_package_handle_standard_args)
     /usr/lib/x86_64-linux-gnu/cmake/gazebo/gazebo-config.cmake:30 (include)
     car_demo/car_demo/CMakeLists.txt:16 (find_package)
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Found Boost: /usr/include (found suitable version "1.58.0", minimum required is "1.40.0") found components: thread system filesystem program_options regex iostreams date_time chrono atomic 
   -- Found Protobuf: /usr/lib/x86_64-linux-gnu/libprotobuf.so;-lpthread (found version "2.6.1") 
   -- Found Boost: /usr/include (found version "1.58.0")  
   -- Looking for OGRE...
   -- Found Ogre Ghadamon (1.9.0)
   -- Found OGRE: optimized;/usr/lib/x86_64-linux-gnu/libOgreMain.so;debug;/usr/lib/x86_64-linux-gnu/libOgreMain.so
   CMake Warning (dev) at /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindBoost.cmake:1311 (if):
     Policy CMP0054 is not set: Only interpret if() arguments as variables or
     keywords when unquoted.  Run "cmake --help-policy CMP0054" for policy
     details.  Use the cmake_policy command to set the policy and suppress this
     warning.
   
     Quoted variables like "chrono" will no longer be dereferenced when the
     policy is set to NEW.  Since the policy is not set the OLD behavior will be
     used.
   Call Stack (most recent call first):
     /home/ds16v2/Software/clion-2020.3.2/bin/cmake/linux/share/cmake-3.17/Modules/FindBoost.cmake:1908 (_Boost_MISSING_DEPENDENCIES)
     /usr/share/OGRE/cmake/modules/FindOGRE.cmake:318 (find_package)
     /usr/lib/x86_64-linux-gnu/cmake/gazebo/gazebo-config.cmake:175 (find_package)
     car_demo/car_demo/CMakeLists.txt:16 (find_package)
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Looking for OGRE_Paging...
   -- Found OGRE_Paging: optimized;/usr/lib/x86_64-linux-gnu/libOgrePaging.so;debug;/usr/lib/x86_64-linux-gnu/libOgrePaging.so
   -- Looking for OGRE_Terrain...
   -- Found OGRE_Terrain: optimized;/usr/lib/x86_64-linux-gnu/libOgreTerrain.so;debug;/usr/lib/x86_64-linux-gnu/libOgreTerrain.so
   -- Looking for OGRE_Property...
   -- Found OGRE_Property: optimized;/usr/lib/x86_64-linux-gnu/libOgreProperty.so;debug;/usr/lib/x86_64-linux-gnu/libOgreProperty.so
   -- Looking for OGRE_RTShaderSystem...
   -- Found OGRE_RTShaderSystem: optimized;/usr/lib/x86_64-linux-gnu/libOgreRTShaderSystem.so;debug;/usr/lib/x86_64-linux-gnu/libOgreRTShaderSystem.so
   -- Looking for OGRE_Volume...
   -- Found OGRE_Volume: optimized;/usr/lib/x86_64-linux-gnu/libOgreVolume.so;debug;/usr/lib/x86_64-linux-gnu/libOgreVolume.so
   -- Looking for OGRE_Overlay...
   -- Found OGRE_Overlay: optimized;/usr/lib/x86_64-linux-gnu/libOgreOverlay.so;debug;/usr/lib/x86_64-linux-gnu/libOgreOverlay.so
   -- Found Protobuf: /usr/lib/x86_64-linux-gnu/libprotobuf.so;-lpthread (found suitable version "2.6.1", minimum required is "2.3.0") 
   -- Config-file not installed for ZeroMQ -- checking for pkg-config
   -- Checking for module 'libzmq >= 4'
   --   Found libzmq , version 4.1.4
   -- Checking for module 'uuid'
   --   Found uuid, version 2.27.0
   -- Found UUID: TRUE  
   -- Checking for module 'tinyxml2'
   --   Found tinyxml2, version 2.2.0
   -- Looking for dlfcn.h - found
   -- Looking for libdl - found
   -- FreeImage.pc not found, we will search for FreeImage_INCLUDE_DIRS and FreeImage_LIBRARIES
   -- Found UUID: TRUE  
   -- Checking for module 'gts'
   --   Found gts, version 0.7.6
   -- Checking for module 'libswscale'
   --   Found libswscale, version 3.1.101
   -- Checking for module 'libavdevice >= 56.4.100'
   --   Found libavdevice , version 56.4.100
   -- Checking for module 'libavformat'
   --   Found libavformat, version 56.40.101
   -- Checking for module 'libavcodec'
   --   Found libavcodec, version 56.60.100
   -- Checking for module 'libavutil'
   --   Found libavutil, version 54.31.100
   -- Checking for module 'jsoncpp'
   --   Found jsoncpp, version 1.7.2
   -- Checking for module 'yaml-0.1'
   --   Found yaml-0.1, version 0.1.6
   -- Checking for module 'libzip'
   --   Found libzip, version 1.0.1
   -- Configuring done
   CMake Warning (dev) at autonomous_park_ros/spot_detection/CMakeLists.txt:45 (add_dependencies):
     Policy CMP0046 is not set: Error on non-existent dependency in
     add_dependencies.  Run "cmake --help-policy CMP0046" for policy details.
     Use the cmake_policy command to set the policy and suppress this warning.
   
     The dependency target "vehicle_control_gencfg" of target "vehicle_control"
     does not exist.
   This warning is for project developers.  Use -Wno-dev to suppress it.
   
   -- Generating done
   -- Build files have been written to: /home/ds16v2/catkin/src/cmake-build-debug
   
   ```

   ros 项目载入完成。







---



## 参考:

链接: [Ubuntu install of ROS Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu)

链接: [Linux学习和ROS安装(1)](https://www.cnblogs.com/yhlx125/p/5765298.html)

链接: [[ROS 不能再详细的安装教程](https://www.cnblogs.com/liu-fa/p/5779206.html)](https://www.cnblogs.com/liu-fa/p/5779206.html#undefined)

链接: [加载cmakelist的一个问题：find_package(catkin) failed.](https://blog.csdn.net/Alex_wise/article/details/105201687)

链接: [setting up ros package in Clion](https://stackoverflow.com/questions/33172132/setting-up-ros-package-in-clion)

