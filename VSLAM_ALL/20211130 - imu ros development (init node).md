# 20211130 - imu ros development (init node)

[toc]




---

**LOG:**

2021年11月30日：解决imu 串口问题。

2021年12月01日：imu 数据解析。

---

convention: maximum logs recorded 3.

---



## 2021年12月01日

### 记录：

#### Task 00：ROS IMU TOOLS（todo）

ros imu_tools  订阅的topic为`imu/data_raw`，因此如果需要使用这个工具，自己编写的 imu 驱动就需要发布 `imu/data_raw`的 topic。使用 imu_tools 的前提，有 imu 驱动发布 `imu/data_raw`。（todo）



#### Task 01：根据 ROS OFFICIAL 编写 ROS 驱动

参考 link01，link02。link14 是一个标准的 ros driver，在完成简单的 ros driver 之后，可以参考其进行封装。



### 参考：

link00: [使用imu_tools对IMU进行滤波并可视化](https://blog.csdn.net/learning_tortosie/article/details/103189118)

link01: [ROS从入门到精通系列（前言）-- ROS官方教程内容概要](https://haowang.blog.csdn.net/article/details/90745221)

link02: [ROS从入门到精通系列博客](https://blog.csdn.net/hhaowang/category_11131544.html)

link03: [ROS自定义msg类型及使用 -- 【可以参考一下ros自建msg的流程】](https://blog.csdn.net/u013453604/article/details/72903398)

link04: [ROS串口通信（2）以十六进制指令读取IMU数据 -- 【继续昨天的ROS代码进行优化】](https://blog.csdn.net/fb_941219/article/details/84486603)

link05: [在ROS中与其他器件使用十六进制串口通信 -- 【可以看一下】](https://blog.csdn.net/chduan_10/article/details/76619179)

link06: [ROS从入门到精通系列（十六）-- IMU in ROS -- 【最早看的文章之一，继续】](https://blog.csdn.net/hhaowang/article/details/104961725)

link07: [ROS从入门到精通系列（二）ROS官方基础教程与实例笔记整理 -- 【link01同作者，一起看】](https://haowang.blog.csdn.net/article/details/102637625)

link08: [ROS下IMU串口通讯接口（通用版）-- 【很简单的代码，可以尝试一下】](https://blog.csdn.net/zhuoyueljl/article/details/75453808?utm_medium=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-5.control404&depth_1-utm_source=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-5.control40)

link09: [ROS学习——读取IMU数据 --【很标准的 ros imu 开发流程，稍微复杂，link08之后可以尝试一下】](https://blog.csdn.net/just_do_it567/article/details/107223025?utm_medium=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-4.control404&depth_1-utm_source=distribute.pc_feed_404.none-task-blog-2~default~BlogCommendFromBaidu~default-4.control40)

link10: [ros接入IMU数据，打包发布topic -- 【很标准的 ros imu 开发流程】](https://blog.csdn.net/xinmei4275/article/details/85040164?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

link11: [ROS传感器之IMU实践 -- 【讲概念文章，实用】](https://zhuanlan.zhihu.com/p/268565374)

link12: [从零搭建ROS机器人平台 -- 【小白机器人的文章，yyds 之一】](https://blog.csdn.net/zhao_ke_xue/category_10285721.html?spm=1001.2014.3001.5482)

link13: [ROS——下位机驱动节点：读取并发布单片机IMU、编码器速度数据（python）-- 【很标准的 ros imu 开发流程】](https://blog.csdn.net/qq_45779334/article/details/113563618)

link14: [iblus](https://github.com/iblus)/**[imu4ros -- 【git 代码，需要补充很多知识后再去使用】](https://github.com/iblus/imu4ros)**

link15: [3.使用串口读取IMU数据并通过话题发布 --【最最开始的 imu 资料】](https://www.corvin.cn/2274.html)



---

## 2021年11月30日

### 记录：

#### Task 00：解决 编译 bug：undefind reference to XXX

```
test_imu_serial.cpp:(.text+0x56b): undefined reference to `serial::Serial::open()'
test_imu_serial.cpp:(.text+0x577): undefined reference to `serial::Serial::isOpen() const'
test_imu_serial.cpp:(.text+0x737): undefined reference to `serial::Serial::available()'
test_imu_serial.cpp:(.text+0x8c8): undefined reference to `serial::Serial::available()'
test_imu_serial.cpp:(.text+0x8e1): undefined reference to make[1]: *** [imu_1129/CMakeFiles/test_imu_serial.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[100%] Linking CXX executable /home/ds18/catkin_x/IMU_ws/devel/lib/serial_msgs/serial_example_node1
[100%] Built target listen1
[100%] Built target serial_example_node1
Makefile:140: recipe for target 'all' failed
make: *** [all] Error 2
Invoking "make -j8 -l8" failed
```

**解决：**

在 CMakeLists.txt --》find_package 中添加：serial

```cmake
find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        sensor_msgs
        std_msgs
        serial  # rospack find serial: /opt/ros/melodic/share/serial
        )
```

对 rospack package 做详细的理解：
```
# ros package 的 .cmake 文件查找路径：

ds18@ubuntu:~/catkin_x/IMU_ws$ rospack find serial
/opt/ros/melodic/share/serial

ds18@ubuntu:~/catkin_x/IMU_ws$ cd /opt/ros/melodic/share/serial/

ds18@ubuntu:/opt/ros/melodic/share/serial$ ls
cmake  package.xml

ds18@ubuntu:/opt/ros/melodic/share/serial$ ls cmake/
serialConfig.cmake  serialConfig-version.cmake

# ros package 的 头文件 (.h) 查找路径：

ds18@ubuntu:/opt/ros/melodic/include/serial$ pwd
/opt/ros/melodic/include/serial

ds18@ubuntu:/opt/ros/melodic/include/serial$ ls
serial.h  v8stdint.h

```

编辑结果：

```
[ 95%] Linking CXX executable /home/ds18/catkin_x/IMU_ws/devel/lib/serial_msgs/listen1
[ 95%] Built target test_imu_serial
[100%] Linking CXX executable /home/ds18/catkin_x/IMU_ws/devel/lib/serial_msgs/talker
[100%] Built target serial_example_node1
[100%] Built target listen1
[100%] Built target talker
```

![image-20211130102750174](20211130 - imu ros development (init node).assets/image-20211130102750174.png)

#### Task 01：run `test_imu_serial.cpp`：虚拟机无法打开串口

```
/home/ds18/catkin_x/IMU_ws/src/cmake-build-debug/devel/lib/imu_1129/test_imu_serial
[ERROR] [1638236870.112381249]: Unable to open port 

Process finished with exit code 255
```

**解决1：可能需要在 ubuntu 虚拟机中使用*串口工具*读取数据。**

linux 串口权限问题，解决（参考 link04）：

> 1. 查看 linux 下的 usb 设备：
>
>    ```
>    lsusb				# 列出所有 usb 设备
>    ls -l /dev/tty*     # 列出 /dev 下 所有 tty*
>    ls -l /dev/ttyUSB*  # 列出 /dev 下 所有 ttyUSB*
>    ```
>
>    ```
>    ds18@ubuntu:~$ lsusb
>    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
>    Bus 002 Device 010: ID 1a86:7523 QinHeng Electronics HL-340 USB-Serial adapter
>    Bus 002 Device 005: ID 0e0f:0008 VMware, Inc. 
>    Bus 002 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
>    Bus 002 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
>    Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
>    ```
>
> 2. 查看串口是否在使用：
>
>    **在使用 .cpp，minicom，cutecom 读取串口数据时，如果出现`input/output error`，可以不用频繁拔插 IMU 串口，这可能是 os 释放串口延时导致的。解决办法是，等待一段时间，再次打开设备。在三种情况下验证可行。**
>
>    ```
>    ds18@ubuntu:~$ ls -l /dev/ttyUSB0
>    crwxrwxrwx 1 root dialout 188, 0 Nov 29 22:04 /dev/ttyUSB0
>    
>    ds18@ubuntu:~$ ls -l /dev/ttyUSB1
>    ls: cannot access '/dev/ttyUSB1': No such file or directory
>    ```
>
> 3. **查看串口设备**：(这个有点问题，为什么是 failed to read modem？链接中就没有这个问题)
>
>    Daiwei 发的关于 ch341 的驱动代码：
>
>     <img src="20211130 - imu ros development (init node).assets/image-20211130153343732.png" alt="image-20211130153343732" style="zoom:50%;" />
>
>    ```
>    ds18@ubuntu:~$ dmesg | grep ttyUSB0
>    [19421.411925] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [20205.222915] ch341-uart ttyUSB0: failed to read modem status: -110
>    [20253.835193] ch341-uart ttyUSB0: failed to read modem status: -110
>    [21427.697504] ch341-uart ttyUSB0: failed to read modem status: -110
>    [21474.753316] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27381.306641] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27436.364640] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27446.369464] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27526.409866] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27535.441496] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27551.450303] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27594.472664] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27609.481462] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27796.586239] ch341-uart ttyUSB0: failed to read modem status: -110
>    [27804.453665] ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
>    [27815.069697] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [28055.839581] ch341-uart ttyUSB0: failed to read modem status: -110
>    [28065.871445] ch341-uart ttyUSB0: failed to read modem status: -110
>    [28142.336373] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29448.633392] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29593.486393] ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
>    [29599.910891] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [29816.973696] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29855.022383] ch341-uart ttyUSB0: failed to read modem status: -110
>    [29927.812395] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30036.908861] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30117.479713] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30304.616999] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30311.653111] ch341-uart ttyUSB0: ch341-uart converter now disconnected from ttyUSB0
>    [30317.861884] usb 2-2.2: ch341-uart converter now attached to ttyUSB0
>    [30534.016853] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30551.054556] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30645.141575] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30654.173933] ch341-uart ttyUSB0: failed to read modem status: -110
>    [30888.338850] ch341-uart ttyUSB0: failed to read modem status: -110
>    [31124.980341] ch341-uart ttyUSB0: failed to read modem status: -110
>    [31565.256791] ch341-uart ttyUSB0: failed to read modem status: -110
>    [31737.109282] ch341-uart ttyUSB0: failed to read modem status: -110
>    [32312.387312] ch341-uart ttyUSB0: failed to read modem status: -110
>    [32400.464474] ch341-uart ttyUSB0: failed to read modem status: -110
>    ```
>
> 4. 串口权限问题：
>
>    每次开机后对串口读写都要附加权限（串口默认普通用户没有读写权限）。
>
>    **临时权限修改：**
>
>    ```bash
>    sudo chmod 666 /dev/ttyUSB0
>       
>    ds18@ubuntu:~$ ll /dev/ttyUSB0
>    crwxrwxrwx 1 root dialout 188, 0 Nov 30 00:08 /dev/ttyUSB0
>    ```
>
>    **永久权限修改：**
>
>    创建 70-ttyusb-serial.rules 文件。这个文件本来是不存在的，只不过编辑器打开不存在的文件会自动创建。70-usb-serial.rules 文件名可以自定义，但必须以.rules结尾。
>
>    ttyUSB*表示所有这一格式的串口名，如果你的是ttyS*或其它，按需改。
>    0666表示加权模式，和chmod后面的参数一致，写成666也可以。
>    vibot_base是我自定义的串口名，就是为ttyUSB*创建一个超链接 ，如下图。**如果你不需要，可以去掉最后一项。**
>
>    ```
>    sudo gedit /etc/udev/rules.d/70-ttyusb.rules
>       
>    KERNEL=="ttyUSB*", MODE="0666", SYMLINK+="vibot_base"
>    ```



**尝试了两种串口工具**

1. minicom
2. cutecom

个人认为 cutecom 更好用，因为它有图形界面。

* minicom 使用总结：

  常用命令：`minicom -c on -H` ascii 颜色打开，使用 hex 模式读取。使用`ctrl + z, x`推出。

  ```
  # 读取失败 /dev/ttyUSB0 IMU 串口没有释放
  
  ds18@ubuntu:~$ minicom -c on -H
  
  minicom: cannot open /dev/ttyUSB0: Input/output error
  
  =====================================================================
  
  # 成功读取 /dev/ttyUSB0 的 IMU 数据
  
  ds18@ubuntu:~$ minicom -c on -H
  
  Welcome to minicom 2.7.1
  
  OPTIONS: I18n 
  Compiled on Aug 13 2017, 15:25:34.
  Port /dev/ttyUSB0, 03:43:53
  
  Press CTRL-A Z for help on special keys
  
  f3 fc ff ff ff f9 ff e1 ff 52 00 c4 ff 66 26 4b 7f 80 20 00 02 00 
  ```

  

* cutecom 使用总结：

  使用`cutecom`，打开`/dev/ttyUSB0`读取失败。 ![image-20211130200850655](20211130 - imu ros development (init node).assets/image-20211130200850655.png)

   打开，读取成功（需要等待 os 释放端口句柄？）。![image-20211130201054859](20211130 - imu ros development (init node).assets/image-20211130201054859.png)



#### Task 02：关于 task 01 串口设备占用问题的一些解决办法

现在有两个设备需要使用 imu 数据，也就是要占用 /dev/ttyUSB0。会出现的情况：当一个设备结束使用 imu，释放 /dev/ttyUSB0，会有释放延时（系统调度问题）。 

当 ros: test_imu_serial.cpp 结束进程，minicom 立刻读取 /dev/ttyUSB0 中的数据，会出现：

```
ds18@ubuntu:~$ minicom -c on -H
minicom: cannot open /dev/ttyUSB0: Input/output error
```



一种可能的找到 /dev/ttyUSB0 被哪一个 process 占用的理想流程：

假设 ros: test_imu_serial.cpp 正在占用 /dev/ttyUSB0。使用`top | grep test*` 查看进程 pid：

```
ds18@ubuntu:~$ top | grep test*
 27192 ds18      20   0  404092  10652   9700 S   1.7  0.1   0:00.96 test_imu_serial     
 27192 ds18      20   0  404092  10652   9700 S   1.3  0.1   0:01.00 test_imu_serial     
 27192 ds18      20   0  404092  10652   9700 S   2.0  0.1   0:01.06 test_imu_serial     
 27192 ds18      20   0  404092  10652   9700 S   1.7  0.1   0:01.11 test_imu_serial     
```

找到 占用的 pid（27192）后，进入`/proc/27192` ：![image-20211130162325943](20211130 - imu ros development (init node).assets/image-20211130162325943.png)

进入`/fd`目录，并查看所有文件： ![image-20211130162535171](20211130 - imu ros development (init node).assets/image-20211130162535171.png)

```
ds18@ubuntu:/proc/27192$ cd fd
ds18@ubuntu:/proc/27192/fd$ ls
0  1  10  12  2  3  4  5  6  7  8  9
ds18@ubuntu:/proc/27192/fd$ ls -l
total 0
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 0 -> /dev/pts/5
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 1 -> /dev/pts/5
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 10 -> /dev/ttyUSB0
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 12 -> 'socket:[428757]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 2 -> /dev/pts/9
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 3 -> 'socket:[432533]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 4 -> 'anon_inode:[eventpoll]'
lr-x------ 1 ds18 ds18 64 Nov 29 23:51 5 -> 'pipe:[432537]'
l-wx------ 1 ds18 ds18 64 Nov 29 23:51 6 -> 'pipe:[432537]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 7 -> 'socket:[432542]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 8 -> 'socket:[432545]'
lrwx------ 1 ds18 ds18 64 Nov 29 23:51 9 -> 'socket:[432546]'
ds18@ubuntu:/proc/27192/fd$ 

```

可以看到 `/fd`目录下的`10 -> /dev/ttyUSB0`就是占用的端口，说明此时`/dev/ttyUSB0`被 pid=27192占用，如果想快速释放，可以使用`kill -9 27192`。

不过有一个问题需要注意，上面的解决办法是在知道执行程序的前提下进行的。如果我们不知道是哪一个执行程序在占用端口，那么就很复杂了。

再来一个例子：minicom 已经再使用`/dev/ttyUSB0`读取 imu 数据。我们查看其 pid 并关闭它，然后再用`test_imu_serial.cpp`读取 imu 数据。 ![image-20211130165923204](20211130 - imu ros development (init node).assets/image-20211130165923204.png)

从下至上：

```shell
# 4. minicom 显示的 imu hex 数据

# 3. 使用 top 查看 minicom pid

# 1. 使用 ps -a | grep mini 查看 minicom pid (27504)

# 2. 在 /proc/27504/fd 下找到 3 -> /dev/ttyUSB0

# 0. 使用 kill -9 27504 杀掉进程 27504

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

# * 运行 ros: test_imu_serial.cpp 显示 端口无法打开：

/home/ds18/catkin_x/IMU_ws/src/cmake-build-debug/devel/lib/imu_1129/test_imu_serial
[ERROR] [1638262911.528352652]: Unable to open port 

Process finished with exit code 255

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

# * 再次运行 ros: test_imu_serial.cpp 可以正常运行：

/home/ds18/catkin_x/IMU_ws/src/cmake-build-debug/devel/lib/imu_1129/test_imu_serial
[ INFO] [1638262996.610282856]: Serial Port initialized
[ INFO] [1638262996.611204440]: Reading from serial port

[ INFO] [1638262996.611257561]: Read: ?
[ INFO] [1638262996.631940408]: Reading from serial port

[ INFO] [1638262996.632009695]: Read: ?"
[ INFO] [1638262996.651928790]: Reading from serial port

[ INFO] [1638262996.651994743]: Read: ?
[ INFO] [1638262996.671842466]: Reading from serial port
```

可行的解决办法：

**在使用 .cpp，minicom，cutecom 读取串口数据时，如果出现`input/output error`，可以不用频繁拔插 IMU 串口，这可能是 os 释放串口延时导致的。解决办法是，等待一段时间，再次打开设备。在三种情况下验证可行。**



#### 知识补充：

##### 00: rospack serial: v8stdint.h 中类型整理

```cpp
#ifndef V8STDINT_H_  // 防止头文件被重复引用
#define V8STDINT_H_

#include <stddef.h>
#include <stdio.h>

#if defined(_WIN32) && !defined(__MINGW32__)

typedef signed char int8_t;					// 1字节，范围：-128 到 127(有符号位)
typedef unsigned char uint8_t;				// 1字节，范围： 0 到 255
typedef short int16_t;  // NOLINT			// 2字节，范围：-32,768 到 32,767
typedef unsigned short uint16_t;  // NOLINT // 2字节，范围：0 到 65,535
typedef int int32_t;              // 2或4字节，范围：-32,768 到 32,767 或 ...	
typedef unsigned int uint32_t;	  // 2或4字节，范围：0 到 65,535 或 0 到 4,294,967,295
typedef __int64 int64_t;					// 未知 __int64
typedef unsigned __int64 uint64_t;			// 未知 __int64
// intptr_t and friends are defined in crtdefs.h through stdio.h.

#else

#include <stdint.h>

#endif  // _WIN32 && !__MINGW32__

#endif  // V8STDINT_H_
```



关于：typedef

> typedef 为类型取一个新名字
>
> ```
> typedef unsigned char BYTE;  // 单字节数字定义了一个术语 BYTE
> BYTE  b1, b2;                // 标识符 BYTE 可作为类型 unsigned char 的缩写
> ```

关于：C 数据类型（菜鸟C）

**整数类型：**

![image-20211130104017894](20211130 - imu ros development (init node).assets/image-20211130104017894.png)

**浮点类型：**

![image-20211130104117639](20211130 - imu ros development (init node).assets/image-20211130104117639.png)

**void 类型：**

![image-20211130104210755](20211130 - imu ros development (init node).assets/image-20211130104210755.png)



---



### 参考：

link00: [**undefined reference in using serial.h**](https://answers.ros.org/question/320280/undefined-reference-in-using-serialh/)

link01: [ifndef define endif的作用](https://blog.csdn.net/bad_good_man/article/details/38682033?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

link02: [ROS串口通信（1）环境搭建](https://blog.csdn.net/fb_941219/article/details/84481689)

link03: [ROS串口通信（2）以十六进制指令读取IMU数据](https://blog.csdn.net/fb_941219/article/details/84486603?utm_medium=distribute.wap_feed_404.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.nonecase&depth_1-utm_source=distribute.wap_feed_404.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.nonecase)

link04: [Ubuntu关于串口的操作(查看串口信息、串口助手、串口权限)](https://blog.csdn.net/maizousidemao/article/details/103236666)

link05: [让Typora的图片居左的两种方法](https://blog.csdn.net/sky0816/article/details/111321812)

link06: [How to restart ttyusb* -- (验证不成功)](https://superuser.com/questions/572034/how-to-restart-ttyusb)

link07: [Linux查看端口占用情况,并强制释放占用的端口 -- (验证不成功)](https://developer.aliyun.com/article/481709)

link08: [解决Linux非root用户读写串口权限问题](https://blog.csdn.net/itas109/article/details/83027431)





---



