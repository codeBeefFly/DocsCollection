# 20220408 学习 dcvs apa parking perpend

<!-- TOC -->

- [20220408 学习 dcvs apa parking perpend](#20220408-学习-dcvs-apa-parking-perpend)
  - [1. 一些信息](#1-一些信息)
  - [1.2. ParkingMiddleGoalStep 代码框架](#12-parkingmiddlegoalstep-代码框架)
    - [1.2.1 Forward_cpu() 框架](#121-forward_cpu-框架)

<!-- /TOC -->



---

目的：
1. 阅读 dcvs perpend. parking 代码
   1. 框架代码：（拿点，设点，数据结构，输入输出）
   2. 算法代码：路径计算（之后里程计算）
2. 写自己的代码
   1. 框架代码：（拿点，输入输出）
   2. 算法研究，开发，移植

---
  
    

## 1. 一些信息
 
 代码分支：master --> dev-20220408-read-parking-code
 代码版本：cd0d1b1dd69be730f75ddd7c796d69a33cc75e7d


## 1.2. ParkingMiddleGoalStep 代码框架
尽情的写注释吧！  
尽情的跑仿真吧！


### 1.2.1 Forward_cpu() 框架

关于这段语法:
```cpp
CurrentPoseState* pose_state = m_sharedstate_holder->getCurrentPoseState();

/* 相关代码

CurrentPoseState* pose_state = m_sharedstate_holder->getCurrentPoseState();

CurrentPoseState* pose_state = (CurrentPoseState*)(States[CurrentPoseType]);

void* States[StateTypeNum];

*/
```
参考：[C++栈上和堆上创建对象的区别](https://blog.csdn.net/error_again/article/details/112601202)

解析：
涉及知识： c++ 创建类对象

