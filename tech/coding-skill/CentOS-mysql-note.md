# CentOS 7 MySQL 笔记

## 安装
```c
// 下载安装包
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

// 给 yum 指定本地安装包
yum localinstall mysql57-community-release-el7-8.noarch.rpm

// 安装 本地安装 + 在线安装（安装依赖包）
yum install mysql-community-server

```
2. 修改密码
```c

// 启动 MySQL 服务
systemctl start mysqld

// 查看密码 mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码
grep 'temporary password' /var/log/mysqld.log

// 登录
mysql -u root -p

// 修改密码 
// MySQL 默认要求密码包括大小写字母、数字、特殊字符、不少于8位
set password for 'root'@'localhost'=password('Hgy12345&%');
```

3. 配置默认编码为 utf-8
```
vi /etc/my.cnf
```

## 基本命令
```c
// 启动服务
systemctl start mysqld

// 关闭服务
systemctl stop mysqld

// 查看状态
systemctl status mysqld

// 登录
mysql -uroot -p

// 退出 mysql
mysql> quit

```

## 连接 Navicat for MySQL
https://blog.csdn.net/szu_Vegetable_Bird/article/details/80216201
https://www.linuxidc.com/Linux/2017-01/139622.htm
https://blog.csdn.net/xuexiaoxu1990/article/details/52487571
https://blog.csdn.net/wailaizhu/article/details/53488954

## 权限管理

https://www.cnblogs.com/SQL888/p/5748824.html
