# Git 版本控制工具使用技巧

## 通用
1. 版本越多越好。有备份作用，改动也方便。

## github.com



## github Desktop


# 命令行

## Git 服务器搭建

> 环境：Ubuntu 18.04 

```c

// 安装
$ sudo apt install git

// 新建账户
$ sudo adduser git

// 创建证书目录。用于放置所有需要登录的用户的公钥
$ cd /home/git
$ sudo mkdir .ssh
$ sudo chmod 755 .ssh
$ sudo touch .ssh/authorized_keys
$ sudo chmod 644 .ssh/authorized_keys

// 创建仓库目录
$ sudo mkdir gitrepos
$ sudo chown git:git gitrepos/

// 创建仓库
$ cd gitrepos/
$ sudo git init --bare test0.git
$ sudo chown -R git:git test0.git

// 配置openssh 和 防火墙
$ sudo apt install openssh-server
$ sudo ufw allow ssh

// 客户端克隆仓库 Git Bash 
$ git clone git@192.168.188.133:/home/git/gitrepos/test0.git
// 提交文件
$ git add index.html
$ git commit -m "add a file"
$ git push -u origin master



```