# 独立编译安装 Python3.x 记录

[toc]



---

## 说明：

独立编译安装 Python3.x，可以在不改变当前 linux 系统自带 Python 版本的情况下，自定义 Python Interpretor 以及 开发环境（方便创建虚拟环境）



触发因素：

1. 不方便改动 当前 linux 系统自带 Python 版本
2. 新建的 Python 虚拟环境中，pyinstaller 遇到问题，追溯到 Python 编译安装编译问题



---

## 问题1：OSError: Python library not found: libpython3.7m.so.1.0, libpython3.7.so.1.0, libpython3.7mu.so.1.0, libpyt

```
17153 INFO: Looking for ctypes DLLs
17246 WARNING: library user32 required via ctypes not found
17259 INFO: Analyzing run-time hooks ...
17266 INFO: Including run-time hook '/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/hooks/rthooks/pyi_rth__tkinter.py'
17267 INFO: Including run-time hook '/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/hooks/rthooks/pyi_rth_mplconfig.py'
17274 INFO: Looking for dynamic libraries
18607 INFO: Looking for eggs
18607 INFO: Python library not in binary dependencies. Doing additional searching...
Traceback (most recent call last):
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/bin/pyinstaller", line 8, in <module>
    sys.exit(run())
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/__main__.py", line 114, in run
    run_build(pyi_config, spec_file, **vars(args))
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/__main__.py", line 65, in run_build
    PyInstaller.building.build_main.main(pyi_config, spec_file, **kwargs)
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/building/build_main.py", line 737, in main
    build(specfile, kw.get('distpath'), kw.get('workpath'), kw.get('clean_build'))
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/building/build_main.py", line 684, in build
    exec(code, spec_namespace)
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/apa_graph_tool/apa_graph_tool_v1.4.spec", line 18, in <module>
    noarchive=False)
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/building/build_main.py", line 242, in __init__
    self.__postinit__()
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/building/datastruct.py", line 160, in __postinit__
    self.assemble()
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/building/build_main.py", line 476, in assemble
    self._check_python_library(self.binaries)
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/building/build_main.py", line 581, in _check_python_library
    python_lib = bindepend.get_python_library_path()
  File "/home/ds16v2/PythonEnvs/GroundFifthPy/lib/python3.7/site-packages/PyInstaller/depend/bindepend.py", line 956, in get_python_library_path
    raise IOError(msg)
OSError: Python library not found: libpython3.7m.so.1.0, libpython3.7.so.1.0, libpython3.7mu.so.1.0, libpython3.7m.so
    This would mean your Python installation doesn't come with proper library files.
    This usually happens by missing development package, or unsuitable build parameters of Python installation.

    * On Debian/Ubuntu, you would need to install Python development packages
      * apt-get install python3-dev
      * apt-get install python-dev
    * If you're building Python by yourself, please rebuild your Python with `--enable-shared` (or, `--enable-framework` on Darwin)
    
(GroundFifthPy) ds16v2@ds16v2:~/PythonEnvs/GroundFifthPy/apa_graph_tool$ 

```

![image-20210429132527804](/home/ds16v2/.config/Typora/typora-user-images/image-20210429132527804.png)



---

## 尝试解决

### 尝试1

```
rm -rf Python-3.7.10
tar -xzvf Python-3.7.10.tgz 
cd Python-3.7.10

./configure --prefix=/usr/local/python3.7.10 --enable-optimizations --with-openssl=/usr/local/openssl --enable-shared

make -j 8
make install 或 make altinstall

————————————————
版权声明：本文为CSDN博主「帅气的Antony」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_37742059/article/details/103026800
```



> The `enable-optimizations` flag will enable some  optimizations within Python to make it run about 10 percent faster.  Doing this may add twenty or thirty minutes to the compilation time. 

> The `with-ensurepip=install` flag will install `pip` bundled with this installation.



执行 `make altinstall` 时，结果：

```
make install 或 make altinstall


Creating directory /usr/local/python-3.7.10/share/man
Creating directory /usr/local/python-3.7.10/share/man/man1
/usr/bin/install -c -m 644 ./Misc/python.man \
        /usr/local/python-3.7.10/share/man/man1/python3.7.1
if test "xupgrade" != "xno"  ; then \
        case upgrade in \
                upgrade) ensurepip="--altinstall --upgrade" ;; \
                install|*) ensurepip="--altinstall" ;; \
        esac; \
        LD_LIBRARY_PATH=/home/ds16v2/Software/Python-3.7.10 ./python -E -m ensurepip \
                $ensurepip --root=/ ; \
fi
Looking in links: /tmp/tmprzixbmcn
Processing /tmp/tmprzixbmcn/setuptools-47.1.0-py3-none-any.whl
Processing /tmp/tmprzixbmcn/pip-20.1.1-py2.py3-none-any.whl
Installing collected packages: setuptools, pip
  WARNING: The script easy_install-3.7 is installed in '/usr/local/python-3.7.10/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
  WARNING: The script pip3.7 is installed in '/usr/local/python-3.7.10/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
Successfully installed pip-20.1.1 setuptools-47.1.0
ds16v2@ds16v2:~/Software/Python-3.7.10$ 

```



问题2： 创建 python 虚拟环境 GroundSixthPy 时出现的问题

![image-20210429143318934](/home/ds16v2/.config/Typora/typora-user-images/image-20210429143318934.png)

没有搜索到所需要的 `.so`，因为没有将 python3.7.10 写入环境变量（独立安装，不写环境变量，以免破坏原生 python 的完整性）





## 问题2： libpython3.7.m.so.1.0: cannot open shared object file: No such file or directory (见上图)

1. libpython3.7.m.so.1.0 动态库所在路径

   ```
   ds16v2@ds16v2:/usr/local/python-3.7.10/lib$ ls
   libpython3.7m.so  libpython3.7m.so.1.0  libpython3.so  pkgconfig  python3.7
   ds16v2@ds16v2:/usr/local/python-3.7.10/lib$ 
   
   ```




### 解决：

```
# 使用 echo 向文件写入内容，'>' 写入，“>>” 追加
echo "/usr/local/python-3.7.10/lib/" >> /etc/ld.so.conf

# 
ldconfig

```



>**ldconfig: **主要是在默认搜寻目录`/lib`和`/usr/lib`以及动态库配置文件`/etc/ld.so.conf`内所列的目录下，搜索出可共享的动态链接库（格式如lib*.so*）,进而创建出动态装入程序(ld.so)所需的连接和缓存文件，
>
>
>
>缓存文件默认为`/etc/ld.so.cache`，此文件保存已排好序的动态链接库名字列表。linux下的共享库机制采用了类似高速缓存机制，将库信息保存在`/etc/ld.so.cache`，程序连接的时候首先从这个文件里查找，然后再到`ld.so.conf`的路径中查找。为了让动态链接库为系统所共享，需运行动态链接库的管理命令`ldconfig`，此执行程序存放在/sbin目录下
>
>————————————————
>版权声明：本文为CSDN博主「winycg」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
>原文链接：https://blog.csdn.net/winycg/article/details/80572735



验证结果：

```
(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs$ pip --version
pip 21.1 from /home/ds16v2/PythonEnvs/GroundSixthPy/lib/python3.7/site-packages/pip (python 3.7)

(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs$ python --version
Python 3.7.10


################## install pyinstaller

(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs$ pip install pyinstaller
Collecting pyinstaller
  Using cached pyinstaller-4.3-py3-none-any.whl
Collecting importlib-metadata
  Using cached importlib_metadata-4.0.1-py3-none-any.whl (16 kB)
Requirement already satisfied: setuptools in ./GroundSixthPy/lib/python3.7/site-packages (from pyinstaller) (56.0.0)
Collecting pyinstaller-hooks-contrib>=2020.6
  Using cached pyinstaller_hooks_contrib-2021.1-py2.py3-none-any.whl (181 kB)
Collecting altgraph
  Using cached altgraph-0.17-py2.py3-none-any.whl (21 kB)
Collecting zipp>=0.5
  Using cached zipp-3.4.1-py3-none-any.whl (5.2 kB)
Collecting typing-extensions>=3.6.4
  Using cached typing_extensions-3.7.4.3-py3-none-any.whl (22 kB)
Installing collected packages: zipp, typing-extensions, pyinstaller-hooks-contrib, importlib-metadata, altgraph, pyinstaller
Successfully installed altgraph-0.17 importlib-metadata-4.0.1 pyinstaller-4.3 pyinstaller-hooks-contrib-2021.1 typing-extensions-3.7.4.3 zipp-3.4.1

################## install numpy

(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs$ pip install numpy
Collecting numpy
  Using cached numpy-1.20.2-cp37-cp37m-manylinux2010_x86_64.whl (15.3 MB)
Installing collected packages: numpy
Successfully installed numpy-1.20.2


################## install matplotlib

(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs$ pip install matplotlib
Collecting matplotlib
  Using cached matplotlib-3.4.1-cp37-cp37m-manylinux1_x86_64.whl (10.3 MB)
Collecting python-dateutil>=2.7
  Using cached python_dateutil-2.8.1-py2.py3-none-any.whl (227 kB)
Collecting pillow>=6.2.0
  Using cached Pillow-8.2.0-cp37-cp37m-manylinux1_x86_64.whl (3.0 MB)
Requirement already satisfied: numpy>=1.16 in ./GroundSixthPy/lib/python3.7/site-packages (from matplotlib) (1.20.2)
Collecting pyparsing>=2.2.1
  Using cached pyparsing-2.4.7-py2.py3-none-any.whl (67 kB)
Collecting cycler>=0.10
  Using cached cycler-0.10.0-py2.py3-none-any.whl (6.5 kB)
Collecting kiwisolver>=1.0.1
  Using cached kiwisolver-1.3.1-cp37-cp37m-manylinux1_x86_64.whl (1.1 MB)
Collecting six
  Using cached six-1.15.0-py2.py3-none-any.whl (10 kB)
Installing collected packages: six, python-dateutil, pyparsing, pillow, kiwisolver, cycler, matplotlib
Successfully installed cycler-0.10.0 kiwisolver-1.3.1 matplotlib-3.4.1 pillow-8.2.0 pyparsing-2.4.7 python-dateutil-2.8.1 six-1.15.0
(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs$ 

```



执行 `pyinstaller` 命令，打包成功

```
$ pyinstaller -F apa_graph_tool_v1.4.py -p common_log.py -p common_util.py -p pack_python_plot_multistep.py -p pack_python_plot_onestep.py

...
...
...

20922 INFO: Building PKG (CArchive) PKG-00.pkg
38841 INFO: Building PKG (CArchive) PKG-00.pkg completed successfully.
38858 INFO: Bootloader /home/ds16v2/PythonEnvs/GroundSixthPy/lib/python3.7/site-packages/PyInstaller/bootloader/Linux-64bit/run
38858 INFO: checking EXE
38859 INFO: Building EXE because EXE-00.toc is non existent
38859 INFO: Building EXE from EXE-00.toc
38859 INFO: Appending archive to ELF section in EXE /home/ds16v2/PythonEnvs/GroundSixthPy/apa_graph_tool/dist/apa_graph_tool_v1.4
38936 INFO: Building EXE from EXE-00.toc completed successfully.

(GroundSixthPy) ds16v2@ds16v2:~/PythonEnvs/GroundSixthPy/apa_graph_tool$ 

```

![image-20210429150405757](/home/ds16v2/.config/Typora/typora-user-images/image-20210429150405757.png)



---

## 参考

链接: [pyinstaller 出现 OSError: Python library not found](https://blog.csdn.net/qq_37742059/article/details/103026800)

链接: [Python 3 Installation & Setup Guide](https://realpython.com/installing-python/)

链接: [linux ldconfig命令,环境变量文件配置详解](https://blog.csdn.net/winycg/article/details/80572735)

