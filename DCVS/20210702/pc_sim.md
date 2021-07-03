# PC_SIM how

[toc]



---

## 1. using mobaxterm

编译`processor_node_xxx`（算法）

```
git stash
git pull
git stash pop

git checkout -b dev

# TauristarPlatform/src/tauristar_platform/apa_full_stack/CMakeLists.txt
# option (show_image "show_image" ON) --> #4264

mkdir build_pc
cd build_pc
cmake .. -DCMAKE_BUILD_TYPE=Debug
make -j8
```



PC_SIM 文件路径：`/home/marvin/work/ts/PC_SIM`

```
.
├── dvr_0630_ts_ver_l1								# dvr 文件夹
│   ├── canbus.txt
│   ├── car_param.yaml
│   ├── computed_path2.txt
│   ├── computed_path.txt
│   ├── front.h264
│   ├── front.hdr
│   ├── left.h264
│   ├── left.hdr
│   ├── naive_trajectory_fixed_dt_fwd_simXY.txt
│   ├── naive_trajectory_fixed_dt.txt
│   ├── naive_trajectory_fixed_dtXY.txt
│   ├── naive_trajectory.txt
│   ├── naive_trajectoryXY.txt
│   ├── plots
│   │   ├── plot1.png
│   │   ├── plot2&3.png
│   │   ├── plot4.png
│   │   └── plot.log
│   ├── processor_node_show_debug					# processor_node 二进制文件
│   ├── rear.h264
│   ├── rear.hdr
│   ├── right.h264
│   ├── right.hdr
│   ├── session.log
│   ├── sin_lut_8.bin
│   ├── test0f_pc.yaml
│   └── test0f.yaml
├── dvr_template									# dvr 文件夹
│   ├── canbus.txt
|   |	.......
│   └── test0f.yaml
└── IC421											# IC421 的配置文件，如有更新，请替换如下文件
    ├── apa_front_cam.dat							# 环影镜头参数文件
    ├── apa_left_cam.dat							# 环影镜头参数文件
    ├── apa_rear_cam.dat							# 环影镜头参数文件
    ├── apa_right_cam.dat							# 环影镜头参数文件
    └── BV3D.xml									# 标定参数文件
```



`processor_node_xxx` 二进制文件需要先编译，然后再将其从

```
/home/marvin/work/ts/TauristarPlatform/src/tauristar_platform/build_xxx/bin/  
```

中拷贝到 `PC_SIM/dvr_xxx` 中。



运行 `processor_node_xxx`:

```
./processor_node_xxx -o conf=test0f_pc.yaml
```



关于 `test0f_pc.yaml`文件的修改，参考 `#5390`



运行效果图，通过 mobaxterm。

![image-20210702195434934](pc_sim.assets/image-20210702195434934.png)

![image-20210702195555290](pc_sim.assets/image-20210702195555290.png)

缺点：带宽不够，视频输出延时。

![image-20210702195701467](pc_sim.assets/image-20210702195701467.png)



---

## 2. using vmware

```
export LD_LIBRARY_PATH=/usr/lib:/home/ds16v2/Work/TS_PC/PC_TEST_2/PC_SIM/lib:/usr/local/opencv/opencv-3.4.5/lib~

# 
# /usr/lib
# /home/ds16v2/Work/TS_PC/PC_TEST_2/PC_SIM/lib
# /usr/local/opencv/opencv-3.4.5/lib~
```

