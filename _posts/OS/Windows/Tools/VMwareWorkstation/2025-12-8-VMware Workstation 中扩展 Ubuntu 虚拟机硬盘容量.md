---
title: "VMware Workstation 中扩展 Ubuntu 虚拟机硬盘容量 -- 命令行方式"
date: 2025-12-09 10:00:00 +0800
categories: [OS, Windows, Tools, VMware Workstation]
permalink: /posts/OS/Windows/Tools/VMware-Workstation/
---

&emsp;&emsp;本篇 note 不适用于 `LVM` (Logical Volume Manager) 类型的扩容，怎么判断自己的 Ubuntu 是否是 LVM 类型的呢，终端执行 `lsblk`，如果你看到某个分区格式（TYPE 字段列）显示为：
```
LVM2_member
```
那你就是 LVM。

---

先查看一下磁盘的使用情况，终端执行 `df -h`:
```bash
zlj@zlj-vm:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           388M  1.9M  386M   1% /run
/dev/sda5        20G   19G  8.0K 100% /
```
+ 字段说明
  + Size → 分区总容量
  + Used → 使用空间
  + Avail → 剩余空间
  + Use% → 使用百分比
  + Mounted on → 挂载目录（/、/boot 等）

- 文件系统的 **根目录 `/`** 被挂载在 **第一个硬盘（`/dev/sda`）** 的 **第五个分区（`/dev/sda5`）**
  - **`/dev/sda`** 是第一块硬盘，通常是主硬盘。
  - **`/dev/sda5`** 是这块硬盘上的第五个分区。
  - 根目录 **`/`** 被挂载在这个分区上，意味着所有系统文件、程序以及数据都会存储在 `/dev/sda5` 分区中。

这里可以看到我的 Ubuntu 虚拟机文件系统根目录 `/` 挂载到的分区已经 100% 使用了，需要进行扩容了，进入正题；


## 1. 扩容前准备: 通过 SSH 服务连通 Windows 主机和 Ubuntu 虚拟机
<details>
<summary>personal exp.</summary>
+ 两次的扩容经历，发现在扩展分区，重启虚拟机之后，每次 VMware Workstation 的虚拟机界面都是黑屏的；<br>
+ 好在第一次扩容之前，自己就有通过 SSH 服务连通了我的 windows 主机和 Ubuntu 虚拟机，可以继续进行命令行的操作；
</details>

这里不提供具体的 SSH 连接指导（可以问问 AI），并不麻烦；
个人有两种方式：
1. MobaXterm 中
2. VScode 中通过 Remote-SSH 插件进行连接；

> 注意：这步很重要，不能跳过！
{: .prompt-tip }

空间不够安装包？删除几个不重要的文件喽~


## 2. 备份数据
在修改分区之前，建议对重要数据进行备份，以防止意外丢失。首先关闭虚拟机；


### 2.1 删除快照
虚拟机设置中，扩展（Extend）按键能够被点击有两个必要条件：
1. 虚拟机关机
2. 没有快照<br>
不然就是灰色的，不能点击；


### 2.2 备份
虚拟机的**虚拟硬盘文件**：<br>
菜单栏"虚拟机" -> 设置 → 硬盘 → 磁盘文件<br>
such as: `D:\Program Files (x86)\VMwares\ubuntu20.04.5\Ubuntu20.04.5.vmdk`

这个文件包含了虚拟机的硬盘数据，包括操作系统、应用程序和所有的文件。<br>
但是，虚拟机的完整备份不仅仅是这个 **`.vmdk` 文件**。一个完整的虚拟机备份还包括以下文件：
- **`.vmdk` 文件**：虚拟硬盘文件，包含所有的数据。
- **`.vmx` 文件**：虚拟机的配置文件，记录虚拟机的硬件配置（如内存、CPU、网络等）。
- **`.nvram` 文件**：虚拟机的 BIOS 设置文件（可选）。
- **`.vmsd` 文件**：快照相关的文件（如果使用快照功能）。
- **`.log` 文件**：虚拟机的日志文件（可选）


#### 2.2.1 备份虚拟机文件夹：

为了确保完整备份虚拟机，应该将整个虚拟机文件夹（包含所有文件）复制到备份位置，而不仅仅是 `.vmdk` 文件。假设你的虚拟机文件夹路径是：
```
D:\Program Files (x86)\VMwares\ubuntu20.04.5\
```
将这一整个文件夹拷贝备份一下；


## 3. 扩展Ubuntu虚拟机的硬盘大小

### 3.1 虚拟机设置中，**扩展虚拟磁盘的容量**

+ brief:
  - 虚拟机关机 → 虚拟机设置 → 硬盘 → 扩展（删除快照才能用） → 设置新的磁盘大小 → 点击扩展

---
+ detail:
  - **打开 VMware Workstation** 并选择要调整的虚拟机。
  - **虚拟机关机**（确保处于关机状态，而非休眠）。
  - 选择**虚拟机**菜单栏 -> **快照**菜单项 -> **快照管理器** -> 选择快照，删除（无快照跳过此步骤）
  - 进入 **设置**：
      - 在虚拟机选项卡中，点击 **Edit virtual machine settings** 或右键虚拟机，选择 **Settings**。
  - 在设置窗口中，选择 **Hard Disk**。
  - 点击 **Utilities** 或 **Expand** 按钮。
  - 输入新的磁盘大小（例如，增加到 100GB）。
  - 确认并保存设置。

扩展虚拟磁盘的容量，扩展完成后，VMware Workstaion提示**磁盘已成功扩展。您必须从客户机操作系统内部对磁盘重新进行分区和扩展文件系统。**


### 3.2 虚拟机上，进行分区和扩展文件系统

#### 3.2.1 登录虚拟机后，运行以下命令查看磁盘大小：

```bash
lsblk
```
查看是否有未分配的额外空间（通常以 sda 或其他设备名称显示）。
```bash
# 此时查看磁盘空间是有60G的，不过磁盘分区还是20G左右的原来大小
# ...
sda      8:0    0    60G  0 disk
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0     1K  0 part
└─sda5   8:5    0  19.5G  0 part /
# ...
```
其实我点击打开虚拟机，虽然有系统启动的过程，但是之后屏幕就是黑的，进不去，试了下 MobaXterm 之前建立的连接还在，能够用命令进行操作；(这就是需要第 1 步的原因)


#### 3.2.2 手动分区和扩展
---
##### 3.2.2.1 查看当前分区表
```bash
sudo fdisk -l
```

确认磁盘 /dev/sda 的总大小为 60GB，以及是否有未分配的空间。

```
Device     Boot   Start      End  Sectors  Size Id Type
/dev/sda1  *       2048  1050623  1048576  512M  b W95 FAT32
/dev/sda2       1052670 41940991 40888322 19.5G  5 Extended
/dev/sda5       1052672 41940991 40888320 19.5G 83 Linux
```
磁盘 `/dev/sda` 的布局如下：
+ `/dev/sda1`：启动分区（EFI分区），大小为 512MB。
+ `/dev/sda2`：扩展分区，包含 `sda5`。
+ `/dev/sda5`：Linux 根分区（挂载到 `/`），大小为 19.5GB。
+ 剩余空间未分配，可用于扩展

**逻辑分区和扩展分区：**
<br>&emsp;&emsp;**`sda5`** 位于扩展分区（`sda2`）中，这是因为在 MBR（主引导记录）分区表中，一个磁盘最多只能有四个主分区。如果需要更多分区，就需要使用扩展分区来容纳逻辑分区，`sda5` 就是这样一个逻辑分区，它被包含在 `sda2`（扩展分区）中。

**分区结构说明：**
  - **`/dev/sda1`** 是主分区，通常是 EFI 系统分区（启动分区），大小为 512MB。
  - **`/dev/sda2`** 是**扩展分区**（`Extended`），它本身没有实际存储数据，但它为逻辑分区提供容器。
  - **`/dev/sda5`** 是**逻辑分区**，属于 `sda2` 扩展分区的一个逻辑分区，挂载了 `/`（根分区），大小为 19.5GB。

&emsp;&emsp;虚拟磁盘的大小已经扩展到 60GB，但当前的分区（`sda1`、`sda2`、`sda5`）总共只占用了 20GB 左右，剩余的 40GB 还未被分配到任何分区。<br>
&emsp;&emsp;目标是将未分配的空间添加到 `sda5`（根分区）。由于 `sda5` 位于扩展分区中，需要通过调整分区的大小来完成扩展。


##### 3.2.2.2 扩展分区 -- 使用命令行工具
---
###### 3.2.2.2.1 删除扩展分区和逻辑分区：

这个操作会稍微复杂一点，需要确保原 `sda5` 的起始位置不变。
+ 运行 `fdisk` 来修改分区表：
  ```bash
  sudo fdisk /dev/sda
  ```
+ 删除现有的 `sda2` 和 `sda5`（注意：删除分区时不会删除数据，但要小心操作）：
  ```bash
  zlj@zlj-vm:~$ sudo fdisk /dev/sda
  [sudo] password for zlj:

  Welcome to fdisk (util-linux 2.34).
  Changes will remain in memory only, until you decide to write them.
  Be careful before using the write command.


  Command (m for help): d
  Partition number (1,2,5, default 5): 5

  Partition 5 has been deleted.

  Command (m for help): d
  Partition number (1,2, default 2): 2

  Partition 2 has been deleted.
  ```
  输入 d 删除分区，分别删除 sda5 和 sda2。
    + 第一次输入d，输入 5，删除了 sda5
    + 再输入d，输入 2，删除了 sda2

###### 3.2.2.2.2 重新创建分区
---
####### 3.2.2.2.2.1 创建**扩展分区**

由于你需要在扩展分区内创建逻辑分区，你需要先创建一个 扩展分区（e）。<br>
1. 输入 n（new）创建一个新分区。
```bash
Command (m for help): n
```
2. 选择 e（extended）作为分区类型。
```bash
Partition type
  p primary (1 primary, 0 extended, 3 free)
  e extended (container for logical partitions)
  Select (default p): e
```
3. 设置分区编号为 2：
  + 系统会默认选择编号为 2，直接回车即可。
```bash
Partition number (2-4, default 2): <Enter>
```
4. 选择扩展分区的起始扇区和结束扇区：
```bash
First sector (1052670-..., default 1052670): <回车>
Last sector, +sectors or +size{K,M,G,T} (1052670-..., default ...): <回车>
```
---
+ 起始扇区：一般按默认值（直接回车）。
  + 起始扇区要与之前的扩展分区一致。例如，之前扩展分区的起始扇区为 `1052670`，则输入此值。如果不确定，可以参考你之前记录的起始扇区。
  + 如果起始扇区默认正确，直接按回车。
  ```bash
  First sector (xxxx-xxxx, default 1052670): 1052670
  ```
+ 结束扇区：输入磁盘剩余空间的最大值（或者直接按默认值回车）。
  + 将扩展分区扩展到磁盘的最后一个扇区，直接回车即可使用默认值。
  ```bash
  Last sector, +sectors or +size{K,M,G,T} (xxxx-xxxx, default xxxx): <Enter>
  ```
5. 扩展分区创建完成后，fdisk 会提示已创建。

---
####### 3.2.2.2.2.2 创建**逻辑分区**

扩展分区内可以创建一个或多个逻辑分区。接着创建 逻辑分区（n）。

1. 输入 n 创建新的分区。
  ```bash
  Command (m for help): n
  ```

2. 选择逻辑分区类型
  <br>系统会自动识别逻辑分区，直接按回车。
  ```bash
  Partition type
  l logical (5 or over)
  p primary (1 primary, 1 extended, 2 free)
  Select (default p): l
  ```

3. 确认逻辑分区的编号为 5：
  <br>逻辑分区的编号默认从 5 开始，直接回车即可。
  ```bash
  Partition number (5-...): 5
  ```

4. 选择逻辑分区的起始扇区和结束扇区：
  ```bash
  First sector (1052672-..., default 1052672): <回车>
  Last sector, +sectors or +size{K,M,G,T} (1052672-..., default ...): <回车>
  ```
  + 起始扇区：按默认值（直接回车）。
    <br>起始扇区必须与之前的 `sda5` 保持一致。例如，之前的起始扇区为 1052672，则输入此值。
    <br>如果起始扇区默认正确，直接回车。
    ```bash
    First sector (1052672-xxxx, default 1052672): 1052672
    ```
  + 结束扇区：指定分区大小（直接回车使用剩余空间）。
    <br>将逻辑分区扩展到扩展分区的最后，直接按回车使用默认值。
    ```bash
    Last sector, +sectors or +size{K,M,G,T} (1052672-..., default ...): <回车>
    ```

5. 分区创建完成后，`fdisk` 会提示已创建

6. 确认逻辑分区创建完成后，输入 p 查看分区表。

---
####### 3.2.2.2.2.3 检查新分区表 & 写入更改

1. 检查新的分区表 
  <br>在完成所有分区后，输入 p 打印当前分区表，确认分区配置是否符合你的预期。
    ```bash
    Command (m for help): p
    ```
  内容如下：
    ```bash
    Device     Boot   Start      End  Sectors  Size Id Type
    /dev/sda1  *       2048  1050623  1048576  512M  b W95 FAT32
    /dev/sda2       1052670  125829119  124776450  59.5G  5 Extended
    /dev/sda5       1052672  125829119  124776448  59.5G 83 Linux
    ```

2. 写入更改到磁盘
  <br>如果确认分区表没有问题，可以输入 w 写入更改到磁盘。
    ```bash
    Command (m for help): w
    ```

3. 保留原有签名
  <br>一执行 fdisk 的 w 命令进行保存；

    ```bash
    Created a new partition 5 of type 'Linux' and of ize 59.5GiB.
    Partition #5 contains a ext4 signature.
    
    Do you want to remove the signature? [Y]es/[N]o:
    ```
  N → 保留原有的文件系统和数据。<br>
  Y → 在新分区上创建一个全新的文件系统（例如重新格式化为 `ext4`）

4. 检查分区是否成功扩展
  <br>使用以下命令检查新的分区：
    ```bash
    lsblk
    ```
  你应该看到 `/dev/sda5` 的大小显示为 59.5GB。
   
5. 扩展文件系统到新分区大小
  <br>虽然分区已经扩展，但文件系统仍然保持之前的大小（20GB）。需要扩展文件系统以匹配新分区的大小。
     1. 使用以下命令扩展文件系统：

        ```bash
        sudo resize2fs /dev/sda5
        ```
      这会将文件系统扩展到分区的最大大小（59.5GB）。
       
     2. 执行完成后，可以再次检查文件系统大小：
   
         ```bash
         df -h
         ```

6. reboot & 验证
   1. `reboot` 重启 Ubuntu 虚拟机
   2. 命令 `df -h` 验证区数据完好，文件系统已扩展到 59.5GB。
   3. 如果分区能够正常挂载并访问，则操作成功。
