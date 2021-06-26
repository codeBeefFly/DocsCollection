# TS PC reproduction

## 20210407

### 1. build TS PC mode

under `..TauristarPlatform/src/tauristar_platform/` directory, target directory `build_debug_0407`

```
$ mkdir build_debug_0407
$ cd build_debug_0407
$ cmake .. -DCMAKE_BUILD_TYPE=Debug -DBUILD_WITH_ROS=OFF 
$ make -j8
```

partial output:

```
[ 98%] Building CXX object CMakeFiles/processor_node.dir/flow_processor/main.cpp.o
[ 98%] Building CXX object CMakeFiles/processor_apa.dir/apa_full_stack/main_tsp.cpp.o
[ 98%] Building CXX object apa_full_stack/CMakeFiles/apa_full_stack_run.dir/param/log_file.cpp.o
[ 98%] Building CXX object apa_full_stack/CMakeFiles/apa_full_stack_run.dir/main.cpp.o
[ 98%] Building CXX object apa_full_stack/CMakeFiles/apa_info_if.dir/ApaInfoProcessIF.cpp.o
[ 98%] Building CXX object apa_full_stack/CMakeFiles/apa_info_if.dir/apa_operations.cpp.o
[100%] Linking CXX static library ../lib/libts_ui_operations.a
[100%] Built target ts_ui_operations
[100%] Linking CXX executable bin/processor_apa
[100%] Linking CXX executable bin/processor_node
[100%] Built target processor_apa
[100%] Built target processor_node
[100%] Linking CXX executable ../bin/apa_full_stack_run
[100%] Built target apa_full_stack_run
[100%] Linking CXX shared library ../lib/libapa_info_if.so
[100%] Built target apa_info_if
```



---



### 2. run processor_node 

1. dvr used: referring to #4056-16 (dvr_0406_0.3_use1_1)



2. if test platform is ic421, make sure `.dat` files are available, better link or copy them under target dvr directory like (dvr_0406_0.3_use1_1). those `.dat` are available from  `../TauristarPlatformData/QA_data/IC421`

    ```
    $ ls
    apa_front_cam.dat  apa_rear_cam.dat   car_param.yaml
    apa_left_cam.dat   apa_right_cam.dat
    ```



3. check processor_node built type:

    ```
    $ file processor_node 
    processor_node: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=6216699d950eac9bedcd84684d4c454fbc276ab7, not stripped
    ```

  

4. do some modifications on `test0f.yaml`, showing result `test0f.yaml` (REDO)

    ```
    #line: 11
    
#line: 27
    
    #line: 74-75

    #line: 79-91
    ```
    
screen cut:
    
    line11
    
    ![image-20210407172847570](/home/ds16/.config/Typora/typora-user-images/image-20210407172847570.png)
    
    line27
    
    ![image-20210407172936121](/home/ds16/.config/Typora/typora-user-images/image-20210407172936121.png)
    
    line74-91
    
    ![image-20210407173107232](/home/ds16/.config/Typora/typora-user-images/image-20210407173107232.png)



5. do some modifications on car_param.yaml

    ```
    #line: 65-69
    coordinate_cam_file:
        left: apa_left_cam.dat
        right: apa_right_cam.dat
        rear: apa_rear_cam.dat
        front: apa_front_cam.dat
    ```
    
    screen cut:
    
    ![image-20210407172745505](/home/ds16/.config/Typora/typora-user-images/image-20210407172745505.png)


---

### 

## 3. how to run use `gdb` to run `processor_node` pc mode

```
# after build a debug binary, in job folder, run command:
gdb ./processor_node

# inside gdb, run command:
run -o conf=test0f.yaml

# if the program crashes, to back track where the error is
bt
```



---



