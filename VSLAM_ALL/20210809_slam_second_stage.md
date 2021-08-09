# 2021年08月09日：slam second stage

[toc]

---

## 0. review & plan

1. review：

   0803 已经完成了 hector slam + rfans，hector slam 没有办法形成闭环，只能进行算法学习（可以将 hector slam 当做 slam 研究的入门课题，以及对其进行商业落地可行性研究）。

2. plan：

   如果要实现闭环以及更精确的 slam，需要更复杂的方案，如：leGo_LOAM 和 vins mono：

   参考 1：[LVI-SAM安装与测试](https://blog.csdn.net/learning_tortosie/article/details/116051761)

3. **Open Motion Planning Library** (ryan)



---

## 1. some slam algorithm overview

1. hector mapping

2. gmapping

   2.1. overview:

   2.2. system requirement:

   2.3. sites:

   ​	site 1: [Gmapping算法原理及源代码解析<这篇博客对 gmapping 的应用框架讲的很详细>](https://cxx0822.github.io/2020/05/05/gmapping-suan-fa-yuan-li-ji-yuan-dai-ma-jie-xi/)

   ​	site 2: [ROS开发与应用<还是上面博主的文章，包含hector、gmapping、cartographer>](https://cxx0822.github.io/categories/ROS%E5%BC%80%E5%8F%91%E4%B8%8E%E5%BA%94%E7%94%A8/)

   ​	site 3: [基于turtlebot的定位与建图<继续上一个博主的文章，包含如何查看 lidar ros 代码，这哥们太仔细了>](https://cxx0822.github.io/2020/04/25/ji-yu-turtlebot-de-ding-wei-yu-jian-tu/)

3. **cartographer**

   3.1. overview: 

   [Cartographer](https://github.com/cartographer-project/cartographer) (制图师) is a system that provides real-time simultaneous localization and mapping ([SLAM](https://en.wikipedia.org/wiki/Simultaneous_localization_and_mapping)) in 2D and 3D across multiple platforms and sensor configurations.

   <img src="https://camo.githubusercontent.com/10d0032636bb1b2a266209bdc0b4fa48ab9ef9fb47dcf834701aa8a50384ddc3/68747470733a2f2f6a2e676966732e636f6d2f777033424a4d2e676966" alt="Cartographer 3D SLAM Demo" style="zoom:150%;float:left" />

   ![_images/demo_2d.gif](https://google-cartographer-ros.readthedocs.io/en/latest/_images/demo_2d.gif)

   3.2. system requirement: 

   - 64-bit, modern CPU (e.g. 3rd generation i7)
   - 16 GB RAM
   - Ubuntu 18.04 (Bionic), 20.04 (Focal)
   - gcc version 6.3.0, 7.5.0, 9.3.0

   3.3. sites

   ​	site 0: [cartographer-project](https://github.com/cartographer-project)/**[cartographer](https://github.com/cartographer-project/cartographer)**

   ​	site 1: [Cartographer](https://google-cartographer.readthedocs.io/en/latest/#cartographer)

   ​	site 2: [Cartographer ROS Integration](https://google-cartographer-ros.readthedocs.io/en/latest/#cartographer-ros-integration)

   ​	site 3: [Compiling Cartographer ROS](https://google-cartographer-ros.readthedocs.io/en/latest/compilation.html)

   

4. OpenVSLAM

5. leGO_LOAM

6. vins mono

注：1-3 可以作为 slam 基础研究实践，4-6 作为进阶研究实践。4-6 在研究过程中可以适当增删，1-3 结合 视觉 slam 研究。如果时间允许。



---

## &. reference link

