# Linux 命令笔记

> 注：测试环境 Ubuntu  18.04

[TOC]

## 文件管理

```c
// 复制文件夹 009，并重命名为 0010
$ cp -a 009 0010


// 查看单个文件大小
$ du -hs t.html

```


## 磁盘管理

### `df` : 查看磁盘分区使用状况

参数：

`-l` 仅显示本地磁盘
`-a` 显示所有磁盘
`-h` 以 1024 计算，显示合适的容量单位
`-H` 以 1000 计算，显示合适的容量单位
`-T` 显示磁盘分区类型
`-t <args>` 显示指定文件系统类型的磁盘分区
`-x <args>` 不显显示指定文件系统类型的磁盘分区

示例：
```c

// 查看所有磁盘分区，并以合适的容量单位显示容量大小，显示磁盘文件系统类型
$ df -ahT

// 显示文件系统为 ext4 的磁盘分区
$ df -ahT -t ext4

// 显示文件系统不为 ext4 的磁盘分区
$ df -ahT -x ext4

```

### `du` : 统计磁盘文件大小

参数：
`-b` 以 byte 为单位统计文件大小
`-k` 以 KB 为单位统计文件大小
`-m` 以 MB 为单位统计文件大小
`-h` 以 1024 计算，显示合适的容量单位
`-H` 以 1000 计算，显示合适的容量单位
`-s` 指定统计指标

示例：

```c

// 统计当前目录下的所有文件夹大小
$ du 

// 统计当前目录下 hgy 文件夹的大小
$ du -s hgy/

// 统计根目录下 home 文件夹的大小
$ sudo du -s /home

// 统计根目录下 home 文件夹的大小，以合适的单位显示（1024）
$ sudo du -h -s /home

// 统计根目录下 home 文件夹的大小，以 KB 为单位
$ sudo du -k -s /home
$ sudo du -sk /home

// 统计 docs 目录下名称含有 hgy 的文件夹的大小
$ du sk docs/hgy*/

```

### `fdisk` 磁盘分区

> 注意：Linux 系统中硬件设备都是以文件的形式存在于根目录下的 dev 目录下。
> 硬件设备是由 Linux 自动识别的
> 必须对磁盘进行分区、格式化、挂载后才能使用
> `fdisk` 只能做 MBR 分区

```c

// 显示分区表 
$ fdisk -l
//如果新增了一块硬盘， sda 是第一块硬盘 sdb 是第二块硬盘

// 对 sdb 磁盘进行分区
$ sudo fdisk /div/sdb
/** 输出：
    欢迎使用 fdisk (util-linux 2.31.1)。
    更改将停留在内存中，直到您决定将更改写入磁盘。
    使用写入命令前请三思。

    命令(输入 m 获取帮助)：

    -----------------------------
    输入 m 后输出：
        帮助：

    DOS (MBR)
    a   开关 可启动 标志
    b   编辑嵌套的 BSD 磁盘标签
    c   开关 dos 兼容性标志

    常规
    d   删除分区
    F   列出未分区的空闲区
    l   列出已知分区类型
    n   添加新分区
    p   打印分区表
    t   更改分区类型
    v   检查分区表
    i   打印某个分区的相关信息

    杂项
    m   打印此菜单
    u   更改 显示/记录 单位
    x   更多功能(仅限专业人员)

    脚本
    I   从 sfdisk 脚本文件加载磁盘布局
    O   将磁盘布局转储为 sfdisk 脚本文件

    保存并退出
    w   将分区表写入磁盘并退出
    q   退出而不保存更改

    新建空磁盘标签
    g   新建一份 GPT 分区表
    G   新建一份空 GPT (IRIX) 分区表
    o   新建一份的空 DOS 分区表
    s   新建一份空 Sun 分区表


    命令(输入 m 获取帮助)：

 */

 //然后根据命令提示进行操作即可。

 //操作过程中注意用 p 打印分区表查看效果，确认没错之后输入 w 保存分区。

```

### 硬盘分区模式

* MBR 模式

    主分区不超过 4 个

    单个分区容量最大 2TB 

> 扩展分区的出现主要为了解决 MBR 分区模式中最多只能分 4 个主分区的的问题。

* GPT 模式

    主分区个数几乎没有限制(128个)

    单个分区容量几乎没有限制(18EB)

> GPT 的主分区中，不适合安装 x86(32位) 架构的操作系统。

### `parted` 磁盘分区

```c

// 进入分区工具
$ parted

// 查看帮助
(parted) help

// 选择要分区的磁盘(默认选择的是 sda)
(parted) select /dev/sdc

// 指定分区表类型：[msdos, gpt]
(parted) mklabel gpt

// 查看当前磁盘分区表
(parted) print

// 查看所有磁盘分区表
(parted) print all

// 创建分区 hgy1:命令模式
(parted) mkpart hgy1 1 2000

// 创建分区 hgy2：交互模式
(parted) mkpart
// 根据命令提示输入

// 删除分区
(parted) rm 分区编号

// 退出并保存
(parted) quit

/** 打印示例
(parted) print
Model: VMware, VMware Virtual S (scsi)
磁盘 /dev/sdc: 8590MB
Sector size (logical/physical): 512B/512B
分区表：gpt
Disk Flags: 

数字  开始：  End     大小    文件系统  Name  标志
 1    1049kB  3000MB  2999MB            hgy1
 2    3000MB  5000MB  2000MB  ext4      hgy2
 3    5001MB  7000MB  2000MB            hgy4
 4    7001MB  8000MB  998MB             hgy5

(parted) rm 4
(parted) print
Model: VMware, VMware Virtual S (scsi)
磁盘 /dev/sdc: 8590MB
Sector size (logical/physical): 512B/512B
分区表：gpt
Disk Flags: 

数字  开始：  End     大小    文件系统  Name  标志
 1    1049kB  3000MB  2999MB            hgy1
 2    3000MB  5000MB  2000MB  ext4      hgy2
 3    5001MB  7000MB  2000MB            hgy4
*/


```

### `mkfs` 磁盘格式化

> MBR 和 GPT 类型的分区表都可以使用 mkfs 进行格式化。

> 磁盘每个分区都被系统认为是一个设备，都可以在 /dev 目录下找到。

> MBR 分区表中的扩展分区不能格式化。

> GPT 分区表的类型使用 fdisk 命令看不到

```c

// 将 sdb1 分区格式化，文件系统设置为 ext3
$ mkfs.ext3 /dev/sdb1

```

### `mount` 挂载

> 没有挂载的分区是不能使用的。
> 分区默认的挂载目录是 /mnt ,理论上可以挂载任何位置。
> 挂载的时候必须有挂载点

> 通过 mount 手动挂载的分区当系统重启后挂载会失效

```c
// 创建挂载点
$ mkdir -p /mnt/hgy1

// 挂载
$ mount /dev/sdb1 /mnt/hgy1

// 卸载
$ umount /mnt/hgy1

// 防止重启后挂载失效，在 /etc/fstab 添加一行
/dev/sdb1       /mnt/hgysdb1    ext3    defaults        0       0

```

### `用户和用户组`

用户：使用操作系统的人
用户组：具有相同系统权限的人

```c

// 添加用户组
$ groupadd market

// 查看group
$ cat /etc/group

// 修改用户组名
$ groupmod -n marketD market

```

## 防火墙

```c
$ systemctl stop firewalld.service //停止firewall

$ systemctl disable firewalld.service //禁止firewall开机启动

$ firewall-cmd --state 
//查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
```


