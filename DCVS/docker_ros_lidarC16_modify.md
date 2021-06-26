# docker + ros + lidarC16

[toc]



---

## 20210621 操作

目标：完成 20210618 剩余步骤，在 ros rviz 中显示 lslidar_c16 的点云图。已经可以ping通 192.168.1.200

![image-20210621173625476](docker_ros_lidarC16.assets/image-20210621173625476.png)

![image-20210621175313072](docker_ros_lidarC16.assets/image-20210621175313072.png)



![image-20210621175852574](docker_ros_lidarC16.assets/image-20210621175852574.png)



![image-20210621175502011](docker_ros_lidarC16.assets/image-20210621175502011.png)

<img src="docker_ros_lidarC16.assets/image-20210621175929324.png" alt="image-20210621175929324" style="zoom:80%;float:left" />

![image-20210621180113459](docker_ros_lidarC16.assets/image-20210621180113459.png)





---

## 20210618 操作

目标：docker GUI，docker file，lslidar

节点：20210617-2-1

### 1. 启动 lidar，通过 rviz 查看

#### E-1: ROS “is neither a launch file in package”报错

![image-20210618130921142](docker_ros_lidarC16.assets/image-20210618130921142.png)

原因： 没有找到 package 配置文件，使用 `rospack list, rospack find packageName` 查看需要的 package。



### 2. docker + ros + rviz + lslidar







---

## 20210617 操作



### 1. 关于 docker volume 的使用

总结：

![image-20210617111614186](docker_ros_lidarC16.assets/image-20210617111614186.png)

docker 中 创建了 `ros_lslidar_c16:v1.0, [5205e9ce027e   2.11GB]` 的镜像，这个镜像包含了 ros 环境 + lslidar_c16 的驱动。此镜像作为当前雷达开发的 lower layer，作为只读层。



基于此镜像 创建了一个容器 `angry_carson [cID: 7138b0a9f2eb iID: 5205e9ce027e]`，作为开发 lslidar_c16 的环境备份，此 container 包含: 1. ros-kinetic 环境，2. lslidar_c16 驱动。



通过使用 `docker run`，相当于 `docker create + docker start`：

`/home/ds16v2/Docker# docker run -i -t -v /home/ds16v2/Docker/volume_5205e:/root/catkin_ws 5205e9ce027e /bin/bash`

使用 `5205e9ce027e` 镜像创建一个新的 container <cN: awesome_noether, cID: 7d8685722712>，与 container <cN: angry_carson, cID: 7138b0a9f2eb> 平行，但是这个container 用作 lslidar_c16 的主要开发容器。

![image-20210617173216673](docker_ros_lidarC16.assets/image-20210617173216673.png)

这个容器实现了<主机跟容器之间的数据共享>，`-v /home/ds16v2/Docker/volume_5205e:/root/catkin_ws 5205e9ce027e`，抽象为 `-v A:B`；其中 A -- 主机上的地址，B -- 容器中的地址（这两个地址如果不存在会自动创建，一旦容器运行，A与B会完全同步）

![image-20210617173336092](docker_ros_lidarC16.assets/image-20210617173336092.png)

<img src="docker_ros_lidarC16.assets/image-20210617173432912.png" alt="image-20210617173432912" style="zoom:80%; float:left;" />

* 修改 typora 图像位置 `<img src="xxx.png" alt="image-20210617173432912" style="zoom:80%; float:left;" />`



### 2. docker + ros + gui

#### 1. 安装 xhost

```
root@7d8685722712:~/catkin_ws# apt install xhost

Reading package lists... Done
Building dependency tree       
Reading state information... Done
Package xhost is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source
However the following packages replace it:
  x11-xserver-utils

E: Package 'xhost' has no installation candidate

root@7d8685722712:~/catkin_ws# apt install x11-xserver-utils
```



```
root@7d8685722712:~/catkin_ws# xhost
xhost:  unable to open display ""

root@7d8685722712:~/catkin_ws# 
```













---

## 1. 当前基本配置

记录当前的 lslidar_c16 docker + ros 环境下的一些配置信息

> 在 docker 中，镜像（image）是只读的，容器（container）是可读可写的。

### 1. ros 镜像

   ```
   image id: 55f40ca92c5b
   docker images
   docker image ls [-a]
   ```

   ![image-20210616162159936](docker_ros_lidarC16.assets/image-20210616162159936.png)

### 2. container ID

   ```
   container id: e3d519d0aa2a
   docker container ls [-a]
   ---
   docker container -h
   docker container stop containerID1 [containerID2] ...
   docker container rm containerID1 [containerID2] ...
   ```

   ![image-20210616162907216](docker_ros_lidarC16.assets/image-20210616162907216.png)

### 3. lslidar_c16 相关路径

   ```
   docker container ls -a                                 # 查看所有container列表
   docker container start containerID                     # 启动一个已经停止的container，后台
   docker container exec -it containerID /bin/bash        # 使用交互模式进入一个container
   docker container logs -t containerID				   # 查看container的日志
   ---
   ls
   pwd
   cd													   # 进入container的home目录下，就是root
   ls													   # 可以看到lslidar_c16文件夹
   ---
   docker diff relaxed_hopper							   # 查看当前container与其image的不同
   
   ```

   ![image-20210616165835221](docker_ros_lidarC16.assets/image-20210616165835221.png)

   

   `docker container logs -t containerID`

   ![image-20210616170428128](docker_ros_lidarC16.assets/image-20210616170428128.png)

   

   `docker diff containerName/containerID`

   ![image-20210616172651775](docker_ros_lidarC16.assets/image-20210616172651775.png)



### 4. 问，如何将此container作为一个基线版本？（commit、volume）

   * 创建一个新镜像 `docker commit`

   ```
   docker container ls -a								# 查看所有的container
   docker commit \										# 从修改的container中创建新image
   	 -a "jacob" \
   	 -m "ros for lslidar_c16" \
   	 e3d519d0aa2a \
   	 ros_lslidar_c16:v1.0
   docker image ls -a									# 查看新创建的image
   docker history ros_lslidar_c16:v1.0
   ```

   ![image-20210616174738040](docker_ros_lidarC16.assets/image-20210616174738040.png)

   ![image-20210616174820455](docker_ros_lidarC16.assets/image-20210616174820455.png)

   ![image-20210616175301382](docker_ros_lidarC16.assets/image-20210616175301382.png)

   

   * 使用`volume` (todo)（见下一章）

   * docker 的文件系统

     docker 的安装目录是在 `/var/lib/docker`

     ```
     mkdir ~/Docker									# 在主机目录创建docker文件夹
     cp -r /var/lib/docker .							# root模式下将docker文件拷贝到当前目录
     chmod 777 -R docker								# 递归修改权限，使当前用户可以访问
     
     docker info										# 显示docker系统级的信息
     ```

     ![image-20210616202730519](docker_ros_lidarC16.assets/image-20210616202730519.png)

     > docker 在系统中默认的文件目录位于 `/var/lib/docker` 下，container 文件系统 保存的位置在 `xxx/docker/overlay2/` 







----

## 2. docker volume

使用命令：

```
docker run -i -t -v /home/ds16v2/Docker/volume_5205e:/root/catkin_ws 5205e9ce027e /bin/bash
```

`-v /home/ds16v2/Docker/volume_5205e:/root/catkin_ws 5205e9ce027e`，抽象为 `-v A:B`；其中 A -- 主机上的地址，B -- 容器中的地址（这两个地址如果不存在会自动创建，一旦容器运行，A与B会完全同步）





---

## 3. docker file





---

raw:

```
root@ds16v2:~# docker images
REPOSITORY        TAG             IMAGE ID       CREATED        SIZE
ros_lslidar_c16   v1.0            5205e9ce027e   41 hours ago   2.11GB
ros               kinetic-robot   55f40ca92c5b   2 weeks ago    1.21GB
hello-world       latest          d1165f221234   3 months ago   13.3kB
root@ds16v2:~# 

root@ds16v2:~# docker container ls -a
CONTAINER ID   IMAGE               COMMAND                  CREATED        STATUS                    PORTS     NAMES
7d8685722712   5205e9ce027e        "/ros_entrypoint.sh …"   17 hours ago   Up 17 hours                         awesome_noether
7138b0a9f2eb   5205e9ce027e        "/ros_entrypoint.sh …"   23 hours ago   Exited (0) 20 hours ago             angry_carson
e3d519d0aa2a   ros:kinetic-robot   "/ros_entrypoint.sh …"   2 days ago     Exited (0) 39 hours ago             relaxed_hopper
7953a5fc88a1   hello-world         "/hello"                 2 days ago     Exited (0) 2 days ago               interesting_newton
root@ds16v2:~# 

```



## APPENDIX I: IMAGE INFO

For registed image:

| REPOSITORY      | TAG           | IMAGE ID     | SIZE   | INFO                                 |
| --------------- | ------------- | ------------ | ------ | ------------------------------------ |
| ros             | kinetic-robot | 55f40ca92c5b | 1.21GB | 原生 ros-kinetic 环境                |
| ros_lslidar_c16 | v1.0          | 5205e9ce027e | 2.11GB | lslidar_c16 driver; ros-kinetic-pcl; |
|                 |               |              |        |                                      |





---

## APPENDIX II: CONTAINER INFO

| CONTAINER ID | IMAGE        | NAMES           | INFO                          |
| ------------ | ------------ | --------------- | ----------------------------- |
| 7138b0a9f2eb | 5205e9ce027e | angry_carson    | --                            |
| 7d8685722712 | 5205e9ce027e | awesome_noether | tree; volume->/root/catkin_ws |
|              |              |                 |                               |

