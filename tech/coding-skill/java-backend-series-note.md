# Java 相关编程知识笔记

## ORM

ORM 对象关系模型

## 三层架构

1. 业务层
2. 表现层
3. 持久层

* SSM 框架
  * 业务层：Spring
  * 表现层：SpringMVC
  * 持久层：MyBatis

* SSH 框架
  * 业务层：Spring
  * 表现层：Struts
  * 持久层：Hibernate

## 常见概念名词

* 实体类：entity class。一般都有很多的属性，并有相应的setter和getter方法。实体类的作用一般是和数据表做映射。

* PO： persistant object 持久对象。可以看成是与数据库中的表想映射的 Java 对象，最简单的 PO 就对应数据中某个表的一条记录。PO 中不包含任何对数据库的操作。

* DTO: Data Transfer Object 数据传输对象。用于表示层和服务层之间的数据传输。

* POJO: plain ordinary java object 简单无规则 Java 对象。纯的传统意义的 Java 对象，最基本的 Java Bean，只有属性字段、setter、getter 。

* DAO: data access object 数据访问对象。用于访问数据库，包含各种对数据库的操作，通常和 PO 结合使用。