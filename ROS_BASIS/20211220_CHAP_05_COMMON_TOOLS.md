# 20211220_CHAP_05_COMMON_TOOLS

[TOC]



---



## 01. ROS TF (transform frame)

### 00. Some Points Noted

> ---
>
> 在明确了不同坐标系之间的的相对关系，就可以实现任何坐标点在不同坐标系之间的转换，但是该计算实现是较为常用的，且算法也有点复杂，因此在 ROS 中直接封装了相关的模块: 坐标变换(TF)。
>
> ---
>
> **坐标系:**
>
> ROS 中是通过坐标系统来标定物体的，确切的将是通过**右手坐标系**来标定的。
>
> #### **作用**
>
> 在 ROS 中用于实现**不同坐标系之间的点或向量的转换**。
>
> ---

<img src="http://www.autolabor.com.cn/book/ROSTutorials/assets/%E5%8F%B3%E6%89%8B%E5%9D%90%E6%A0%87%E7%B3%BB.jpg" alt="img" style="zoom:110%;" align="left" />



---



### 01. geometry_msgs（坐标msg消息）

> 在坐标转换实现中常用的 msg:
>
> `geometry_msgs/TransformStamped`：传输坐标系相关位置信息。
> `geometry_msgs/PointStamped`：传输某个坐标系内坐标点的信息。
>
> 在坐标变换中，频繁的需要使用到坐标系的相对关系以及坐标点信息。



#### 1. `geometry_msgs/TransformStamped`

传输坐标系相关位置信息。

```shell
rosmsg info geometry_msgs/TransformStamped

std_msgs/Header header                       #头信息
  uint32 seq                                 #|-- 序列号
  time stamp                                 #|-- 时间戳
  string frame_id                            #|-- 所属坐标系的 id
string child_frame_id                        #子坐标系的 id
geometry_msgs/Transform transform            #坐标信息
  geometry_msgs/Vector3 translation          #偏移量
    float64 x                                #|-- X 方向的偏移量
    float64 y                                #|-- Y 方向的偏移量
    float64 z                                #|-- Z 方向上的偏移量
  geometry_msgs/Quaternion rotation          #四元数：四元数用于表示坐标的相对姿态
    float64 x                                
    float64 y                                
    float64 z                                
    float64 w
```





#### 2. `geometry_msgs/PointStamped`

传输**某个坐标系内坐标点**的信息。

```shell
rosmsg info geometry_msgs/PointStamped

std_msgs/Header header                    	 #头信息
  uint32 seq                                 #|-- 序号
  time stamp                                 #|-- 时间戳
  string frame_id                            #|-- 所属坐标系的 id
geometry_msgs/Point point                    #点坐标
  float64 x                                  #|-- x y z 坐标
  float64 y
  float64 z
```



---



### 02. Static TF（静态坐标变换）

#### 0. Some Points Noted

>---
>
>**静态坐标变换**，是指两个坐标系之间的相对位置是固定的。
>
>---



#### 1. 需求与流程

> **需求：**
>
> ---
>
> 一机器人模型，核心构成包含主体与雷达，各对应一坐标系。
>
> 坐标系的原点分别位于主体与雷达的物理中心。
>
> 已知雷达原点相对于主体原点位移关系如下: `x: 0.2, y: 0.0, z: 0.5`。当前雷达检测到一障碍物，在雷达坐标系中障碍物的坐标为 `(2.0, 3.0, 5.0)`，请问，该障碍物相对于主体的坐标是多少？
>
> ---



> **实现分析:**
>
> ---
>
> 1. 坐标系相对关系，可以通过发布方发布
> 2. 订阅方，订阅到发布的坐标系相对关系，再传入坐标点信息(可以写死)，然后借助于 tf 实现坐标变换，并将结果输出
>
> ---



> **实现流程:**
>
> ---
>
> 1. 新建功能包，添加依赖（依赖就是包，使用 `rospack find` 找）
> 2. 编写发布方实现
> 3. 编写订阅方实现
> 4. 执行并查看结果
>
> ---



#### 2. C++ 实现 ★

创建项目功能包依赖于 
```
tf2
tf2_ros
tf2_geometry_msgs
roscpp 
rospy 
std_msgs 
geometry_msgs
```



##### 1. 发布方（talker）★

`demo_06_tf_talker_node.cpp`

```cpp
//
// Created by ds18 on 12/20/21.
//

/*
    静态坐标变换发布方:
        发布关于 laser 坐标系的位置信息

    实现流程:
        1.包含头文件
        2.初始化 ROS 节点
        3.创建静态坐标转换广播器
        4.创建坐标系信息
        5.广播器发布坐标系信息
        6.spin()
*/


// 1.包含头文件
#include "ros/ros.h"
#include "tf2_ros/static_transform_broadcaster.h"
#include "geometry_msgs/TransformStamped.h"
#include "tf2/LinearMath/Quaternion.h"

int main(int argc, char *argv[]) {
    setlocale(LC_ALL, "");

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "demo_06_tf_static_broadcast");

    // 3.创建静态坐标转换广播器
    tf2_ros::StaticTransformBroadcaster broadcaster;

    // 4.创建坐标系信息（加印花的TF，也就是时间戳）
    geometry_msgs::TransformStamped ts;

    //----设置头信息
    ts.header.seq = 100;
    ts.header.stamp = ros::Time::now();
    ts.header.frame_id = "base_link";
    //----设置子级坐标系
    ts.child_frame_id = "laser";
    //----设置子级相对于父级的偏移量
    ts.transform.translation.x = 0.2;
    ts.transform.translation.y = 0.0;
    ts.transform.translation.z = 0.5;
    //----设置四元数:将 欧拉角数据转换成四元数
    tf2::Quaternion qtn;
    qtn.setRPY(0, 0, 0);
    ts.transform.rotation.x = qtn.getX();
    ts.transform.rotation.y = qtn.getY();
    ts.transform.rotation.z = qtn.getZ();
    ts.transform.rotation.w = qtn.getW();

    // 5.广播器发布坐标系信息
    broadcaster.sendTransform(ts);

    // 进入循环执行回调函数
    ros::spin();
    return 0;
}

```

###### 代码解释

```cpp
// 1. 确定代码流程。1, 2, 3, 4, 5

// 2. 需要创建静态坐标转换广播器，用来发布 静态坐标系
tf2_ros::StaticTransformBroadcaster broadcaster;

// 3. 创建静态坐标系转换信息，包含时间戳（加印花的TF，也就是时间戳）
geometry_msgs::TransformStamped ts;

/*
rosmsg info geometry_msgs/TransformStamped

std_msgs/Header header                       ## 头信息
  uint32 seq                                 # |-- 序列号
  time stamp                                 # |-- 时间戳
  string frame_id                            # |-- 所属坐标系的 id
string child_frame_id                        ## 子坐标系的 id
geometry_msgs/Transform transform            ## 坐标信息
  geometry_msgs/Vector3 translation          # 偏移量
    float64 x                                # |-- X 方向的偏移量
    float64 y                                # |-- Y 方向的偏移量
    float64 z                                # |-- Z 方向上的偏移量
  geometry_msgs/Quaternion rotation          # 四元数：四元数用于表示坐标的相对姿态
    float64 x                                
    float64 y                                
    float64 z                                
    float64 w
*/

// 4. 广播器发布静态坐标系转换信息
/*
send a transfomStamped msg. the stamped data structure includes frame_id, time, parent_id...
*/
broadcaster.sendTransform(ts);
```



##### 2. 订阅方（listener）★

`demo_06_tf_listener_node.cpp`

```cpp
//
// Created by ds18 on 12/20/21.
//

/*
    订阅坐标系信息，生成一个相对于 子级坐标系的坐标点数据，转换成父级坐标系中的坐标点

    实现流程:
        1.包含头文件
        2.初始化 ROS 节点
        3.创建 TF 订阅节点
        4.生成一个坐标点(相对于子级坐标系)
        5.转换坐标点(相对于父级坐标系)
        6.spin()
*/

//1.包含头文件
#include "ros/ros.h"
#include "tf2_ros/transform_listener.h"
#include "tf2_ros/buffer.h"
#include "geometry_msgs/PointStamped.h"
#include "tf2_geometry_msgs/tf2_geometry_msgs.h" //注意: 调用 transform 必须包含该头文件

int main(int argc, char *argv[]) {
    setlocale(LC_ALL, "");

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "demo_06_tf_static_subcribe");
    ros::NodeHandle nh;

    // 3.创建 TF 订阅节点
    tf2_ros::Buffer buffer;
    tf2_ros::TransformListener listener(buffer);

    ros::Rate r(1);
    while (ros::ok()) {

        // 4.生成一个坐标点(相对于子级坐标系)
        geometry_msgs::PointStamped point_laser;
        point_laser.header.frame_id = "laser";
        point_laser.header.stamp = ros::Time::now();
        point_laser.point.x = 1;
        point_laser.point.y = 2;
        point_laser.point.z = 7.3;

        // 5.转换坐标点(相对于父级坐标系)
        //新建一个坐标点，用于接收转换结果
        //--使用 try 语句或休眠，否则可能由于缓存接收延迟而导致坐标转换失败--
        try {
            geometry_msgs::PointStamped point_base;
            point_base = buffer.transform(point_laser, "base_link");
            ROS_INFO("转换后的数据:(%.2f, %.2f, %.2f),参考的坐标系是: %s", point_base.point.x, point_base.point.y, point_base.point.z,
                     point_base.header.frame_id.c_str());

        }
        catch (const std::exception &e) {
            // std::cerr << e.what() << '\n';
            ROS_INFO("程序异常.....");
        }

        r.sleep();
        // 执行一次回调函数(没有循环)
        ros::spinOnce();
    }

    return 0;
}
```

###### 代码解释

```shell
rosrun rqt_graph rqt_graph

rosrun rqt_tf_tree rqt_tf_tree
```



```cpp
// 1. 确定业务流程

// 3. 创建 TF 对象
// 创建一个 buffer 用来存储一个坐标系，并启动一个服务 tf_frames
// 创建一个 listener 来 request 和 receive 坐标系转换信息
/*
Stores known frames and offers a ROS service, "tf_frames", which responds to client requests with a response containing a tf2_msgs::FrameGraph representing the relationship of known frames.
存储一个已知的坐标系，同时提供一个 ROS 服务："tf_frames"。
*/
tf2_ros::Buffer buffer;
/*
provides an easy way to request and receive coordinate frame transform information

// constructor for transform listener
TransformListener(tf2::BufferCore& buffer, bool spin_thread = true);
*/
tf2_ros::TransformListener listener(buffer);

// 4.生成一个坐标点(相对于子级坐标系, laser)
/*
$ rosmsg info geometry_msgs/PointStamped 
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
geometry_msgs/Point point
  float64 x
  float64 y
  float64 z
*/
geometry_msgs::PointStamped point_laser;  	// 创建带时间戳的坐标点对象 point_laser
point_laser.header.frame_id = "laser";		// 设定坐标点 坐标系为 laser
point_laser.header.stamp = ros::Time::now();// 设定坐标点 时间戳为 now
point_laser.point.x = 1;					// 设定坐标点的 x 坐标相对 laser 坐标系为 1
point_laser.point.y = 2;					// 设定坐标点的 y 坐标相对 laser 坐标系为 2
point_laser.point.z = 7.3;					// 设定坐标点的 z 坐标相对 laser 坐标系为 7.3


// 5.转换坐标点(相对于父级坐标系, base_link)		
geometry_msgs::PointStamped point_base;		// 创建带时间戳的坐标点对象 point_base
/*
Transform an input into the target frame and convert to a specified output type. It is templated on two types: the type of the input object and the type of the transformed output.
将 input 转换到 目标坐标系下，同时转换到特定的 output 类型。
以两种类型模板化：input 对象类型，转换的 output 对象类型。

Template Parameters
A: The type of the object to transform.
B: The type of the transformed output.

Parameters
in: The object to transform
out: The transformed output, converted to the specified type.
target_frame: The string identifer for the frame to transform into.
timeout: How long to wait for the target frame. Default value is zero (no blocking).

Returns
The transformed output, converted to the specified type.

  template <class A, class B>
    B& transform(const A& in, B& out,
        const std::string& target_frame, ros::Duration timeout=ros::Duration(0.0)) const
  {
    A copy = transform(in, target_frame, timeout);
    tf2::convert(copy, out);
    return out;
  }
  
---+---+---+---+---+---+---+---+---+---+---+---+---+
这段代码中：
in: 	point_laser: geometry_msgs::PointStamped
out:	point_base:  geometry_msgs::PointStamped	
target_frame: "base_link"
*/
point_base = buffer.transform(point_laser, "base_link");
```





##### 3. 配置文件 ★

由于使用的是标准消息，所以不需要配置`package.xml`文件。

配置`CMakeLists.txt`：

只显示相关修改：

```cmake
find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        std_msgs
        message_generation  # 需要加入 message_generation,必须有 std_msgs

        geometry_msgs       # demo_06 tf
        tf2                 # demo_06 tf
        tf2_ros             # demo_06 tf
        tf2_geometry_msgs   # demo_06 tf
        )


## demo_06: tf talker
add_executable(demo_06_tf_talker
        src/demo_06_tf_talker_node.cpp
        )

## demo_06: tf listener
add_executable(demo_06_tf_listener
        src/demo_06_tf_listener_node.cpp
        )
        

## demo_06: tf talker
target_link_libraries(demo_06_tf_talker
        ${catkin_LIBRARIES}
        )

## demo_06: tf listener
target_link_libraries(demo_06_tf_listener
        ${catkin_LIBRARIES}
        )
```

###### cmake 讲解

```
（暂时略）
```





#### 3. 实现结果

<img src="20211220_CHAP_05_COMMON_TOOLS.assets/image-20211221131955249.png" alt="image-20211221131955249" style="zoom:80%;" align="left"/>

<img src="20211220_CHAP_05_COMMON_TOOLS.assets/image-20211221132110131.png" alt="image-20211221132110131" style="zoom:80%;" align="left"/>

```SHELL
$ rosnode list

/demo_06_tf_static_broadcast
/demo_06_tf_static_subscribe


$ rostopic list

/clicked_point					
/initialpose				# Type: geometry_msgs/PoseWithCovarianceStamped
/move_base_simple/goal		# Type: geometry_msgs/PoseStamped
/rosout						
/rosout_agg
/tf							# Type: tf2_msgs/TFMessage
/tf_static					# Type: tf2_msgs/TFMessage
```

查看相关的 rosnode 信息：`rosnode info`

```shell
$ rosnode info /demo_06_tf_static_broadcast 
--------------------------------------------------------------------------------
Node [/demo_06_tf_static_broadcast]
Publications: 
 * /rosout [rosgraph_msgs/Log]
 * /tf_static [tf2_msgs/TFMessage]

Subscriptions: None

Services: 
 * /demo_06_tf_static_broadcast/get_loggers
 * /demo_06_tf_static_broadcast/set_logger_level


contacting node http://ubuntu:41321/ ...
Pid: 107039
Connections:
 * topic: /rosout
    * to: /rosout
    * direction: outbound (34193 - 127.0.0.1:40940) [12]
    * transport: TCPROS
 * topic: /tf_static
    * to: /rviz_1640052818025969474
    * direction: outbound (34193 - 127.0.0.1:40938) [10]
    * transport: TCPROS
 * topic: /tf_static
    * to: /demo_06_tf_static_subscribe
    * direction: outbound (34193 - 127.0.0.1:40950) [13]
    * transport: TCPROS

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+


$ rosnode info /demo_06_tf_static_subscribe 
--------------------------------------------------------------------------------
Node [/demo_06_tf_static_subscribe]
Publications: 
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /tf [unknown type]
 * /tf_static [tf2_msgs/TFMessage]

Services: 
 * /demo_06_tf_static_subscribe/get_loggers
 * /demo_06_tf_static_subscribe/set_logger_level


contacting node http://ubuntu:43257/ ...
Pid: 107128
Connections:
 * topic: /rosout
    * to: /rosout
    * direction: outbound (42035 - 127.0.0.1:40484) [13]
    * transport: TCPROS
 * topic: /tf_static
    * to: /demo_06_tf_static_broadcast (http://ubuntu:41321/)
    * direction: inbound (40950 - ubuntu:34193) [12]
    * transport: TCPROS
```

与 tf 相关的 topic：

```shell
$ rostopic info /initialpose
Type: geometry_msgs/PoseWithCovarianceStamped

Publishers: 
 * /rviz_1640052818025969474 (http://ubuntu:34477/)

Subscribers: None

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

$ rostopic info /move_base_simple/goal
Type: geometry_msgs/PoseStamped

Publishers: 
 * /rviz_1640052818025969474 (http://ubuntu:34477/)

Subscribers: None

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

$ rostopic info /tf
Type: tf2_msgs/TFMessage

Publishers: None

Subscribers: 
 * /rviz_1640052818025969474 (http://ubuntu:34477/)
 * /demo_06_tf_static_subscribe (http://ubuntu:43257/)

---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

$ rostopic info /tf_static
Type: tf2_msgs/TFMessage

Publishers: 
 * /demo_06_tf_static_broadcast (http://ubuntu:41321/)

Subscribers: 
 * /rviz_1640052818025969474 (http://ubuntu:34477/)
 * /demo_06_tf_static_subscribe (http://ubuntu:43257/)
```



##### 关于如何查看一个 rosnode 中的所包含的消息信息：

这样就可以避免盲查。

**就当前程序来说，查看消息的流程：**

```shell
# 1. 使用 rosnode list 查看当前运行的 node 节点，可以使用 rosnode cleanup 进行清理。

# 2. 使用 rosnode info 查看目标 node 节点信息。

# 3.1. 在结果的 Publications、Subscriptions 中可以得到 
#	1. 此 node 节点发布、订阅的 topic 列表，以及 这些 topic 说使用的消息类别。
#   2. 此 node 节点包含的服务 Services 列表中。
# 3.2. 同样的，也可以获取 目标 node 中连接的对象信息 Connections，比如 topic。

# 4. 在 3.2 中，可以使用 rostopic type topic_name 获取 topic 包含的消息类型 <message type>

# 5. 使用 rosmsg info <message type> 查看消息的信息
```

**示例：**

> ---
>
> **步骤1：**rosnode list
>
> ```shell
> $ rosnode list
> /demo_06_tf_static_broadcast
> /demo_06_tf_static_subscribe
> /rosout
> /rviz_1640052818025969474
> 
> ```
>
> ---
>
> **步骤2：**rosnode info 查看目标 node：/demo_06_tf_static_broadcast 节点信息
>
> ```shell
> $ rosnode info /demo_06_tf_static_broadcast 
> --------------------------------------------------------------------------------
> Node [/demo_06_tf_static_broadcast]
> Publications: 
>  * /rosout [rosgraph_msgs/Log]
>  * /tf_static [tf2_msgs/TFMessage]
> 
> Subscriptions: None
> 
> Services: 
>  * /demo_06_tf_static_broadcast/get_loggers
>  * /demo_06_tf_static_broadcast/set_logger_level
> 
> 
> contacting node http://ubuntu:41321/ ...
> Pid: 107039
> Connections:
>  * topic: /rosout
>     * to: /rosout
>     * direction: outbound (34193 - 127.0.0.1:40940) [12]
>     * transport: TCPROS
>  * topic: /tf_static
>     * to: /rviz_1640052818025969474
>     * direction: outbound (34193 - 127.0.0.1:40938) [10]
>     * transport: TCPROS
>  * topic: /tf_static
>     * to: /demo_06_tf_static_subscribe
>     * direction: outbound (34193 - 127.0.0.1:40950) [13]
>     * transport: TCPROS
> 
> ```
>
> ---
>
> **步骤3：**从 Publications、Subscriptions、Services 以及 Connections 查看 包含的 topic 或 消息。这里可以确定是 topic 是：/tf_static，topic 中的通信数据是：/tf2_msgs/TFMessage
>
> ---
>
> **步骤4：**在确定目标 node 节点 的目标 topic 名称之后，就可以查阅 这个 topic 使用的消息类别：rostopic type topic_name，获得的结果是 消息类型 `<message_type>`
>
> ```shell
> $ rostopic type /tf_static
> tf2_msgs/TFMessage
> ```
>
> ---
>
> **步骤5：**使用 `rosmsg info <message_type>` 查看消息信息：
>
> ```shell
> $ rosmsg info tf2_msgs/TFMessage
> geometry_msgs/TransformStamped[] transforms
>   std_msgs/Header header
>     uint32 seq
>     time stamp
>     string frame_id
>   string child_frame_id
>   geometry_msgs/Transform transform
>     geometry_msgs/Vector3 translation
>       float64 x
>       float64 y
>       float64 z
>     geometry_msgs/Quaternion rotation
>       float64 x
>       float64 y
>       float64 z
>       float64 w
> ```
>
> ---



#### 4. 补充

<img src="20211220_CHAP_05_COMMON_TOOLS.assets/image-20211221170339221.png" alt="image-20211221170339221" style="zoom:120%;" align="left"/>



---



### 03. Dynamic TF（动态坐标转换）

#### 0. Some Ponts Noted

> ---
>
> **动态坐标变换，是指两个坐标系之间的相对位置是变化的。**
>
> ---



#### 1. 需求与流程

> ---
>
> **需求描述:**
>
> 启动 turtlesim_node，该节点中窗体有一个世界坐标系（world）（左下角为坐标系原点），乌龟是另一个坐标系（turtle1），键盘控制乌龟运动，将两个坐标系的相对位置动态发布。
>
> ---
>
> **实现分析:**
>
> 1. 乌龟本身不但可以看作坐标系，也是世界坐标系中的一个坐标点
>
>    乌龟自身是坐标系，也是世界坐标系的点。
>
> 2. 订阅 turtle1/pose,可以获取乌龟在**世界坐标系**的 x坐标、y坐标、偏移量以及线速度和角速度
>
> 3. 将 pose 信息转换成 坐标系相对信息并发布（怎么转，转出的数据代表什么？）
>
> ---
>
> **实现流程：**C++ 与 Python 实现流程一致
>
> 1. 新建功能包，添加依赖
> 2. 创建**坐标相对关系发布方**(同时需要订阅乌龟位姿信息)
> 3. 创建**坐标相对关系订阅方**
> 4. 执行
>
> ---



#### 2. C++ 实现 ★

##### 1. 发布方（talker）

**`demo_07_tf_dynamic_talker_node.cpp`**

````cpp
//
// Created by ds18 on 12/21/21.
//

/*
    动态的坐标系相对姿态发布(一个坐标系相对于另一个坐标系的相对姿态是不断变动的)

    需求: 启动 turtlesim_node,该节点中窗体有一个世界坐标系(左下角为坐标系原点)，乌龟是另一个坐标系，键盘
    控制乌龟运动，将两个坐标系的相对位置动态发布

    实现分析:
        1.乌龟本身不但可以看作坐标系，也是世界坐标系中的一个坐标点
        2.订阅 turtle1/pose,可以获取乌龟在世界坐标系的 x坐标、y坐标、偏移量以及线速度和角速度
        3.将 pose 信息转换成 坐标系相对信息并发布

    实现流程:
        1.包含头文件
        2.初始化 ROS 节点
        3.创建 ROS 句柄
        4.创建订阅对象
        5.回调函数处理订阅到的数据(实现TF广播)
            5-1.创建 TF 广播器
            5-2.创建 广播的数据(通过 pose 设置)
            5-3.广播器发布数据
        6.spin
*/

// 1.包含头文件
#include "ros/ros.h"
#include "turtlesim/Pose.h"
#include "tf2_ros/transform_broadcaster.h"
#include "geometry_msgs/TransformStamped.h"
#include "tf2/LinearMath/Quaternion.h"

void doPose(const turtlesim::Pose::ConstPtr &pose) {
    //  5-1.创建 TF 广播器
    static tf2_ros::TransformBroadcaster broadcaster;

    //  5-2.创建 广播的数据(通过 pose 设置)
    geometry_msgs::TransformStamped tfs;

    //  |----头设置
    tfs.header.frame_id = "world";
    tfs.header.stamp = ros::Time::now();

    //  |----坐标系 ID
    tfs.child_frame_id = "turtle1";

    //  |----坐标系相对信息设置
    tfs.transform.translation.x = pose->x;
    tfs.transform.translation.y = pose->y;
    tfs.transform.translation.z = 0.0; // 二维实现，pose 中没有z，z 是 0

    //  |--------- 四元数设置
    tf2::Quaternion qtn;
    qtn.setRPY(0, 0, pose->theta);
    tfs.transform.rotation.x = qtn.getX();
    tfs.transform.rotation.y = qtn.getY();
    tfs.transform.rotation.z = qtn.getZ();
    tfs.transform.rotation.w = qtn.getW();

    //  5-3.广播器发布数据
    broadcaster.sendTransform(tfs);
}

int main(int argc, char *argv[]) {
    setlocale(LC_ALL, "");

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "demo_07_dynamic_tf_pub");

    // 3.创建 ROS 句柄
    ros::NodeHandle nh;

    // 4.创建订阅对象
    // 5.回调函数处理订阅到的数据(实现TF广播)
    // 订阅的 topic：/turtle1/pose 是由 /turtlesim 发布的（龟龟模拟器）
    ros::Subscriber sub = nh.subscribe<turtlesim::Pose>("/turtle1/pose", 1000, doPose);

    // 6.spin
    ros::spin();
    return 0;
}

````

###### 代码解释

```cpp
/* 1. 这个是发布方源码
		
*/
```





##### 2. 订阅方（listener）

**`demo_07_tf_dynamic_listener_node.cpp`**

````cpp
//
// Created by ds18 on 12/21/21.
//

//1.包含头文件
#include "ros/ros.h"
#include "tf2_ros/transform_listener.h"
#include "tf2_ros/buffer.h"
#include "geometry_msgs/PointStamped.h"
#include "tf2_geometry_msgs/tf2_geometry_msgs.h" //注意: 调用 transform 必须包含该头文件

int main(int argc, char *argv[]) {
    setlocale(LC_ALL, "");

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "demo_07_dynamic_tf_sub");
    ros::NodeHandle nh;

    // 3.创建 TF 订阅节点
    tf2_ros::Buffer buffer;
    tf2_ros::TransformListener listener(buffer);

    ros::Rate r(1);
    while (ros::ok()) {
        // 4.生成一个坐标点(相对于子级坐标系)
        geometry_msgs::PointStamped point_laser;
        point_laser.header.frame_id = "turtle1";
        point_laser.header.stamp = ros::Time();
        point_laser.point.x = 1;
        point_laser.point.y = 1;
        point_laser.point.z = 0;

        // 5.转换坐标点(相对于父级坐标系)
        //新建一个坐标点，用于接收转换结果
        //--使用 try 语句或休眠，否则可能由于缓存接收延迟而导致坐标转换失败--
        try {
            geometry_msgs::PointStamped point_base;
            point_base = buffer.transform(point_laser, "world");
            ROS_INFO("坐标点相对于 world 的坐标为:(%.2f,%.2f,%.2f)", point_base.point.x, point_base.point.y, point_base.point.z);
        }
        catch (const std::exception &e) {
            // std::cerr << e.what() << '\n';
            ROS_INFO("程序异常:%s", e.what());
        }

        r.sleep();
        ros::spinOnce();
    }

    return 0;
}

````

###### 代码解释



##### 3. 配置文件 ★

只记录修改项。

`CMakeLists.txt`

```cmake
## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
        roscpp
        rospy
        std_msgs
        message_generation  # 需要加入 message_generation,必须有 std_msgs

        geometry_msgs       # demo_06 tf static
        tf2                 # demo_06 tf static
        tf2_ros             # demo_06 tf static
        tf2_geometry_msgs   # demo_06 tf static

        turtlesim           # demo_07 tf dynamic
        )


######################################################################


## demo_07: tf dynamic talker
add_executable(demo_07_tf_dynamic_talker
        src/demo_07_tf_dynamic_talker_node.cpp
        )

## demo_07: tf dynamic listener
add_executable(demo_07_tf_dynamic_listener
        src/demo_07_tf_dynamic_listener_node.cpp
        )


######################################################################


## demo_07: tf dynamic talker
target_link_libraries(demo_07_tf_dynamic_talker
        ${catkin_LIBRARIES}
        )

## demo_07: tf dynamic listener
target_link_libraries(demo_07_tf_dynamic_listener
        ${catkin_LIBRARIES}
        )
```

###### cmake 讲解



##### 4. launch 文件

```xml
<launch>

    <!-- run turtlesim_node node: turtlesim_node -->
    <node pkg="turtlesim" type="turtlesim_node" 
          name="demo_07_tutlesim_node" required="true"/>

    <!-- run turtle_teleop_key node: turtle_teleop_key ? -->
    <node pkg="turtlesim" type="turtle_teleop_key" 
          name="demo_07_turtle_teleop_key"/>

    <!-- run talker node: demo_07_dynamic_tf_pub -->
    <node pkg="test_pkg" type="demo_07_tf_dynamic_talker" 
          name="demo_07_dynamic_tf_pub" />

    <!-- run listener node: demo_07_dynamic_tf_sub -->
    <node pkg="test_pkg" type="demo_07_tf_dynamic_listener" 
          name="demo_07_dynamic_tf_sub" />

    <!-- run rviz -->
    <node pkg="rviz" type="rviz" 
          name="rviz" 
          args="-d $(find test_pkg)/rviz/demo_07_tf_dynamic.rviz"/>

</launch>

<!-- ----------------------------------
	node  --- 包含的某个节点
		pkg ----- 功能包: 使用 rospack find 查找到的 pkg_name
		type ---- 被运行的节点文件: add_executable 中的二进制名称
		name   -- 为节点命名: 使用 rosnode list 得到的 node 名称
		output  - 设置日志的输出目标
----------------------------------- -->
```

###### 代码解释：

> ---
>
> 通过 launch 文件设置参数的方式前面已经介绍过了。
>
> 可以在 node 标签外，或 node 标签中通过 param 或 rosparam 来设置参数。
>
> 在 node 标签外设置的参数是全局性质的，参考的是 `/` 。
>
> 在 node 标签中设置的参数是私有性质的，参考的是 `/命名空间/节点名称`。
>
> ---
>
> 
>
> ```xml
> <launch>
> 
>     <param name="p1" value="100" />
>     <node pkg="turtlesim" type="turtlesim_node" name="t1">
>         <param name="p2" value="100" />
>     </node>
> 
> </launch>
> ```
>
> ---



#### 3. 实现结果与调试：

**`rosrun rqt_tf_tree rqt_tf_tree`**

<img src="20211220_CHAP_05_COMMON_TOOLS.assets/image-20211223105330199.png" alt="image-20211223105330199" style="zoom:80%;" align="left"/>

**`rqt_graph`**

<img src="20211220_CHAP_05_COMMON_TOOLS.assets/image-20211223105414980.png" alt="image-20211223105414980" style="zoom:80%;" align="left"/>

<img src="20211220_CHAP_05_COMMON_TOOLS.assets/image-20211223105450487.png" alt="image-20211223105450487" style="zoom:80%;" align="left"/>



```shell
$ rosnode list

/demo_07_dynamic_tf_pub
/demo_07_dynamic_tf_sub
/rosout
/teleop_turtle
/turtlesim


---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+


ds18@ubuntu:~/catkin_x2/IMU_ws_mk2/IMU_ws_mk2/LINES354_ws$ rosnode info /demo_07_dynamic_tf_pub 
--------------------------------------------------------------------------------
Node [/demo_07_dynamic_tf_pub]
Publications: 
 * /rosout [rosgraph_msgs/Log]
 * /tf [tf2_msgs/TFMessage]

Subscriptions: 
 * /turtle1/pose [turtlesim/Pose]

Services: 
 * /demo_07_dynamic_tf_pub/get_loggers
 * /demo_07_dynamic_tf_pub/set_logger_level


contacting node http://ubuntu:42885/ ...
Pid: 121306
Connections:
 * topic: /rosout
    * to: /rosout
    * direction: outbound (50609 - 127.0.0.1:59432) [13]
    * transport: TCPROS
 * topic: /tf
    * to: /demo_07_dynamic_tf_sub
    * direction: outbound (50609 - 127.0.0.1:59442) [11]
    * transport: TCPROS
 * topic: /turtle1/pose
    * to: /turtlesim (http://ubuntu:38797/)
    * direction: inbound (38568 - ubuntu:51303) [12]
    * transport: TCPROS


---+---+---+---+---+---+---+---+---+---+---+---+---+---+---+

    
ds18@ubuntu:~/catkin_x2/IMU_ws_mk2/IMU_ws_mk2/LINES354_ws$ rosnode info /demo_07_dynamic_tf_sub 
--------------------------------------------------------------------------------
Node [/demo_07_dynamic_tf_sub]
Publications: 
 * /rosout [rosgraph_msgs/Log]

Subscriptions: 
 * /tf [tf2_msgs/TFMessage]
 * /tf_static [unknown type]

Services: 
 * /demo_07_dynamic_tf_sub/get_loggers
 * /demo_07_dynamic_tf_sub/set_logger_level


contacting node http://ubuntu:44661/ ...
Pid: 121409
Connections:
 * topic: /rosout
    * to: /rosout
    * direction: outbound (55439 - 127.0.0.1:44052) [13]
    * transport: TCPROS
 * topic: /tf
    * to: /demo_07_dynamic_tf_pub (http://ubuntu:42885/)
    * direction: inbound (59442 - ubuntu:50609) [12]
    * transport: TCPROS

```





---



### 04. Multi-cord. TF（多坐标变换）







---





## 02. XX







## 03. XX





--



## 参考：

