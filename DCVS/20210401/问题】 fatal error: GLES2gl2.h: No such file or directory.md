# 【问题】 fatal error: GLES2/gl2.h: No such file or directory

## 简述

```
[ 26%] Building CXX object tauristar_bvs/CMakeFiles/bvs.dir/secret/nda_check.cpp.o
In file included from /home/ds16/work/TauristarPlatform/src/tauristar_platform/tauristar_bvs/secret/nda_check.cpp:7:0:
/home/ds16/work/TauristarPlatform/src/tauristar_platform/tauristar_bvs/secret/../bvs_utils.h:18:23: fatal error: GLES2/gl2.h: No such file or directory
compilation terminated.
tauristar_bvs/CMakeFiles/bvs.dir/build.make:62: recipe for target 'tauristar_bvs/CMakeFiles/bvs.dir/secret/nda_check.cpp.o' failed
make[2]: *** [tauristar_bvs/CMakeFiles/bvs.dir/secret/nda_check.cpp.o] Error 1
CMakeFiles/Makefile2:3981: recipe for target 'tauristar_bvs/CMakeFiles/bvs.dir/all' failed
make[1]: *** [tauristar_bvs/CMakeFiles/bvs.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[ 26%] Building CXX object tauristar_motion_planner/CMakeFiles/motion_planner.dir/src/Configuration.cpp.o

```



## 解决
