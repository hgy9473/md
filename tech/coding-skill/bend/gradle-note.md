# Gradle 学习笔记

jar 包：成百上千（各个版本的、有用的没用的）
测试：能不写就不写。
打包：用Eclipse 生成 war jar
部署：FTP 上传

* 构建工具的作用：
  * 管理 jar 包
  * 自动测试、打包、发布

* 主流构建工具
  * Ant 编译、测试、打包
  * Maven +依赖管理、发布
  * Gradle +Groovy

> Gradle 是一个开源的项目自动化构建工具，建立在 Apache Ant 和 Apache Maven 概念的基础上，并引入了基于 Groovy 的特定领域语言（DSL）,不再使用 XML 来管理构建脚本。

## 安装 Gradle

> 安装 Gradle 之前需要安装 JDK 并配置环境变量 (JAVA_HOME 和 Path)

* 下载 gradle-4.10.2-all.zip，解压
* 配置环境变量 GRADLE_HOME -> 解压文件的目录
* 系统 Path 变量添加 %GRADLE_HOME%\bin;
* 验证是否安装成功：命令行输入 gradle -v

## Groovy

> Groovy 是用于 Java 虚拟机的一种敏捷的动态语言，它是一种成熟的面向对象编程语言，既可以用于面向对象编程，又可以用作纯粹的脚本语言。使用这种语言不需要编写过多的代码，同时又具有闭包和动态语言汇总的其他特性。

> Groovy 兼容 Java 语法。分号可选。类、方法默认 public。属性自动添加setter、getter。最后一个表达式的值作为返回值。

> == 等同于 equals()


