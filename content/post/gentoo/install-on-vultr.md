---
title: "Vultr 安装 Gentoo"
date: 2018-05-23T11:14:36+08:00
draft: true
comment: true
tags: ["vultr", "gentoo"]
categories: ["development"]
---

前段时间在 Vultr 服务器上成功安装了 Gentoo，发文分享下，记录安装的细节和需要注意的地方，以作笔记。
<!--more-->


## 准备 ISO

因为 Vultr 提供的 Linux 发行版并未包含 Gentoo，所以我们需要自己准备 Gentoo ISO。

首先进入 Vultr ISO 管理页面, 控制面板 -> **Servers** -> **ISO**，点击 **Add ISO**，填写 Gentoo ISO 地址即可。

- Gentoo 下载页面 - https://gentoo.org/downloads/。

本文使用的 Gentoo ISO 是 `AMD64 Minimal Installation CD`。


## 发布服务器

等 ISO 准备完毕后，接下来就是发布我们的服务器了，发布流程就不详述了，
关键需要在 **Server Type** -> **Upload ISO** 中勾选刚才准备的 ISO ，其他的按照你的喜欢和需要选择即可。

> 其实也可以在现有服务器的 **Settings** 中 **Custom ISO** 绑定 ISO，然后 **Reinstall Server**，不过这会丢失原服务器的数据，请注意！

为方便后续阐述，服务器的主机名（hostname）为 `srv-jp-01`，域名为 `srv-jp-01.errlogs.com`。


## 安装

### 启动 Gentoo Live CD

等服务器初始化完成后，紧接着我们打开 **View Console**，启动 Gentoo Live CD。

打开后，可能看到 iPXE 的欢迎提示，不需要理会这些（有兴趣的童鞋可以自行了解），直接点击 `Send CtrlAtrDel` 按钮，然后直接回车键，就可以启动 Gentoo Live CD。

### 开启 sshd 服务

由于 View Console 限制很多，比如无法复制粘贴，所以我们开启远程，用本地终端访问进行安装。

```shell
rc-service sshd start
```

### 修改 root 密码

```shell
passwd root
```

完成后，关闭 View Console，使用本地的终端连接服务器。

### 连接远程

```
ssh root@srv-jp-01.errlogs.com
```

### 测试网络

```
livecd ~ # ping www.gentoo.org
PING www.gentoo.org(2001:41c8:0:936::136 (2001:41c8:0:936::136)) 56 data bytes
64 bytes from 2001:41c8:0:936::136 (2001:41c8:0:936::136): icmp_seq=1 ttl=50 time=228 ms
64 bytes from 2001:41c8:0:936::136 (2001:41c8:0:936::136): icmp_seq=2 ttl=50 time=227 ms
^C
--- www.gentoo.org ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1006ms
rtt min/avg/max/mdev = 227.754/227.955/228.157/0.518 ms
```

可以看到网络是正常的，如果真的有问题，可能需要自行解决了。

- Gentoo 网络配置 - https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Networking/zh-cn

### 磁盘分区

接下来就是磁盘分区了，其中主要是分区表和分区方案。

#### 分区表

由于 Vultr “似乎” 不能支持 GPT 分区表（尝试过 GPT，但无法安装 Grub2，如果说错了，麻烦指正下），所以本文使用 MBR 分区技术。

#### 分区方案

服务器：
- 硬盘 - 25G
- 内存 - 1G

本文的磁盘是：/dev/vda

| 分区       | 文件系统     | 大小     | 描述                       |
|:----------|:-----------|:--------|:--------------------------|
| /dev/vda1 | bootloader | 2M      | BIOS boot partition       |
| /dev/vda2 | ext2       | 128M    | Boot system partition     |
| /dev/vda3 | swap       | 2G      | Swap partition            |
| /dev/vda4 | ext4       | 剩余空间  | Root partition            |

如果有足够空间的话，建议单独建一个 `var` 分区，因为很多 web 项目的代码，图片，数据库数据可能都放在这个分区。

另外由于后续会升级套餐，比如增加内存，CPU，磁盘等，可以预留足够的空间给 swap 分区，后续无需扩张该分区容量。

#### 分区

我们使用 fdisk 进行分区：

```
fdisk /dev/vda
```

> 关于 fdisk 的使用，已经超出本文范畴，你可以使用 m 命令查看帮助，以及根据 fdisk 给出的提示进行操作。

**BIOS 引导分区**

```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-52428799, default 2048): 2048
Last sector, +sectors or +size{K,M,G,T,P} (2048-52428799, default 52428799): +2M
```

**创建引导分区**

```
Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2): 2
First sector (6144-52428799, default 6144):
Last sector, +sectors or +size{K,M,G,T,P} (6144-52428799, default 52428799): +128M
```

将分区 `2` 设置为可引导：

```
Command (m for help): a
Partition number (1,2, default 2): 2
```

**创建 swap 分区

```
Command (m for help): n
Partition type
   p   primary (2 primary, 0 extended, 2 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (3,4, default 3): 3
First sector (268288-52428799, default 268288):
Last sector, +sectors or +size{K,M,G,T,P} (268288-52428799, default 52428799): +2G
```

设置分区类型为 swap：

```
Command (m for help): t
Partition number (1-3, default 3): 3
Hex code (type L to list all codes): 82
```

**创建根分区**

以上分区方案，剩余容量都分配给根分区，first 和 last sector 均不用理会，直接回车即可。

```
Command (m for help): n
Partition type
   p   primary (3 primary, 0 extended, 1 free)
   e   extended (container for logical partitions)
Select (default e): p

Selected partition 4
First sector (4462592-52428799, default 4462592):
Last sector, +sectors or +size{K,M,G,T,P} (4462592-52428799, default 52428799):
```

**预览分区布局**

在保存之前，先看下是否分区正确：

```
Command (m for help): p

Device     Boot   Start      End  Sectors  Size Id Type
/dev/vda1          2048     6143     4096    2M 83 Linux
/dev/vda2  *       6144   268287   262144  128M 83 Linux
/dev/vda3        268288  4462591  4194304    2G 82 Linux swap / Solaris
/dev/vda4       4462592 52428799 47966208 22.9G 83 Linux
```

**保存分区布局**

```
Command (m for help): w
```

#### 创建文件系统

分区创建完后，接下来自然是创建文件系统：

**引导分区**

```
mkfs.ext2 /dev/vda2
```

**根分区**

```
mkfs.ext4 /dev/vda4
```

**激活swap分区**

```
mkswap /dev/vda3
swapon /dev/vda3
```

#### 挂载分区

```
mount /dev/vda4 /mnt/gentoo
```

如果有单独的 `var`、`usr` 等分区，也需要逐个挂载。另外一开始目录是空的，所以你还需要先创建目录，然后才能挂载，比如：

```
mkdir /mnt/gentoo/var
mount /dev/vda* /mnt/gentoo/var
```

### 安装 stage3

**进入 /mnt/gentoo 目录**

```
cd /mnt/gentoo
```

> 因为我们的目的是将 stage 包解压到根分区，如果没有切换到 /mnt/gentoo，解压 stage 时，需要指定输出目录为 /mnt/gentoo，为了减少麻烦，推荐先进入该目录，然后下载、解压。这也是为什么要先挂载 `var` `usr` 等分区的原因。

**下载 stage 包**

```
wget http://ftp.jaist.ac.jp/pub/Linux/Gentoo/releases/amd64/autobuilds/20180522T214503Z/stage3-amd64-20180522T214503Z.tar.xz
```

> 这里笔者选择的是日本的镜像（服务器位于日本东京，就近原则），其他镜像地址可以在 https://www.gentoo.org/downloads/mirrors/ 找到。

**解压 stage 包**

```
tar xvpf stage3-amd64-20180522T214503Z.tar.xz --numeric-owner
```

- x - 表示解压
- v - 表示详细信息
- p - 表示保留权限
- f - 表示解压的是一个文件，而不是标准输入
- --numeric-owner - 确保提取的文件的用户和组ID与Gentoo发布工程团队预期的保持一致

### 安装基本系统

#### 选择镜像

```
mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf
```

#### Gentoo ebuild 仓库

```
mkdir /mnt/gentoo/etc/portage/repos.conf
cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
```

#### 复制DNS信息

```
cp -L /etc/resolv.conf /mnt/gentoo/etc/
```

#### 挂载必要的文件系统

```
mount -t proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
```

#### Chroot

```
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) $PS1"
```

#### 挂载引导分区

```
mount /dev/vda2 /boot
```

#### 配置 Portage

**更新 ebuild 快照**

```
emerge-webrsync
```

**选择配置文件**

```
eselect profile list
eselect profile set default/linux/amd64/17.0/selinux
```

**更新 @world 集合**

```
emerge --ask --update --deep --newuse @world
```

#### 时区

```
echo "Asia/Shanghai" > /etc/timezone
emerge --config sys-libs/timezone-data
```

### 配置内核

#### 安装源码

```
emerge --ask sys-kernel/gentoo-sources
```

#### 配置内核

**安装 pciutils 工具**

```
emerge --ask sys-apps/pciutils
```

**查看硬件**

```
lspci

00:00.0 Host bridge: Intel Corporation 440FX - 82441FX PMC [Natoma] (rev 02)
00:01.0 ISA bridge: Intel Corporation 82371SB PIIX3 ISA [Natoma/Triton II]
00:01.1 IDE interface: Intel Corporation 82371SB PIIX3 IDE [Natoma/Triton II]
00:01.2 USB controller: Intel Corporation 82371SB PIIX3 USB [Natoma/Triton II] (rev 01)
00:01.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 03)
00:02.0 VGA compatible controller: Cirrus Logic GD 5446
00:03.0 Ethernet controller: Red Hat, Inc. Virtio network device
00:04.0 SCSI storage controller: Red Hat, Inc. Virtio block device
00:05.0 Unclassified device [00ff]: Red Hat, Inc. Virtio memory balloon
00:06.0 Unclassified device [00ff]: Red Hat, Inc. Virtio RNG
00:07.0 Ethernet controller: Red Hat, Inc. Virtio network device
```

先把上述输出记录下来，我们在配置内核的时候使用到。

**配置内核**

```
cd /usr/src/linux
make menuconfig
```

我们采取手动方式进行内核配置，可以参阅官方的[配置内核](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel/zh-cn)，这里我仅简述一些关键的配置。

> 进入菜单配置界面后，按 `/` 可以进行搜索。

**文件系统**

按照分区方案里边的，我们需要开启 `CONFIG_EXT2_FS` 和 `CONFIG_EXT4_FS`

**Virtio**

根据 lspci，我们还需要配置与 Virtio 相关的 network、block 等设备配置。我们依次搜索，选择索引序号，进行配置。

- **VIRTIO_PCI**
- **VIRTIO_BLK**
- **VIRTIO_NET**
- **VIRTIO_BALLOON**

> 请根据自己服务器实际情况进行配置。

内核配置完毕后，保存退出。

**编译内核**

```
make && make modules_install
make install
```

> 编译比较费时间，这里我们可以稍作休息。

**initramfs**

如果 `var`、`usr` 是独立分区，则需要生成 `initramfs`：

```
emerge --ask sys-kernel/genkernel
genkernel --install initramfs
```

**安装固件**

```
emerge --ask sys-kernel/linux-firmware
```

### 配置系统

#### fstab

编辑 `etc/fstab`：

```
/dev/vda2   /boot   ext2    defaults,noatime    0   2
/dev/vda3   none    swap    sw                  0   0
/dev/vda4   /       ext4    noatime             0   1
```


#### 配置网络

```
emerge --ask --noreplace net-misc/netifrc
```

```
nano -w /etc/conf.d/net
```

```
config_eth0="dhcp"
```

```
cd /etc/init.d
ln -s net.lo net.eth0
rc-update add net.eth0 default
```

#### 修改密码

```
passwd
```

#### 工具

```
emerge --ask app-admin/sysklogd sys-process/cronie sys-apps/mloc
rc-update add sysklogd default
rc-update add cronie default
```

#### 开启 sshd 服务

```
rc-update add sshd default
```

#### 安装 DHCP 客户端

```
emerge --ask net-misc/dhcpcd
```

### 配置引导加载程序

```
emerge --ask --verbose sys-boot/grub:2
grub-install /dev/vda
grub-mkconfig -o /boot/grub/grub.cfg
```

### 添加用户

```
useradd -m -G users,wheel -s /bin/bash gentoo
passwd gentoo
```

### 重启

```
reboot
```