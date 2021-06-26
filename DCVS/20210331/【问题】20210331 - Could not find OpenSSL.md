# 【问题】20210331 - Could not find OpenSSL


## 简述
在 t480s 的双系统 ubuntu-16.04 中 使用 pc 模式编译 desay 的 TauristarPlatform，出现了 openssl 问题

## terminal 输出

```bash
ds16@ds16-TP:~/work/TauristarPlatform/src/tauristar_platform/build$ cmake .. -DBUILD_WITH_ROS=OFF
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- CMAKE_MODULE_PATH: /home/ds16/work/TauristarPlatform/src/tauristar_platform/cmake
-- CMAKE_PREFIX_PATH: /home/ds16/work/TauristarPlatform/src/tauristar_platform/third_party/g2o_9b41a4e/cmake_modules
-- TAURISTAR_SRC_DIR: /home/ds16/work/TauristarPlatform/src/tauristar_platform
-- CMAKE_BINARY_DIR:  /home/ds16/work/TauristarPlatform/src/tauristar_platform/build
-- TAURISTAR_THIRD_PARTY: /home/ds16/work/TauristarPlatform/src/tauristar_platform/third_party
-- TAURISTAR_CROSS_COMPONENTS_DIR: /home/ds16/work/TauristarPlatform/src/tauristar_platform/cross_components
CMake Error at /usr/share/cmake-3.5/Modules/FindPackageHandleStandardArgs.cmake:148 (message):
  Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the
  system variable OPENSSL_ROOT_DIR (missing: OPENSSL_LIBRARIES
  OPENSSL_INCLUDE_DIR)
Call Stack (most recent call first):
  /usr/share/cmake-3.5/Modules/FindPackageHandleStandardArgs.cmake:388 (_FPHSA_FAILURE_MESSAGE)
  /usr/share/cmake-3.5/Modules/FindOpenSSL.cmake:370 (find_package_handle_standard_args)
  CMakeLists.txt:258 (find_package)


-- Configuring incomplete, errors occurred!
See also "/home/ds16/work/TauristarPlatform/src/tauristar_platform/build/CMakeFiles/CMakeOutput.log".
ds16@ds16-TP:~/work/TauristarPlatform/src/tauristar_platform/build$ 

```

## linux 截图方法

**补充，如何在 linux 中截图**
![image-20210331133601591](/home/ds16/.config/Typora/typora-user-images/image-20210331133601591.png)

```
你想要截取整个屏幕？屏幕中的某个区域？某个特定的窗口？

如果只需要获取一张屏幕截图，不对其进行编辑的话，那么键盘的默认快捷键就可以满足要求了。而且不仅仅是 Ubuntu ，绝大部分的 Linux 发行版和桌面环境都支持以下这些快捷键：

* PrtSc – 获取整个屏幕的截图并保存到 Pictures 目录。
* Shift + PrtSc – 获取屏幕的某个区域截图并保存到 Pictures 目录。
* Alt + PrtSc –获取当前窗口的截图并保存到 Pictures 目录。
* Ctrl + PrtSc – 获取整个屏幕的截图并存放到剪贴板。
* Shift + Ctrl + PrtSc – 获取屏幕的某个区域截图并存放到剪贴板。
* Ctrl + Alt + PrtSc – 获取当前窗口的 截图并存放到剪贴板。

如上所述，在 Linux 中使用默认的快捷键获取屏幕截图是相当简单的。但如果要在不把屏幕截图导入到其它应用程序的情况下对屏幕截图进行编辑，还是使用屏幕截图工具比较方便。
```

**问题截图**

![image-20210331133914238](/home/ds16/.config/Typora/typora-user-images/image-20210331133914238.png)



## 尝试解决-已解决

使用命令

```
sudo apt-get install libssl-dev
```

链接：CMake not able to find OpenSSL library -- stackoverflow