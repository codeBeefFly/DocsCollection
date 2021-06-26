# something about python2, python3 and their staffs.

## 1. 查看 python 安装路径

```
ds16@ds16-TP:~/work/TauristarPlatform$ which python
/usr/bin/python

ds16@ds16-TP:~/work/TauristarPlatform$ which python3
/usr/bin/python3
```



## 2. 查看 python 版本

```
ds16@ds16-TP:~/work/TauristarPlatform$ python --version
Python 2.7.12

ds16@ds16-TP:~/work/TauristarPlatform$ python3 --version
Python 3.5.2
```



## 3. 升级 python3

## 问题：`You are using pip version 8.1.1, however version 21.0.1 is available.`

```
ds16@ds16-TP:~/work/TauristarPlatform$ pip3 install matplotlib -i https://mirrors.aliyun.com/pypi/simple/
Collecting matplotlib
  Downloading https://mirrors.aliyun.com/pypi/packages/84/61/28711c7773a3a47c7f798cafc219968aab78d260c0d674696a077432bbd4/matplotlib-3.4.1.tar.gz (37.3MB)
    100% |████████████████████████████████| 37.3MB 25kB/s 
    Complete output from command python setup.py egg_info:
    
    Beginning with Matplotlib 3.4, Python 3.7 or above is required.
    You are using Python 3.5.2.
    
    This may be due to an out of date pip.
    
    Make sure you have pip >= 9.0.1.
    
    
    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-meajolmv/matplotlib/
You are using pip version 8.1.1, however version 21.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
ds16@ds16-TP:~/work/TauristarPlatform$ pip3 --version
pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)

```

## 原因：