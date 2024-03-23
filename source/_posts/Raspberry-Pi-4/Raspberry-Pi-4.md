---
title: 树莓派4B安装archlinux
cover: date
banner:
  type: img
  bgurl: '/img/common/{C739C275-C89C-1860-17D8-40F0C62E7ACA}.jpg'
  bannerText: ''
tags:
  - '笔记'
categories: '笔记'
date: 2024-03-23 23:53:36
---
> 在树莓派4B上安装archlinux。参考文档：[https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-4](https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-4)
<!-- more -->

## 一、准备事项
- 一台linux环境设备
- 树莓派4B
- SD卡
- 读卡器
- 科大镜像地址：[https://mirrors.ustc.edu.cn/archlinuxarm/os/ArchLinuxARM-rpi-aarch64-latest.tar.gz](https://mirrors.ustc.edu.cn/archlinuxarm/os/ArchLinuxARM-rpi-aarch64-latest.tar.gz)

> 本文使用了安装了ubuntu-22.04.4-live-server的笔记本对SD卡进行的archlinux安装。

## 二、安装步骤
下面步骤中**sdX**表示为linux设备中显示的SD卡的设备名称，具体实现时需替换为实际名称。例如我的设备识别是**sdb**，则所有的**sdX**均替换为**sdb**即可。

**注意**：查看磁盘信息：`fdisk -l`

### 1. 通过**fdisk**对 SD 卡进行分区：
```bash
fdisk /dev/sdX
```

### 2. 在 fdisk 提示符下，删除旧分区并创建一个新分区，步骤如下：
- 删除旧分区：在 fdisk 提示符下，输入 `o`，然后按回车键。这将清除驱动器上的所有分区。
- 查看当前分区： 在 fdisk 提示符下，输入 `p`，然后按回车键。这将显示当前分区，此时应该没有分区。
- 创建第一个分区：在 fdisk 提示符下，键入`n`，然后键入`p` 表示主分区，然后键入`1`表示驱动器上的第一个分区，按 Enter 接受默认的第一个扇区，然后键入 +200M 表示最后一个扇区。
- 设置第一个分区类型：在 fdisk 提示符下，键入`t`，然后键入`c` 将第一个分区设置为`W95 FAT32 (LBA)`。
- 创建第二个分区：在 fdisk 提示符下，键入`n`, 然后键入`p` 表示主分区，然后键入`2`表示驱动器上的第二个分区，连续键入两次Enter接受默认的第一个和最后一个扇区。
- 保存分区并退出：在 fdisk 提示符下，键入`w`，然后按回车键。这将保存写入分区表并退出fdisk。

### 3. 创建并挂载FAT文件系统：
```bash
mkfs.vfat /dev/sdX1
mkdir boot
mount /dev/sdX1 boot
```

### 4. 创建并挂载XFS文件系统：
```bash
mkfs.xfs /dev/sdX2
mkdir root
mount /dev/sdX2 root
```

### 5. 下载并解压根文件系统（以 root 身份，而不是通过 sudo）：
```bash
wget https://mirrors.ustc.edu.cn/archlinuxarm/os/ArchLinuxARM-rpi-aarch64-latest.tar.gz
bsdtar -xpf ArchLinuxARM-rpi-aarch64-latest.tar.gz -C root
sync
```

### 6. 将引导文件移动到第一个分区：
```bash
mv root/boot/* boot
```

### 7. 更新与 Raspberry Pi 3 相比的不同 SD 块设备的 /etc/fstab
```bash
sed -i 's/mmcblk0/mmcblk1/g' root/etc/fstab
```

### 8. 取消挂载两个分区：
```bash
umount boot root
```

### 9. 插入SD卡，启动系统：
将SD卡插入树莓派，连接网络，通上电源。

### 10.使用SSH连接到路由器提供给树莓派主板的IP地址。
- 通过默认用户登录：默认用户名为 ==alarm== ，默认密码也为 ==alarm== 。 
- root用户默认密码为 ==root== 。
### 11. 初始化 pacman 密钥环并填充 Arch Linux ARM 软件包签名密钥：
```bash
pacman-key --init
pacman-key --populate archlinuxarm
```

至此系统完成安装。