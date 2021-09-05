# 2021年08月31日：cartographer 硬件準備

---

[toc]

---

## 0. 現有條件

1. thinkpad T480s
   1. i7
   2. mx150
   3. ubuntu 16.04
   4. 20GB free space
2. xiaoxin15
   1. i7
   2. mx450
   3. vmware ubuntu 18.04
   4. space undefined
3. server old
   1. super
   2. super
   3. ubuntu 16.04
   4. sys. space full
4. server new
   1. super
   2. super
   3. ubuntu 18.04
   4. no root access



ryan 那邊遇到了 安裝問題，我先試着在 虛擬機上安裝一下。

虛擬機上出重複安裝 cartographer，cartographer_ros，獲得可靠的安裝手冊。

在 server new 或 old 上搭建 cartographer 的環境。





---

2021年09月05日：

已經在 ubuntu18.04 LTS carto 虛擬機中安裝了 cartographer_ros

1. 線按照 cartographer 的安裝步驟，將所有的依賴安裝好。
2. 再按照 cartographer_ros 的官方安裝步驟，將所有的步驟安裝好。
3. 最主要的問題依然是 rosdep init 與 rosdep update。

