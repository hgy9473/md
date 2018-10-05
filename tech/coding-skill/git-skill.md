# Git 版本控制工具使用技巧

## 通用

* Git 跟踪管理的是修改，而非文件。文件增删、文件内容的增删改都算是修改。
* Git 的本地有一个仓库，在本地可以进行版本管理。这是和 SVN 不同的地方之一。


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

// 生成 SSH Key


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

// 把文件变更添加到暂存区// 把要提交的修改放到暂存区，等待执行commit时一次性提交。
// git add 命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支
$ git add <file>

// 把暂存区的所有内容提交到仓库 commit 针对的是暂存区，也就是只提交被 add 过的修改。
$ git commit -m <message>

// 查看文件具体修改了什么 ，diff 可以实现工作区、暂存区、版本库分支、任一提交版本之间相互比较。
$ git diff <option> <file>
$ git diff  <file> //工作区与暂存区比较
$ git diff HEAD <file> //工作区与HEAD ( 当前工作分支) 比较
$ git diff --staged 或 --cached  <file> //暂存区与HEAD比较
$ git diff branchName <file>  //当前分支的文件与branchName 分支的文件进行比较
$ git diff commitId <file> //与某一次提交进行比较

// 查看文件状态
$ git status
/*
 git status 有 4 种提示：
 1. Untrackd files: 文件没有添加到暂存区过，版本系统没有跟踪文件变更。
 2. Changes not staged for commit: 文件被修改了，还没有添加到暂存区
 3. Changes to be commited: 文件被修改了，已经添加到暂存区，等待提交到仓库。
 4. nothing to commit, working tree clean: 没有文件被修改。此时暂存区是空的，工作区和文件和分支当前版本一样。


*/

// 查看版本提交日志
$ git log
$ git log --pretty=oneline  // 简化查看日志
$ git reflog // 查看所有操作记录（包括已经删除的commit和reset操作）git log则是看不出来被删除的commitid



// 版本回退
//回退到上一个版本
$ git reset --hard HEAD^

// 回退到前N个版本
$ git reset --hard HEAD~N

// 恢复到指定版本（过去和未来版本都行）
$ git reset --hard <commit ID> // 每一个commit都有唯一的id


// 撤销修改
/*
 撤销修改有 4 种情况
 1. 修改还没有添加到暂存区
 2. 修改已经添加到暂存区
 3. 修改已经提交，这种情况可以利用版本回退。
 4. 修改已经提交并推送到远程仓库，这种情况下修改已经写入历史，无法撤销了。
*/

// 撤销还没有添加到暂存区的修改，一种方式是手动修改，一种是使用git让文件回到最近一次commit时的状态。
$ git checkout -- <file>

// 撤销已经添加到暂存区的修改，需要两步：先清理暂存区，再清理工作区
$ git reset HEAD <file> // 这个操作实际上是移除了暂存区的记录，工作区没受到影响,工作区的文件还是最后一次被编辑完的状态。执行完这一步之后还要执行 git checkout -- <file> 才算撤销完成。


// 删除文件
$ git rm <file> // 这一步会删除工作区的文件并将修改添加到暂存区。如果再执行一下 commit ，那么版本库将删除这个文件。







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