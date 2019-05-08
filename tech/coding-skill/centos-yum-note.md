# CentOS yum 使用笔记

> yum 是 rmp 系统的软件安装、更新、卸载工具。它会自动计算依赖关系并决定在安装的时候应该额外安装什么。它使得更新一组机器非常容易，不用手动使用 rpm 去更新每一台机器。
参考： http://yum.baseurl.org/

## 常用命令

```c
// 查看可安装软件清单
$ yum list

// 查看指定软件安装情况
$ yum list package-name

// 查看已安装的软件
$ yum list installed
$ yum list installed | grep mysql // 查看 mysql 是否安装

// 查看可更新软件
$ yum check-update

// 查看安装包信息
$ yum info package-name

// 安装软件
$ yum install package-name

// 查找软件包
$ yum search package-name


```