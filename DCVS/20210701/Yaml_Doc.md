# 5390-Yaml-Doc

[toc]

---

motto: DDL is a primary productive force!



---

## PC SIM

```yaml
      - key: APAReadTxt                            # mark doc.
        ignore: 1  #DVR                            # mark doc.
        input: H002_DVR/DVR_2/left.txt             # mark doc.
        output: txt_str_layer       
        accurate_time: 1      

      - key: APAInfoProcess                        # mark doc.
        ignore: 1  # DVR                           # mark doc.
        coordinate_cam_file:
          left: H002_DVR/apa_left_cam.dat          # mark doc.
          right: H002_DVR/apa_right_cam.dat        # mark doc.
          rear: H002_DVR/apa_rear_cam.dat          # mark doc.
          front: H002_DVR/apa_front_cam.dat        # mark doc.
        car_param: H002_DVR/car_param.yaml         # mark doc.
        video_file: 
          left_h264: H002_DVR/DVR_2/left.h264      # mark doc.
          left_hdr: H002_DVR/DVR_2/left.hdr        # mark doc.
          right_h264: H002_DVR/DVR_2/right.h264    # mark doc.
          right_hdr: H002_DVR/DVR_2/right.hdr      # mark doc.
        #input_cam: [left_cam, right_cam]
        input_txt: txt_str_layer
        pub_pose: 1
        show_image: 1
        output_resolution: 0.2
        output: grid_map_layer
        enable_tsp: 1
```







---

## 4959 vertical parking









---

## 4459 parallel parking

