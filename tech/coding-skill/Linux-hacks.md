
# Linux 技巧

## cd 命令

### 1. CDPATH

CDPATH环境变量可用于为cd命令定义基本目录。

```c
// ~/.bash_profile
export CDPATH=.:~:/etc:/var 
```

### 2. alias

为长指令定义别名

```c
// ~/.bash_profile
alias ..="cd .."
alias ..2="cd ../.."
alias ..3="cd ../../.."
alias ..4="cd ../../../.."
alias ..5="cd ../../../../.." 

// other
alias ..="cd .."
alias ...="cd ../.."
alias ....="cd ../../.."
alias .....="cd ../../../.."
alias ......="cd ../../../../.."

//
alias cd..="cd .."
alias cd...="cd ../.."
alias cd....="cd ../../.."
alias cd.....="cd ../../../.."
alias cd......="cd ../../../../.."

// 
alias cd1="cd .."
alias cd2="cd ../.."
alias cd3="cd ../../.."
alias cd4="cd ../../../.."
alias cd5="cd ../../../../.."
```

### mkdir cd 联合
```c
// vi .bash_profile
function mkdircd () { mkdir -p "$@" && eval cd "\"\$$#\"";
}

```

### 在最近的两个目录中切换

```c

cd -

```

### 目录栈的使用

```c

/*
dirs: Display the directory stack
pushd: Push directory into the stack
popd: Pop directory from the stack and cd to it
*/

# popd
-bash: popd: directory stack empty

# dirs
~

# cd /tmp/dir1
# pushd .
# cd /tmp/dir2
# pushd .
# cd /tmp/dir3
# pushd .
# cd /tmp/dir4
# pushd .

# dirs
/tmp/dir4 /tmp/dir4 /tmp/dir3 /tmp/dir2 /tmp/dir1

# popd
# pwd
/tmp/dir4

# popd
# pwd
/tmp/dir3

# popd
# pwd
/tmp/dir2

# popd
# pwd
/tmp/dir1

# popd
-bash: popd: directory stack empty
```

### 自动纠正打字错误

```c
# cd /etc/mall
-bash: cd: /etc/mall: No such file or directory

# shopt -s cdspell

# cd /etc/mall

# pwd
/etc/mail
```

## 重要的 Linux 命令

### grep 搜索命令

> 语法 : grep [options] pattern [files]

```c
// 搜索文件内容
# grep John /etc/passwd
jsmith:x:1082:1082:John Smith:/home/jsmith:/bin/bash
jdoe:x:1083:1083:John Doe:/home/jdoe:/bin/bash

// 搜索文件内容，显示不匹配的
# grep -v John /etc/passwd
jbourne:x:1084:1084:Jason Bourne:/home/jbourne:/bin/bash

// 显示行数
# grep -c John /etc/passwd
2
# grep -cv John /etc/passwd
39


// 忽略大小写
# grep -i john /etc/passwd
jsmith:x:1082:1082:John Smith:/home/jsmith:/bin/bash
jdoe:x:1083:1083:John Doe:/home/jdoe:/bin/bash

// 搜索目录以及字母录下的所有文件
grep -r  a ./
./a:how are you?
./a:rain 
./a:cat 
./a:applce
./a:banana
./a:orange
./a:weather

// 只显示文件名
grep -rl  a ./
./a

```

### 在 grep 中使用正则表达式

