# 【问题】fatal error: GL/glew.h: No such file or directory

## 简述

```
[ 62%] Building CXX object TauristarUI_Engine/UI_elements/UI_shader/CMakeFiles/ui_shader_lib.dir/shader.cpp.o
In file included from /home/ds16/work/TauristarPlatform/src/tauristar_platform/TauristarUI_Engine/UI_elements/UI_shader/shader.cpp:2:0:
/home/ds16/work/TauristarPlatform/src/tauristar_platform/TauristarUI_Engine/UI_elements/UI_shader/shader.h:12:23: fatal error: GL/glew.h: No such file or directory
   #include <GL/glew.h>
                       ^
compilation terminated.
TauristarUI_Engine/UI_elements/UI_shader/CMakeFiles/ui_shader_lib.dir/build.make:62: recipe for target 'TauristarUI_Engine/UI_elements/UI_shader/CMakeFiles/ui_shader_lib.dir/shader.cpp.o' failed
make[2]: *** [TauristarUI_Engine/UI_elements/UI_shader/CMakeFiles/ui_shader_lib.dir/shader.cpp.o] Error 1
CMakeFiles/Makefile2:4599: recipe for target 'TauristarUI_Engine/UI_elements/UI_shader/CMakeFiles/ui_shader_lib.dir/all' failed
make[1]: *** [TauristarUI_Engine/UI_elements/UI_shader/CMakeFiles/ui_shader_lib.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[ 62%] Building CXX object tauristar_perception/tauristar_image_pipeline/ts_vision_opencv/image_geometry/CMakeFiles/ts_image_geometry.dir/src/pinhole_camera_model.cpp.o

```

