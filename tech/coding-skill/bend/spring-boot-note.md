# Spring Boot 学习笔记

## Spring Boot 与微服务

| 传统应用 | 微服务应用 |
| --- | --- |
| 功能、模块堆砌 | 功能、模块拆分为独立项目，项目之前通过接口通信 | 

* Spring Boot 可以快速开发微服务：
  * 可以简化 JavaEE 开发
  * 可以整合整个 Spring 技术栈（Spring SpringMVC，SPringCloud...）
  * 可以整合所有 Java 技术栈。

## 环境搭建

1. JDK 安装：  
  * JAVA_HOME: jdk 根目录
  * Path: %JAVA_HOME%\bin
  * classpath: .; %JAVA_HOME%\lib

2. Maven 安装：
  * MAVEN_HOME：Maven 根目录
  * Path：%MAVEN_HOME%\bin
  * 开发工具中配置 Maven 
    * Eclipse(或者 STS) :Window->Preference->Maven: Installations 和 User Settings

3. IDE 安装/配置
  * STS
  * Eclispe + STS
  * ItelliJ IDEA

## Spring Boot 注解

* `@SpringBootApplication` 主配置类。
