# CentOS 7 MySQL 笔记

## 安装
```c
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

yum localinstall mysql57-community-release-el7-8.noarch.rpm

yum install mysql-community-server

```
2. 修改密码
```c

// 启动 MySQL 服务
systemctl start mysqld

// 查看密码 mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码
grep 'temporary password' /var/log/mysqld.log

// 登录
mysql -uroot -p

// 修改密码
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
