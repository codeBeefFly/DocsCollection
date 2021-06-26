# linux staff -- how to fast set up a preferred workable ubuntu environment



[toc]

---



this document records general steps for linux basic personal environment setups whenever and wherever you want to quickly install a ubuntu-linux OS and development env. with following requirements



---



## 01. (root) fast install and setup ready for project development ()()

   

---



## 02. with Chinese language input method (done)(done)

​	这个是中文测试   



---



## 03. with pycharm and clion (done)(done)

### pycharm

check python interpretor: 

![image-20210417180221964](/home/ds16v2/.config/Typora/typora-user-images/image-20210417180221964.png)

### clion

![image-20210419094401676](/home/ds16v2/.config/Typora/typora-user-images/image-20210419094401676.png)

   ### create pycharm or clion desktop entry

> 1. enter IDE
> 2. tools
> 3. create desktop entry



---



## 04. with redmine setup ready (done)(done)

   ```
jin.li@aitronx.com
jin.li
lijin911
   ```



---



## 05. with gitlab setup ready (add secure key and 如何通过拷贝的仓库建立远程链接<仓库中包含软连接>) (done)(done)

1. China repository

   ```
   http://120.78.195.113/
   lijingitlab911
   ```

   

2. US repository

   ```
   http://35.247.3.214/tauristar/TauristarPlatformSimulation
   lijingl911
   ```



3. add ssh key

   注：如果使用了 `sudo`或者是在`root`用户下生成的 `ssh-key`，系统会生成 一个文件路径 `Created directory '/root/.ssh'.`，这样不好，尽量在用户模式下生成 `ssh-key`，此模式下 `ssh-key`的默认目录在 `/home/[usr]/.ssh`，对应的 `id_rsa, id_rsa.pub` 都在这个目录下，所以在做修改时要参考这个目录。
   
   ```
   root@ds16v2:/home/ds16v2/Software/pycharm-community-2020.3.3/bin# cd
   root@ds16v2:~# cd ~/.ssh
   bash: cd: /root/.ssh: No such file or directory
   
   # 需要按三次空格
   root@ds16v2:~# ssh-keygen -t rsa -C "jin.li@aitronx.com"
   Generating public/private rsa key pair.
   Enter file in which to save the key (/root/.ssh/id_rsa): 
   Created directory '/root/.ssh'.
   Enter passphrase (empty for no passphrase): 
   Enter same passphrase again: 
   Your identification has been saved in /root/.ssh/id_rsa.
   Your public key has been saved in /root/.ssh/id_rsa.pub.
   The key fingerprint is:
   SHA256:VJ2akpAw4ja2CpaBr4Y2wbnb4POQ1HOKKA2BcUITAIA jin.li@aitronx.com
   The key's randomart image is:
   +---[RSA 2048]----+
   |%++ o. .  .. .   |
   |E=.. .o  .  o    |
   |+.=    ... o     |
   |.==o   .o o      |
   |oBoo .  S.       |
   |*== +            |
   |*X..             |
   |=.*              |
   | ooo             |
   +----[SHA256]-----+
   
   root@ds16v2:~# cat ~/.ssh/id_rsa.pub
   ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDa0Fh0soZBhVfSTBwAYjLZsHmUfkujGxJ97AED0oK8Asx3uEsLvndOtcuQH7YGoQfJi+BLoa7isLLFYCbDt4ygTQEqQ0UMSnR
   
   root@ds16v2:~# ls -a ~/.ssh
   .  ..  id_rsa  id_rsa.pub  known_hosts
   
   ```
   
   注：因为是在 root 下生成的 ssh，git clone 的时候需要切换到 root 用户。



### 问题：使用 sudo 命令生成 ssh key，只能使用 root 用户 git clone

找到方法使之在不重新使用‘’用户‘’模式生成 ssh key 的情况下, 可以在 用户模式 git clone 远程仓库

1. problem reproduce: 见上，是在 root 用户中生成的 `.ssh`

2. 查看 `.ssh`, 发现不同用户中的文件内容是不同的

   ```
   ds16v2@ds16v2:~$ ls -a ~/.ssh
   .  ..  known_hosts
   
   ds16v2@ds16v2:~$ sudo su
   [sudo] password for ds16v2: 
   root@ds16v2:/home/ds16v2# ls -a ~/.ssh
   .  ..  id_rsa  id_rsa.pub  known_hosts
   
   ```

3. 查看 `.ssh` 中文件详细信息，可以看到因为是在 `root` 下生成的 `.ssh`，所以权限都是 `root`，路径在 `~/.ssh`

   ```
   root@ds16v2:/home/ds16v2# ls -l ~/.ssh
   total 12
   -rw------- 1 root root 1675 4月  19 10:44 id_rsa
   -rw-r--r-- 1 root root  400 4月  19 10:44 id_rsa.pub
   -rw-r--r-- 1 root root  666 4月  19 11:19 known_hosts
   
   ```

4. 需要将 `~/.ssh` 文件夹拷贝到 `home/[usr]/`下

   ```
   # 1. 拷贝 .ssh 到 /home/ds16v2/ 下
   root@ds16v2:~# cp -r .ssh /home/ds16v2/
   
   # 2. 查看 /home/ds16v2/.ssh 下的文件
   root@ds16v2:/home/ds16v2/.ssh# ls -al
   total 20
   drwx------  2 ds16v2 ds16v2 4096 4月  19 13:03 .
   drwxr-xr-x 31 ds16v2 ds16v2 4096 4月  19 10:56 ..
   -rw-------  1 root   root   1675 4月  19 13:03 id_rsa
   -rw-r--r--  1 root   root    400 4月  19 13:03 id_rsa.pub
   -rw-r--r--  1 ds16v2 ds16v2  666 4月  19 13:03 known_hosts
   
   # 3. 因为一些原因，没有将 user:group 改完全，需要将 id_rsa, id_rsa.pub 改成 ds16v2:ds16v2
   root@ds16v2:/home/ds16v2/.ssh# chown ds16v2:ds16v2 id_rsa id_rsa.pub
   
   # 4. 继续查看
   root@ds16v2:/home/ds16v2/.ssh# ll -al
   total 20
   drwx------  2 ds16v2 ds16v2 4096 4月  19 13:03 ./
   drwxr-xr-x 31 ds16v2 ds16v2 4096 4月  19 10:56 ../
   -rw-------  1 ds16v2 ds16v2 1675 4月  19 13:03 id_rsa
   -rw-r--r--  1 ds16v2 ds16v2  400 4月  19 13:03 id_rsa.pub
   -rw-r--r--  1 ds16v2 ds16v2  666 4月  19 13:03 known_hosts
   
   # 对比 2 现在可以看到 id_rsa, id_rsa.pub 文件了
   root@ds16v2:/home/ds16v2/.ssh# ls -a ~/.ssh
   .  ..  id_rsa  id_rsa.pub  known_hosts
   
   ```

   

### 小技巧：可以换系统后可以不用再 git clone，可以直接拷贝已有的 本地仓库，然后 git status + git pull，前提是此系统已经关联了远程仓库：

比如： TauristarPlatformSimulation 和 TDA2XBSP，直接从 Ubuntu-ds16 系统中拷贝，然后做 git 操作

![image-20210419141218467](/home/ds16v2/.config/Typora/typora-user-images/image-20210419141218467.png)

![image-20210419141331403](/home/ds16v2/.config/Typora/typora-user-images/image-20210419141331403.png)





---



## 06. (VIP) with python pip installed and upgrade ready (done)(done) -- link to 14

   how to upgrade python3 in ubuntu 16.04.

   how to create a python vertual environment.  (done)(new)

   ```
   ds16v2@ds16v2:~$ pip3 -V
   pip 8.1.1 from /usr/lib/python3/dist-packages (python 3.5)
   ds16v2@ds16v2:~$ python3
   Python 3.5.2 (default, Jan 26 2021, 13:30:48) 
   [GCC 5.4.0 20160609] on linux
   Type "help", "copyright", "credits" or "license" for more information.
   >>> import numpy
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   ImportError: No module named 'numpy'
   >>> exit()
   ds16v2@ds16v2:~$ pip3 install numpy -i https://pypi.douban.com/simple/
   Collecting numpy
     Downloading https://pypi.doubanio.com/packages/82/a8/1e0f86ae3f13f7ce260e9f782764c16559917f24382c74edfb52149897de/numpy-1.20.2.zip (7.8MB)
       100% |████████████████████████████████| 7.8MB 218kB/s 
       Complete output from command python setup.py egg_info:
       Traceback (most recent call last):
         File "<string>", line 1, in <module>
         File "/tmp/pip-build-q8v0ukuj/numpy/setup.py", line 30, in <module>
           raise RuntimeError("Python version >= 3.7 required.")
       RuntimeError: Python version >= 3.7 required.
       
       ----------------------------------------
   Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-q8v0ukuj/numpy/
   You are using pip version 8.1.1, however version 21.0.1 is available.
   You should consider upgrading via the 'pip install --upgrade pip' command.
   ds16v2@ds16v2:~$ pip3 install --upgrade pip
   Collecting pip
     Downloading https://files.pythonhosted.org/packages/fe/ef/60d7ba03b5c442309ef42e7d69959f73aacccd0d86008362a681c4698e83/pip-21.0.1-py3-none-any.whl (1.5MB)
       51% |████████████████▋               | 798kB 2.8kB/s eta 0:04:28Exception:
   Traceback (most recent call last):
     File "/usr/share/python-wheels/urllib3-1.13.1-py2.py3-none-any.whl/urllib3/response.py", line 226, in _error_catcher
       yield
     File "/usr/share/python-wheels/urllib3-1.13.1-py2.py3-none-any.whl/urllib3/response.py", line 301, in read
       data = self._fp.read(amt)
     File "/usr/share/python-wheels/CacheControl-0.11.5-py2.py3-none-any.whl/cachecontrol/filewrapper.py", line 49, in read
       data = self.__fp.read(amt)
     File "/usr/lib/python3.5/http/client.py", line 462, in read
       n = self.readinto(b)
     File "/usr/lib/python3.5/http/client.py", line 502, in readinto
       n = self.fp.readinto(b)
     File "/usr/lib/python3.5/socket.py", line 575, in readinto
       return self._sock.recv_into(b)
     File "/usr/lib/python3.5/ssl.py", line 929, in recv_into
       return self.read(nbytes, buffer)
     File "/usr/lib/python3.5/ssl.py", line 791, in read
       return self._sslobj.read(len, buffer)
     File "/usr/lib/python3.5/ssl.py", line 575, in read
       v = self._sslobj.read(len, buffer)
   socket.timeout: The read operation timed out
   
   During handling of the above exception, another exception occurred:
   
   Traceback (most recent call last):
     File "/usr/lib/python3/dist-packages/pip/basecommand.py", line 209, in main
       status = self.run(options, args)
     File "/usr/lib/python3/dist-packages/pip/commands/install.py", line 328, in run
       wb.build(autobuilding=True)
     File "/usr/lib/python3/dist-packages/pip/wheel.py", line 748, in build
       self.requirement_set.prepare_files(self.finder)
     File "/usr/lib/python3/dist-packages/pip/req/req_set.py", line 360, in prepare_files
       ignore_dependencies=self.ignore_dependencies))
     File "/usr/lib/python3/dist-packages/pip/req/req_set.py", line 577, in _prepare_file
       session=self.session, hashes=hashes)
     File "/usr/lib/python3/dist-packages/pip/download.py", line 810, in unpack_url
       hashes=hashes
     File "/usr/lib/python3/dist-packages/pip/download.py", line 649, in unpack_http_url
       hashes)
     File "/usr/lib/python3/dist-packages/pip/download.py", line 871, in _download_http_url
       _download_url(resp, link, content_file, hashes)
     File "/usr/lib/python3/dist-packages/pip/download.py", line 595, in _download_url
       hashes.check_against_chunks(downloaded_chunks)
     File "/usr/lib/python3/dist-packages/pip/utils/hashes.py", line 46, in check_against_chunks
       for chunk in chunks:
     File "/usr/lib/python3/dist-packages/pip/download.py", line 563, in written_chunks
       for chunk in chunks:
     File "/usr/lib/python3/dist-packages/pip/utils/ui.py", line 139, in iter
       for x in it:
     File "/usr/lib/python3/dist-packages/pip/download.py", line 552, in resp_read
       decode_content=False):
     File "/usr/share/python-wheels/urllib3-1.13.1-py2.py3-none-any.whl/urllib3/response.py", line 344, in stream
       data = self.read(amt=amt, decode_content=decode_content)
     File "/usr/share/python-wheels/urllib3-1.13.1-py2.py3-none-any.whl/urllib3/response.py", line 311, in read
       flush_decoder = True
     File "/usr/lib/python3.5/contextlib.py", line 77, in __exit__
       self.gen.throw(type, value, traceback)
     File "/usr/share/python-wheels/urllib3-1.13.1-py2.py3-none-any.whl/urllib3/response.py", line 231, in _error_catcher
       raise ReadTimeoutError(self._pool, None, 'Read timed out.')
   requests.packages.urllib3.exceptions.ReadTimeoutError: HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Read timed out.
   You are using pip version 8.1.1, however version 21.0.1 is available.
   You should consider upgrading via the 'pip install --upgrade pip' command.
   ds16v2@ds16v2:~$ 
   
   ```

​    





---

  


## 07. setup preference for terminator (done)(done)

   use the original default preference for terminator. (done)



---



## 08. setup ready for pycharm and clion Chinese input environment (done)(done)

in ../bin/clion.sh or ../bin/pycharm.sh

add:

```
# ---------------------------------------------------------------------
# Configure fcitx.
# ---------------------------------------------------------------------
export XMODIFIERS="@im=fcitx"
export GTK_IM_MOUDLE="fcitx"
export QT_IM_MODULE="fcitx"
 
```



   

---



## 09. install nvidia driver (done)(done)

   ```
   ds16v2@ds16v2:~$ nvidia-smi
   Fri Apr 16 21:01:04 2021       
   +-----------------------------------------------------------------------------+
   | NVIDIA-SMI 430.64       Driver Version: 430.64       CUDA Version: 10.1     |
   |-------------------------------+----------------------+----------------------+
   | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
   | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
   |===============================+======================+======================|
   |   0  GeForce MX150       Off  | 00000000:01:00.0 Off |                  N/A |
   | N/A   53C    P0    N/A /  N/A |    201MiB /  2002MiB |      0%      Default |
   +-------------------------------+----------------------+----------------------+
                                                                                  
   +-----------------------------------------------------------------------------+
   | Processes:                                                       GPU Memory |
   |  GPU       PID   Type   Process name                             Usage      |
   |=============================================================================|
   |    0      1102      G   /usr/lib/xorg/Xorg                           161MiB |
   |    0      1974      G   compiz                                        39MiB |
   +-----------------------------------------------------------------------------+
   ds16v2@ds16v2:~$ 
   ds16v2@ds16v2:~$ nvidia-settings 
   
   (nvidia-settings:3832): GLib-GObject-CRITICAL **: g_object_unref: assertion 'G_IS_OBJECT (object)' failed
   GPU at BusId 0x1 doesn't have a supported video decoder
   ** Message: PRIME: Requires offloading
   ** Message: PRIME: is it supported? yes
   
   ```

   

   ![image-20210416211724818](/home/ds16v2/.config/Typora/typora-user-images/image-20210416211724818.png)

   ![image-20210416214047518](/home/ds16v2/.config/Typora/typora-user-images/image-20210416214047518.png)

   ![image-20210416215051548](/home/ds16v2/.config/Typora/typora-user-images/image-20210416215051548.png)



---




## 10. grub name and booting sequence setup (done)(done)

```
#ubuntu 和 windows 启动项名称，顺序修改
sudo gedit /boot/grub/grub.cfg

menuentry 'Ubuntu-ds16v2' ...
```



---



## 11. ubuntu use screen-cut shortcut (done)(done)

    link:[Best Tools For Taking and Editing Screenshots in Linux](https://itsfoss.com/take-screenshot-linux/)
    
    **PrtSc** – *Save a screenshot of the entire screen to the “Pictures” directory.*
    **Shift + PrtSc** – *Save a screenshot of a specific region to Pictures.*
    **Alt + PrtSc** – *Save a screenshot of the current window to Pictures*.
    **Ctrl + PrtSc** – *Copy the screenshot of the entire screen to the clipboard.*
    **Shift + Ctrl + PrtSc** – *Copy the screenshot of a specific region to the clipboard.*
    **Ctrl + Alt + PrtSc** – *Copy the screenshot of the current window to the clipboard.*



---



## 12. install typora on linux (done)(done)

    link:[Install Typora on Linux](https://support.typora.io/Typora-on-Linux/)



---



## 13. update g++ and gcc (done)(not-done)

    we have to specify the gcc version to use a new version of gcc or g++ such as using g++-7 or gcc-7
    
    ```
    ds16v2@ds16v2:~$ gcc --version
    gcc (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609
    Copyright (C) 2015 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    
    ds16v2@ds16v2:~$ gcc-7 --version
    gcc-7 (Ubuntu 7.5.0-3ubuntu1~16.04) 7.5.0
    Copyright (C) 2017 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    
    ds16v2@ds16v2:~$ g++ --version
    g++ (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609
    Copyright (C) 2015 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    
    ds16v2@ds16v2:~$ g++-7 --version
    g++-7 (Ubuntu 7.5.0-3ubuntu1~16.04) 7.5.0
    Copyright (C) 2017 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    
    ```

​    

---



## 14. python virtual environment (done)(done)

why learn this:

1. ROS OS environment has conflict with Anaconda (no anaconda is allowed)

2. try not to damage ubuntu original packed python-2.7 and python-3.5

3. problem appeared

   ```
   root@ds16v2:/usr/bin# pip
   Traceback (most recent call last):
     File "/usr/local/bin/pip", line 7, in <module>
       from pip._internal.cli.main import main
     File "/usr/local/lib/python2.7/dist-packages/pip/_internal/cli/main.py", line 60
       sys.stderr.write(f"ERROR: {exc}")
                                      ^
   SyntaxError: invalid syntax
   root@ds16v2:/usr/bin# 
   ```

4. introduced issue:

   ```
   ds16v2@ds16v2:~$ pip3 install virtualenv
   Collecting virtualenv
     Downloading https://files.pythonhosted.org/packages/91/fb/ca6c071f4231e06a9f0c3bd81c15c233bbacd4a7d9dbb7438d95fece8a1e/virtualenv-20.4.3-py2.py3-none-any.whl (7.2MB)
       100% |████████████████████████████████| 7.2MB 83kB/s 
   Collecting filelock<4,>=3.0.0 (from virtualenv)
     Downloading https://files.pythonhosted.org/packages/93/83/71a2ee6158bb9f39a90c0dea1637f81d5eef866e188e1971a1b1ab01a35a/filelock-3.0.12-py3-none-any.whl
   Collecting six<2,>=1.9.0 (from virtualenv)
     Downloading https://files.pythonhosted.org/packages/ee/ff/48bde5c0f013094d729fe4b0316ba2a24774b3ff1c52d924a8a4cb04078a/six-1.15.0-py2.py3-none-any.whl
   Collecting importlib-metadata>=0.12; python_version < "3.8" (from virtualenv)
     Downloading https://files.pythonhosted.org/packages/52/d0/bdb31463f2d9ca111e39b268518e9baa3542ef73ca449b711a7b4da69764/importlib_metadata-3.10.1-py3-none-any.whl
   Collecting distlib<1,>=0.3.1 (from virtualenv)
     Downloading https://files.pythonhosted.org/packages/f5/0a/490fa011d699bb5a5f3a0cf57de82237f52a6db9d40f33c53b2736c9a1f9/distlib-0.3.1-py2.py3-none-any.whl (335kB)
       100% |████████████████████████████████| 337kB 99kB/s 
   Collecting importlib-resources>=1.0; python_version < "3.7" (from virtualenv)
     Downloading https://files.pythonhosted.org/packages/f0/5e/69e6a0602c1f18d390952177de648468c4a380252858b0022affc3ce7811/importlib_resources-5.1.2-py3-none-any.whl
   Collecting appdirs<2,>=1.4.3 (from virtualenv)
     Downloading https://files.pythonhosted.org/packages/3b/00/2344469e2084fb287c2e0b57b72910309874c3245463acd6cf5e3db69324/appdirs-1.4.4-py2.py3-none-any.whl
   Collecting typing-extensions>=3.6.4; python_version < "3.8" (from importlib-metadata>=0.12; python_version < "3.8"->virtualenv)
     Downloading https://files.pythonhosted.org/packages/60/7a/e881b5abb54db0e6e671ab088d079c57ce54e8a01a3ca443f561ccadb37e/typing_extensions-3.7.4.3-py3-none-any.whl
   Collecting zipp>=0.5 (from importlib-metadata>=0.12; python_version < "3.8"->virtualenv)
     Downloading https://files.pythonhosted.org/packages/0f/8c/715c54e9e34c0c4820f616a913a7de3337d0cd79074dd1bed4dd840f16ae/zipp-3.4.1-py3-none-any.whl
   Installing collected packages: filelock, six, typing-extensions, zipp, importlib-metadata, distlib, importlib-resources, appdirs, virtualenv
   Successfully installed appdirs distlib filelock importlib-metadata importlib-resources six-1.10.0 typing-extensions virtualenv zipp
   You are using pip version 8.1.1, however version 21.0.1 is available.
   You should consider upgrading via the 'pip install --upgrade pip' command.
   
   ```

   from the issue appeared above, we need have to 

   ~~first, install `pip/pip3`~~ (no, we did not need to first install 'pip or pip3')

   ```
   You are using pip version 8.1.1, however version 21.0.1 is available.
   You should consider upgrading via the 'pip install --upgrade pip' command.
   ```

   > update for first: there's no need to first install pip/pip3. to create a virtual environment, we can use command either
   >
   > ```
   > python3 -m pip install --user virtualenv
   > ```
   >
   > or
   >
   > to install python env through command
   >
   > ```
   > ds16v2@ds16v2:~$ sudo apt-get install python3-venv
   > ds16v2@ds16v2:~$ python3 -m venv /home/ds16v2/PythonEnvs/GroundZeroPy
   > 
   > 
   > ```
   >
   > we can use the following command to activate virtual environment:
   >
   > ```
   > (GroundZeroPy) ds16v2@ds16v2:~$ source /home/ds16v2/PythonEnvs/GroundZeroPy/bin/activate
   > 
   > # to activate GroundZeroPy environment, you can see that, when environment 
   > # activated, at the beginning of the console there is a bracket within indicate 
   > # the current virtual environment used. 
   > ```
   >
   > 

   

   second, updated `pip/pip3` to higher version (how? goto requirement 6)

   solution: 

   1. enter the virtual environment

      ```
      ds16v2@ds16v2:~$ source /home/ds16v2/PythonEnvs/GroundZeroPy/bin/activate
      ```

   2. upgrade pip3 

      > (when in virtual environment, python and pip indicate python3 and pip3, because, for my understanding, we use `sudo apt-get install python3-venv` command for installation, that means, if no manually specify python version, by default, python3 will be the virtual environment language and interpretor )

      ```
      (GroundZeroPy) ds16v2@ds16v2:~$ pip install --upgrade pip
      Collecting pip
        Downloading https://files.pythonhosted.org/packages/fe/ef/60d7ba03b5c442309ef42e7d69959f73aacccd0d86008362a681c4698e83/pip-21.0.1-py3-none-any.whl (1.5MB)
          100% |████████████████████████████████| 1.5MB 859kB/s 
      Installing collected packages: pip
        Found existing installation: pip 8.1.1
          Uninstalling pip-8.1.1:
            Successfully uninstalled pip-8.1.1
      Successfully installed pip-21.0.1
      ```

      introduced new issue:

      ```
      (GroundZeroPy) ds16v2@ds16v2:~$ pip install numpy
      Traceback (most recent call last):
        File "/home/ds16v2/PythonEnvs/GroundZeroPy/bin/pip", line 7, in <module>
          from pip._internal.cli.main import main
        File "/home/ds16v2/PythonEnvs/GroundZeroPy/lib/python3.5/site-packages/pip/_internal/cli/main.py", line 60
          sys.stderr.write(f"ERROR: {exc}")
                                         ^
      SyntaxError: invalid syntax
      (GroundZeroPy) ds16v2@ds16v2:~$ pip --version
      Traceback (most recent call last):
        File "/home/ds16v2/PythonEnvs/GroundZeroPy/bin/pip", line 7, in <module>
          from pip._internal.cli.main import main
        File "/home/ds16v2/PythonEnvs/GroundZeroPy/lib/python3.5/site-packages/pip/_internal/cli/main.py", line 60
          sys.stderr.write(f"ERROR: {exc}")
                                         ^
      SyntaxError: invalid syntax
      (GroundZeroPy) ds16v2@ds16v2:~$ pip3 --version
      Traceback (most recent call last):
        File "/home/ds16v2/PythonEnvs/GroundZeroPy/bin/pip3", line 7, in <module>
          from pip._internal.cli.main import main
        File "/home/ds16v2/PythonEnvs/GroundZeroPy/lib/python3.5/site-packages/pip/_internal/cli/main.py", line 60
          sys.stderr.write(f"ERROR: {exc}")
                                         ^
      SyntaxError: invalid syntax
      
      ```

      solution for this issue reporduce:

      ```
      # 1. create a new virtual environment
      ds16v2@ds16v2:~$ python3 -m venv /home/ds16v2/PythonEnvs/GroundFirstPy
      
      # 2. activate new virtual environment
      ds16v2@ds16v2:~$ source /home/ds16v2/PythonEnvs/GroundFirstPy/bin/activate
      
      # 3. check pip versions (virtual envrionment is so useful)
      (GroundFirstPy) ds16v2@ds16v2:~$ pip --version
      pip 8.1.1 from /home/ds16v2/PythonEnvs/GroundFirstPy/lib/python3.5/site-packages (python 3.5)
      
      # 4. upgrade pip < 21, for python 3.5
      (GroundFirstPy) ds16v2@ds16v2:~$ pip install --upgrade "pip < 21.0"
      Collecting pip<21.0
        Downloading https://files.pythonhosted.org/packages/27/79/8a850fe3496446ff0d584327ae44e7500daf6764ca1a382d2d02789accf7/pip-20.3.4-py2.py3-none-any.whl (1.5MB)
          100% |████████████████████████████████| 1.5MB 89kB/s 
      Installing collected packages: pip
        Found existing installation: pip 8.1.1
          Uninstalling pip-8.1.1:
            Successfully uninstalled pip-8.1.1
      Successfully installed pip-20.3.4
      You are using pip version 20.3.4, however version 21.0.1 is available.
      You should consider upgrading via the 'pip install --upgrade pip' command.
      
      # 6. check updated pip
      (GroundFirstPy) ds16v2@ds16v2:~$ pip --version
      pip 20.3.4 from /home/ds16v2/PythonEnvs/GroundFirstPy/lib/python3.5/site-packages/pip (python 3.5)
      
      ```

      check if workable

      ```
      (GroundFirstPy) ds16v2@ds16v2:~$ pip install numpy
      DEPRECATION: Python 3.5 reached the end of its life on September 13th, 2020. Please upgrade your Python as Python 3.5 is no longer maintained. pip 21.0 will drop support for Python 3.5 in January 2021. pip 21.0 will remove support for this functionality.
      Collecting numpy
        Downloading numpy-1.18.5-cp35-cp35m-manylinux1_x86_64.whl (19.9 MB)
           |████████████████████████████████| 19.9 MB 6.4 MB/s 
      Installing collected packages: numpy
      Successfully installed numpy-1.18.5
      (GroundFirstPy) ds16v2@ds16v2:~$ pip install matplotlib
      DEPRECATION: Python 3.5 reached the end of its life on September 13th, 2020. Please upgrade your Python as Python 3.5 is no longer maintained. pip 21.0 will drop support for Python 3.5 in January 2021. pip 21.0 will remove support for this functionality.
      Collecting matplotlib
        Downloading matplotlib-3.0.3-cp35-cp35m-manylinux1_x86_64.whl (13.0 MB)
           |████████████████████████████████| 13.0 MB 180 kB/s 
      Collecting python-dateutil>=2.1
        Downloading python_dateutil-2.8.1-py2.py3-none-any.whl (227 kB)
           |████████████████████████████████| 227 kB 143 kB/s 
      Requirement already satisfied: numpy>=1.10.0 in ./PythonEnvs/GroundFirstPy/lib/python3.5/site-packages (from matplotlib) (1.18.5)
      Collecting pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1
        Downloading pyparsing-2.4.7-py2.py3-none-any.whl (67 kB)
           |████████████████████████████████| 67 kB 195 kB/s 
      Collecting kiwisolver>=1.0.1
        Downloading kiwisolver-1.1.0-cp35-cp35m-manylinux1_x86_64.whl (90 kB)
           |████████████████████████████████| 90 kB 196 kB/s 
      Collecting cycler>=0.10
        Downloading cycler-0.10.0-py2.py3-none-any.whl (6.5 kB)
      Collecting six
        Using cached six-1.15.0-py2.py3-none-any.whl (10 kB)
      Requirement already satisfied: setuptools in ./PythonEnvs/GroundFirstPy/lib/python3.5/site-packages (from kiwisolver>=1.0.1->matplotlib) (20.7.0)
      Installing collected packages: six, python-dateutil, pyparsing, kiwisolver, cycler, matplotlib
      Successfully installed cycler-0.10.0 kiwisolver-1.1.0 matplotlib-3.0.3 pyparsing-2.4.7 python-dateutil-2.8.1 six-1.15.0
      
      ```

      problem solved.

      to uninstall pip3 in base environment:

      ```
      root@ds16v2:~# sudo apt-get remove python3-pip
      ```

      

      for complete virtual environment creating steps using python3-venv, this method could not specify python version in the virtual environment

      ```
      # install python3-venv
      ds16v2@ds16v2:~$ sudo apt-get install python3-venv
      
      # create virtual environment
      ds16v2@ds16v2:~$ python3 -m venv /home/ds16v2/PythonEnvs/GroundFirstPy
      
      # activate vertual environment
      ds16v2@ds16v2:~$ source /home/ds16v2/PythonEnvs/GroundFirstPy/bin/activate
      
      # check python version
      (GroundFirstPy) ds16v2@ds16v2:~$ python --version
      Python 3.5.2
      
      # check pip version
      (GroundFirstPy) ds16v2@ds16v2:~$ pip --version
      pip 20.3.4 from /home/ds16v2/PythonEnvs/GroundFirstPy/lib/python3.5/site-packages/pip (python 3.5)
      
      # deactivate virtual environment
      (GroundFirstPy) ds16v2@ds16v2:~$ deactivate
      ds16v2@ds16v2:~$ 
      
      ```

      

      for differences in using virualvenv and python3-venv, the former can specify python version. 
      
      ![image-20210429104131046](/home/ds16v2/.config/Typora/typora-user-images/image-20210429104131046.png)

