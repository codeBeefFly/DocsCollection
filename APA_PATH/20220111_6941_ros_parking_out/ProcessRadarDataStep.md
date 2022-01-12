# ProcessRadarDataStep

## 1. 文件：`APA_radar_exit.yaml`



```yaml
  - thread_name: planning
    # ignore: 1
    print_time: 0
    frame_rate: 20. # Each thread has its own frame rate.  # Use frame_period or frame_rate to control it.
    # frame_period: 0.050. # unit is second.
    thread_procedure:

      - key: ProcessRadarData
        #input_map: grid_map_layer
        output_map: obstacle_map_layer
        output_points: obstacle_layer
        output_side_points: side_obstacle_layer
        sweep_mode: 1
        scan_front: 0
        use_side_radar: 1
        recent_points: 50
        map_resolution: 0.03 #0.2
        map_width: 700 #100
        map_height: 700 #100
        x_offset: -10
        y_offset: -10
```





---



## 2. 文件：`obstacle_operations.h`

```cpp
/**
 * @brief An operation to get radar data from sharedstate and convert to point layer
 */
class ProcessRadarDataStep : public OperationStep
{
public:
    explicit ProcessRadarDataStep(const StepParameter& param);
    virtual void StepSetUp();

protected:
    virtual void Forward_cpu();
    std::string m_output_name;
    std::string m_output_map_name;
    std::string m_side_output_name;
    std::string m_input_map_name;
    std::vector<tauristar::PoseState> m_occupied_points;
    int m_recent_points;
    bool m_sweep_mode;
    double m_map_resolution;
    int m_width, m_height;
    double m_x_offset, m_y_offset;
    int m_prev_read_id;
    boost::shared_ptr<tauristar::LayerWrapper> m_maplayer_ptr;
    tauristar::GeomPose3D m_map_pose;

    bool m_scan_front;
    bool m_use_side_radar;
    tauristar::PoseState m_prev_pose;
    double m_empty_dist;
    double m_dist_factor;
};
```

