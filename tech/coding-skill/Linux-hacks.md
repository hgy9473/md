
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

```c
// ^ 匹配行首
# grep "^c" a
cat 
computer

// $ 匹配行尾
# grep "e$" a
applce
juice
orange


// ^$ 匹配空行
// 统计空行数量
# grep -c  "^$" a
10

// . 匹配一个字符
// * 匹配零个或者多个前一个字符
// 

```

### find 命令

> find 常常用于查找文件
> 语法：find [pathnames] [conditions]

```c
// 查找 /etc 路径下 名字带有 mail 的文件
# find /etc -name "*mail*"

// 查找当前目录下 60 天以前修改的文件
# find . -mtime +60

// 查找系统中大小超过 100M 的文件
# find / -type f -size +100M

```

### 控制标准输出和错误信息输出
```c
# cat file.txt > /dev/null


# cat invalid-file-name.txt 2> /dev/null

```

### join 指令
```c
$ cat employee.txt
100 Jason Smith
200 John Doe
300 Sanjay Gupta
400 Ashok Sharma


$ cat bonus.txt
100 $5,000
200 $500
300 $3,000
400 $1,250


$ join employee.txt bonus.txt
100 Jason Smith $5,000
200 John Doe $500
300 Sanjay Gupta $3,000
400 Ashok Sharma $1,250

```

### 转换大小写

```c
$ cat employee.txt
100 Jason Smith
200 John Doe
300 Sanjay Gupta
400 Ashok Sharma

$ tr a-z A-Z < employee.txt
100 JASON SMITH
200 JOHN DOE
300 SANJAY GUPTA
400 ASHOK SHARMA

$ cat department.txt
100 FINANCE
200 MARKETING
300 PRODUCT DEVELOPMENT
400 SALES

$ tr A-Z a-z < department.txt
100 finance
200 marketing
300 product development
400 sales
```

### xargs 命令

