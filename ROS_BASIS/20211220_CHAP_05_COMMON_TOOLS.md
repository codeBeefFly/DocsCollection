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



## 02. 







## 03. 





--



## 参考：

