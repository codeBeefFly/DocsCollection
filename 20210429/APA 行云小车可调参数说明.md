# APA: 行云小车可调参数说明

[TOC]

---

## 简述表





| S.NO | 可调参数             | 默认             | 范围                                       | 说明                                                         |
| ---- | -------------------- | ---------------- | ------------------------------------------ | ------------------------------------------------------------ |
| 1    | MAX_STEERING_ANGLE   | PI * 0.4 （72°） | （问硬件）                                 | 转向轮最大转角                                               |
| 2    | MIN_CAR_SPEED        | 0                | 一般不会设为 0                             | 程序执行时，最小行进速度                                     |
| 3    | PARKING_OFFSET       | 0.0f             | 负值：[-3.5, -2]            正值：[0, 1.5] | 车位补偿量，如果此值大于0，通过CD坐标中心点计算泊入点；如果小于0，通过AB坐标中心点计算泊入点 |
| 4    | FRONT_WHEEL_X_OFFSET | 0.0f             | （问硬件）                                 |                                                              |
| 5    | FRONT_WHEEL_Y_OFFSET | 0.0f             | （问硬件）                                 |                                                              |
| 6    | REAR_WHEEL_X_OFFSET  | 0.0f             | （问硬件）                                 |                                                              |
| 7    | REAR_WHEEL_Y_OFFSET  | 0.0f             | （问硬件）                                 |                                                              |
|      |                      |                  |                                            |                                                              |



---

## 详细说明

###  MAX_STEERING_ANGLE

```
global_parameters.cpp
```

###  MIN_CAR_SPEED 

```
global_parameters.cpp
```

###  PARKING_OFFSET

```
APA_ParkSpaceGridmap.cpp

            if (PARKING_OFFSET < 0) {
                x = ptAB.x + PARKING_OFFSET * cos(ang_rad);
                y = ptAB.y + PARKING_OFFSET * sin(ang_rad);

            } else {
                if (REAR_WHEEL_AXIS > PARKING_OFFSET) {
                    x = ptCD.x + REAR_WHEEL_AXIS * cos(ang_rad);
                    y = ptCD.y + REAR_WHEEL_AXIS * sin(ang_rad);
                } else {
                    x = ptCD.x + PARKING_OFFSET * cos(ang_rad);
                    y = ptCD.y + PARKING_OFFSET * sin(ang_rad);
                }
            }
```

###  FRONT_WHEEL_X_OFFSET

```
car_param.cpp
```

###  FRONT_WHEEL_Y_OFFSET

```
car_param.cpp
```

###  REAR_WHEEL_X_OFFSET 

```
car_param.cpp
```

###  REAR_WHEEL_Y_OFFSET 

```
car_param.cpp
```

