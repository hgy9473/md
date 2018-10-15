# Mybatis 笔记

* iBatis = "internet" + "abatis" 

* iBATIS 是一个基于 Java 的持久层框架。

* 持久层：Java 中的对象有两种状态：瞬态、持久态。瞬态：对象新建之后被回收，状态和属性没有保持。持久态：对象新建之后状态和属性一直保持。

* JDBC 操作数据库不是很方便，于是产生了持久层框架。

* iBatis 后来改名为 MyBatis。

* MyBatis 的特点：
  * 开源、轻量
  * SQL 语句和 Java 代码分离
  * 面向配置的编程
  * 支持复杂数据映射
  * 动态 SQL 技术

* MyBatis 是对 JDBC 的封装

## 原生态 JDBC 程序中存在的问题

> 使用 jdbc 的一般流程：数据库连接、预编译Statement、获取结果集，释放结果集、释放预编译、释放数据库连接。

1. 数据库连接：使用时创建、使用完立即释放。连接频繁、浪费资源、影响性能。解决方案：使用数据库连接池来管理数据库连接。
2. SQL 语句硬编码到 Java 代码，SQL 已修改就需要重新编译。不利于系统维护。解决方案：将 SQL 配置在 XML 文件。
3. preparedStatement 语句中的占位符和参数也是硬编码。
4. 从 resultSet 中遍历结果集数据时也存在硬编码。解决方案：结果集映射成 Java 对象。

## MyBatis 简介

MyBatis 是一个持久层的框架，起源于 Apache 的一个顶级项目。后来托管到 GoogleCode，再后来托管到了 GitHub。 

MyBatis 让程序员将主要精力放到 SQL 上，通过 MyBatis 提供的映射方式，自由灵活生成（半自动化）满足需要的 SQL 语句。

MyBatis 可以将 SQL 参数（preparedStatement参数）自动进行输入映射，可以将查询结果集进行输出映射（映射成 Java 对象）。

SqlMapConfig.xml(MyBatis 的全局配置文件)，配置了数据源、事务等 MyBatis 运行环境。配置了映射文件（mapper.xml）。 

SqlSessionFactory，用于创建 SqlSession。

SqlSession，用于发出 SQL 操作（增删改查）

Executor，SqlSession 内部通过 Executor 来操作数据库。

mapped statement，对数据进行存储、封装，包括 SQL 语句、输入参数、输出结果。


## Hibernate 存在的问题

> Hibernate 是一个全自动的 ORM 框架。

1. 消除了 SQL，SQL 优化不方便，SQL 定制不方便。
2. 全映射。

## MyBatis 安装包目录结构

```c
mybatis-3.2.7
    lib // MyBatis 依赖的包
    mybatis-3.2.7.jar //MyBatis 核心包
```

> 使用 MyBatis 还需要数据库驱动包

## 配置

```python
# file： config/log4j.properties

# Global logging configuration
# 全局日志配置：开发环境设置成 DEBUG，生产环境设置成 info或error
log4j.rootLogger=DEBUG, stdout
# MyBatis logging configuration...
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

```


## 参考资料

[1] 《SpringMVC+Mybatis由浅入深全套视频教程》.爱酷.www.icoolxue.com
[2] 《尚硅谷MyBatis视频教程》
[3] 《初识 MyBatis》.极客学院.www.jikexueyuan.com



