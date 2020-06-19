
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
```
