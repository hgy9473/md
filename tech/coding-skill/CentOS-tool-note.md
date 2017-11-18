# CentOS 工具软件使用笔记 


## CentOS vi 使用笔记
---
> vi 是面向屏幕的文本编辑器，最初是为 Unix 操作系统创造的。
```c
// 使用 vi 打开文件
$ vi a.js

// 退出 vi
// 退出 不保存
:q
// 退出 并保存
:wq

// 打开文件并修改
$ vi a.js
//进入插入模式，初次打开文件是命令行模式
i  
// 移动光标、增减字符

//  撤销
// 进入命令行模式
ESC
// 行撤销
U
// 撤销
u
// 回退
CTRL + R

```

## CentOS yum 使用笔记
---
> yum 是 rmp 系统的软件安装、更新、卸载工具。它会自动计算依赖关系并决定在安装的时候应该额外安装什么。它使得更新一组机器非常容易，不用手动使用 rpm 去更新每一台机器。
参考： http://yum.baseurl.org/

- 常用命令
```c
// 查看可安装软件清单
$ yum list

// 查看指定软件安装情况
$ yum list package-name

// 查看可更新软件
$ yum check-update

// 查看安装包信息
$ yum info package-name

// 安装软件
$ yum install package-name

// 查找软件包
$ yum search package-name

```

## CentOS pip 使用笔记
---
> pip 是一个包管理系统，用于安装和管理用 Python 写的软件包。

### 常用命令
```c
// 查看 pip 版本
pip -V

// 查看已安装的包
pip list

// 安装 django
pip install django==1.9

// 查看可更新的包
pip list --

// 查找包并查看已安装的版本
pip search package-name


```

## CentOS rpm 笔记
---
> RPM(Red Hat Package Manager) 是在 Linux 下广泛使用的软件包管理器。最早由 Red Hat 研制。 RPM 仅适用于安装用 RPM 来打包的软件。