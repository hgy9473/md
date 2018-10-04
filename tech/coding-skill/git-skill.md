# Git 版本控制工具使用技巧

## 通用
1. 版本越多越好。有备份作用，改动也方便。

## github.com



## github Desktop


# Git 命令

```c
// 查看版本
$ git --version

// 配置
/*
git 的配置分为
  系统级（git config -system）
  用户级（git config -global）
  仓库级（git config -local）

  下级配置会覆盖上级配置
*/
// 配置用户
$ git config --global user.name "hgy"
$ git config --global user.email "1527947721@qq.com"
// 查看配置
$ git config --list

// 基本概念及命令
/*
工作区：正在编辑的文件的目录/文件夹
暂存区（stage/index）：git add 命令的意思是 add file contents to the index. 就是把文件暂存到暂存区。把文件添加到暂存区之后Git才会跟踪文件的变化。
版本库：本地仓库。git commit 命令的意思是 Record changes to the repository. 就是把暂存区的内容写入本地仓库。
远程仓库：通过网络和本地通信的仓库。git push 的作用就是把本地仓库的变更推送到远程仓库。

*/

// 创建仓库
// 使用当前目录作为仓库
$ git init
// 注释原文： Create an empty Git repository or reinitialize an existing one

// 克隆仓库
$git clone <repo>
/*
 Git 的工作流程一般是
 1. 克隆仓库
 2. 添加或修改文件
 3. 查看他人的修改，合并文件
 4. 提交文件、撤回提交
 ...
*/

// 把文件变更添加到暂存区// 把要提交的修改放到暂存去，等待执行commit时一次性提交。
$ git add <file>

// 把暂存区的所有内容提交到仓库
$ git commit -m <message>

// 查看文件具体修改了什么
$ git diff <file>

// 查看文件状态
$ git status
/*
 git status 有 4 种提示：
 1. Untrackd files: 文件没有添加到暂存区过，版本系统没有跟踪文件变更。
 2. Changes not staged for commit: 文件被修改了，还没有添加到暂存区
 3. Changes to be commited: 文件被修改了，已经添加到暂存区，等待提交到仓库。
 4. nothing to commit, working tree clean: 没有文件被修改。

*/




```

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