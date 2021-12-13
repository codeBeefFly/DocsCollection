# 20211213_CHAP_02_ROS_COMMU

[TOC]

---

LOGS:

2021年12月13日：ros 通信



---



## 00. SOME POINTS

> 机器人是一种高度复杂的系统性实现，为了解耦合，在ROS中每一个功能点都是一个单独的进程，每一个进程都是独立运行的。
>
> **ROS是进程（也称为*****Nodes*****）的分布式框架。**
>
> 不同的进程是如何通信的？也即不同进程间如何实现数据交换的？

> ROS 中的基本通信机制主要有如下三种实现策略:
>
> - 话题通信(发布订阅模式)：topic -- talker <publish> + listener <subscrib>
> - 服务通信(请求响应模式)：service -- server <respond> + client <request> 
> - 参数服务器(参数共享模式)：parameter server



---

## 01. TOPIC (话题通信)

>话题通信是ROS中使用频率最高的一种通信模式，话题通信是基于**发布订阅**模式的，也即:一个节点发布消息，另一个节点订阅该消息。



>**概念**：以发布订阅的方式实现不同节点之间数据交互的通信模式。
>
>**作用**：用于不断更新的、少逻辑处理的数据传输场景。



### 01：理论模型（略）

> 该模型中涉及到三个角色:
>
> - ROS Master (管理者)
> - Talker (发布者)
> - Listener (订阅者)
>
> ROS Master 负责保管 Talker 和 Listener 注册的信息，并匹配话题相同的 Talker 与 Listener，帮助 Talker 与 Listener 建立连接，连接建立后，Talker 可以发布消息，且发布的消息会被 Listener 订阅。



参考：

link00: [2.1.1 话题通信理论模型 · GitBook (autolabor.com.cn)](http://www.autolabor.com.cn/book/ROSTutorials/di-2-zhang-ros-jia-gou-she-ji/22hua-ti-tong-xin/211-li-lun-mo-xing.html)



### 02：TOPIC 通信代码（cpp）

编写 TOPIC 通信代码，从设计到实现的流程：

1. 需求
2. 分析
3. 代码流程（发布方，talker）
4. 代码流程（接收方，listener）



#### 1. 需求：

> 1. 编写发布订阅实现
>
> 2. 发布方以10HZ(每秒10次)的频率发布文本消息
>
> 3. 订阅方订阅消息并将消息内容打印输出。


#### 2. 分析：

> 在模型实现中，需要关注的关键点有三个:
>
> 1. 发布方：talker（publish）
> 2. 接收方：listener（subscribe）
> 3. 数据（此处为普通文本）


#### 3. 流程：

>1. 编写发布方实现；
>2. 编写订阅方实现；
>3. 编辑配置文件；
>4. 编译并执行。



---

## 02. SERVICE (服务通信)





---

## 03. PARAMETER SERVER (参数服务器)





---

## 04.  







---

## 参考

link01: [rosparam和ROS参数服务](https://blog.csdn.net/u014695839/article/details/78348600)

link02: [在ROS中处理yaml文件](https://blog.csdn.net/u014610460/article/details/79508869)

link03: []()

link04: []()

