# Git 版本控制工具学习笔记

## Git 常识

* Git 跟踪管理的是修改，而非文件。文件增删、文件内容的增删改都算是修改。
* 版本越多越好。有备份作用，改动也方便。
* Git 的本地有一个仓库，在本地可以进行版本管理,不联网也可以进行版本管理。这是和 SVN 不同的地方之一。
* Git 支持多种协议。https 速度慢而且每次推送都要输入密码。原生的 git 协议速度最快。

---

* 分支的用途：可以提交代码进行版本管理，同时不影响别的分支。
* HEAD 指的是当前活跃分支的游标，可以用 checkout 命令改变 HEAD 的指向位置。origin 是默认的远程仓库名字。master 是创建仓库时默认的分支名字，一般把 master 分支作为主分支。

---

* 分支策略：master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；干活都在dev分支上，dev分支是不稳定的，到版本发布时，再把dev分支合并到master上，在master分支发布版本；项目每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并。
* Bug 分支：每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
* feature 分支：开发一个新feature，最好新建一个分支。

---

* 分支推送：
  * master 分支是主分支，因此要时刻与远程同步；
  * dev 分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
  * bug 分支只用于在本地修复 bug，没必要推到远程，除非管理上需要看 bug 修复进度。
  * feature 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

---

* 多人协作的工作模式通常是这样：

  * 首先，可以试图用 `git push origin <branch-name>` 推送自己的修改；
  * 如果推送失败，则因为远程分支比你的本地更新，需要先用 `git pull` 试图合并；
  * 如果合并有冲突，则解决冲突，并在本地提交；
  * 没有冲突或者解决掉冲突后，再用 `git push origin <branch-name>` 推送就能成功！
  * 如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream-to <branch-name> origin/<branch-name>` 。

---

* 标签管理：发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

## Git 命令

### 基本

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
$ ssh-keygen -t rsa -C "youremail@example.com"
// 这个命令会生成一对密钥（私钥和公钥），私钥通常在保存在 ~/.ssh/id_rsa，公钥通常保存在 ~/.ssh/id_rsa.pub。
// 当需要向远程仓库推送修改时，需要验证身份。 GitHub 和 码云都是利用 SSH Key 来做身份验证的。只要远程仓库的 SSH Key 列表有推送人的公钥，就可以提交。



// 创建仓库
// 使用当前目录作为仓库
$ git init
// 注释原文： Create an empty Git repository or reinitialize an existing one

// 把文件变更添加到暂存区// 把要提交的修改放到暂存区，等待执行commit时一次性提交。
// git add 命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支
$ git add <file>

// 把暂存区的所有内容提交到仓库 commit 针对的是暂存区，也就是只提交被 add 过的修改。
$ git commit -m <message>

// 删除文件
$ git rm <file> // 这一步会删除工作区的文件并将修改添加到暂存区。如果再执行一下 commit ，那么版本库将删除这个文件。


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

```


### Git 的整体架构

![git 的整体架构](../images/git-pull-fetch.jpg)

- 工作区（working directory）：本地工作目录

- 暂存区（stage area, 又称为索引区）：用于标记将要提交的内容。

- 本地仓库（local repository）

- 远程仓库（remote repository）

- 远程仓库副本：可以理解为存在于本地的远程仓库缓存。使用fetch 获取时，并未合并到本地仓库，此时可使用 git merge 实现远程仓库副本与本地仓库的合并。 

### 版本回退

```c

//回退到上一个版本
$ git reset --hard HEAD^

// 回退到前N个版本
$ git reset --hard HEAD~N

// 恢复到指定版本（过去和未来版本都行）
$ git reset --hard <commit ID> // 每一个commit都有唯一的id

```

### 撤销修改

```c
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

```

### 修改文件名

```c
// 示例：修改文件名

$ git mv Maven-noet.md Maven-note.md

$ git status -s
//R  Maven-noet.md -> Maven-note.md
//Git在文件名之前显示R，表示文件已被重命名

$ git commit -m "更正文件名"
/*
[master 438c814] 更正文件名
1 file changed, 0 insertions(+), 0 deletions(-)
rename tech/coding-skill/bend/{Maven-noet.md => Maven-note.md} (100%)
*/

```

### 远程库操作

```c
// 克隆仓库，默认克隆的是 master 分支，
$git clone <repo>
/*
 Git 的工作流程一般是
 1. 克隆仓库
 2. 添加或修改文件
 3. 查看他人的修改，合并文件
 4. 提交文件、撤回提交
 ...
*/

// 关联远程仓库
$ git remote add origin <url>

// 切断远程仓库关联
$ git remote rm origin

// 查看远程仓库信息
$ git remote -v

// 取回远程仓库分支的内容(与当前分支合并)
// git pull <远程主机名> <远程分支名>:<本地分支名>
$ git pull origin master

// 推送本地仓库master分支的更新到远程仓库
$ git push origin master

/*********************************** 
 * 使用 git fetch 合并分支
 *
 */
git fetch origin master:tmp 
//在本地新建一个temp分支，并将远程origin仓库的master分支代码下载到本地temp分支
git diff tmp 
//来比较本地代码与刚刚从远程下载下来的代码的区别
git merge tmp
//合并temp分支到本地的master分支
git branch -d temp
//如果不想保留temp分支 可以用这步删除

```

### 分支操作

```c
// 创建分支并切换分支
$ git checkout -b dev 
// 等同于 
// git branch dev  创建分支
// git checkout dev 切换分支

// 创建远程origin的dev分支到本地
$ git checkout -b dev origin/dev

//指定本地dev分支与远程origin/dev分支的链接
$ git branch --set-upstream-to=origin/dev dev

// 查看分支
$ git branch

// 查看远程仓库分支
$ git branch -r

// 推送本地的 dev 分支到远程的 dev 分支（如果远程库没有会自动创建）
$ git push origin dev:dev

// 合并指定分支到当前分支
$ git merge <branchName>
// 当当前分支和被合并分支出现冲突时，Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容。可以手动修改冲突文件再提交。 
// 合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息，看不出来曾经做过合并。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

// 合并分支，禁用Fast forward模式
$ git merge --no-ff -m "commit messge" <branchName>

// 查看分支合并图
$ git log --graph

// 删除分支
$ git branch -d <branchName>

// 给分支打补丁：引入其他分支的提交
// 如项目在并行开发两个版本的程序，分支为：App1-dev App2-dev
// App1-dev 完成了一个功能X开发，想应用到 App2-dev。
// 一种方式是手动修改文件
// 另一种方式是使用cherry-pick将功能X的的提交作为一个新的提交引入 App2-dev 分支
// cherry-pick 命令执行后可能会有冲突，手动处理冲突后在add 、 commit之后，cherry-pick 才算完成。
$ git cherry-pick f4381b0c95b123

// pick 区间(idx1, idx2]之间的提交当当前分支
$ git cherry-pick idx1..idx2

// pick 区间[A, B]之间的提交当当前分支
$ git cherry-pick A^..B


// 将当前分支的当前状态保存。当前分区工作未完成，需要切换到其他分支临时处理事情是需要这么做。
$ git stash 

// 查看保存的状态
$ git stash list

// 恢复状态
$ git stash pop 
// 恢复，同时删除 stash 内容
$ git stash apply // 恢复，不删除 stash 内容

```

### 标签操作

```c
// 添加标签
$ git tag <version>
$ git tag <version> <commitId>
$ git tag -a <version> -m "version description" <commitId>

// 查看所有标签
$ git tag

// 查看指定标签
$ git show <version>

// 删除标签
$ git tag -d <version>

// 推送标签到远程
$ git push origin <version> // 推送一个
$ git push origin --tags // 推送全部

// 删除远程标签
$ git tag -d <version>
$ git push origin :refs/tags/<version>

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

## GitHub

* GitHub是一个面向开源及私有软件项目的托管平台，因为只支持git 作为唯一的版本库格式进行托管，故名 GitHub。
* GitHub于 2008 年 4 月 10 日正式上线，除了 Git 代码仓库托管及基本的 Web管理界面以外，还提供了订阅、讨论组、文本渲染、在线文件编辑器、协作图谱（报表）、代码片段分享（Gist）等功能。
* GitHub 还是一个开源协作社区。
* 在GitHub上，可以任意Fork开源仓库；自己拥有 Fork 后的仓库的读写权限；可以推送 pull request 给官方仓库来贡献代码。

## 码云

* 码云是开源中国社区 2013 年推出的基于 Git 的完全免费的代码托管服务，这个服务是基于 Gitlab 开源软件所开发的。

* 码云除了提供最基础的 Git 代码托管之外，还提供代码在线查看、历史版本查看、Fork、Pull Request、打包下载任意版本、Issue、Wiki 、保护分支、代码质量检测、PaaS项目演示等方便管理、开发、协作、共享的功能。


## 参考资料

1. [Git 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

2. [图解 Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html)

3. [Git Cheat Sheet](https://www.git-tower.com/blog/git-cheat-sheet/)

4. [谈谈版本管理Git之理论与架构](https://zhuanlan.zhihu.com/p/59591617)