# Linux 中使用 Tomcat 笔记

## 安装配置 Tomcat

* 安装 JDK

```c
// 通过 yum 安装 OpenJDK
$ yum install java-1.8.0-openjdk-devel

// 配置环境变量
$ vim /etc/profile.d/java.sh
//---------------------------java.sh
    export JAVA_HOME=/usr/java/latest   # 首先定义JAVA_HOME的环境变量
    export PATH=$JAVA_HOME/bin:$PATH   # 然后向后追加即可
//---------------------------java.sh
```



https://www.linuxidc.com/Linux/2018-02/151085.htm
