# RosLoadRadarStep



## 1. 文件：`APA_radar_exit.yaml`

```yaml
  - thread_name: planning
    # ignore: 1
    print_time: 0
    frame_rate: 20. # Each thread has its own frame rate.  # Use frame_period or frame_rate to control it.
    # frame_period: 0.050. # unit is second.
    thread_procedure:
      - key: RosLoadRadar
```



---



## 2. 文件：`ros_basic_ops.h`

```cpp
#ifdef BUILD_WITH_ROS
/**
 * @brief An operation to subscribe to radar range
 */
class RosLoadRadarStep : public OperationStep
{
public:
    explicit RosLoadRadarStep(const StepParameter& param);

    virtual void StepSetUp();


protected:

    TSP_ROS(void actOnRangeMsg(const sensor_msgs::RangeConstPtr& msg););

    virtual void Forward_cpu();

    std::vector<std::string> m_topic_names;
    tauristar::PoseState m_sensor_pose;
    static const int m_max_radar_num = 16;
    int m_radar_num;
    int m_counter;
    double m_wait_second;
    RadarDistanceSignals m_radar_distance_signal;

    TSP_ROS(RosSubscribePack m_sub_pack[m_max_radar_num];);
    
};
```

