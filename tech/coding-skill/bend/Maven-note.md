# Maven 学习笔记

Maven 基于项目对象模型（POM）,通过一小段描述信息来管理项目的构建，是一个软件项目管理工具。

[TOC]

## 安装配置

> 注：测试环境 windows 8 / JDK 1.8

### 系统环境变量配置

M2_HOME .../apache-maven-3.5.4

Path ...;%M2_HOME%\bin

检验：在命令行窗口输入 `mvn -v`。 正常情况应当显示 Maven 版本、Java　版本和操作系统版本。


### Maven 项目搭建

* Maven 项目目录结构

```c

-src
  |-main
    |-java
  |-test
    |-java
  |-resources
-pom.xml

```

#### 写一个 HelloWorld 项目：handmaven

##### 编码

* handmaven/src/main/java/com/hgy/test/HelloMaven.java

```java

package com.hgy.test;

public class HelloMaven{
    public String Hello(){
        return "Hello Maven";
    }
}

```

* handmaven/src/test/java/com/hgy/test/TestHelloMaven.java

```java

package com.hgy.test;

import org.junit.*;
import org.junit.Assert.*;

public class HelloMavenTest{
    @Test
    public void testHello(){
        Assert.assertEquals("Hello Maven",new HelloMaven().Hello());
    }
}

```

* handmaven/pom.xml

```xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.hgy.test</groupId>
    <artifactId>handmaven</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
</project>

```

##### 调试

* 编译

打开命令行界面，进入 handmaven 目录，执行 `mvn compile`

* 测试

打开命令行界面，进入 handmaven 目录，执行 `mvn test`

* 打包

打开命令行界面，进入 handmaven 目录，执行 `mvn package`

打包完成之后会得到一个文件 handmaven\target\handmaven-0.0.1-SNAPSHOT.jar

文件内容

```c

├─com
│  └─hgy
│      └─test
            HelloMaven.class
└─META-INF
    └─maven
        └─com.hgy.test
            └─handmaven
                pom.xml
                pom.properties
```

* 清理

> 前面的三个命令都会在 handmaven/target 目录下生成文件，清理命令将删除 target 目录

打开命令行界面，进入 handmaven 目录，执行 `mvn clean`

#### 引用本地项目包

> 新项目 maven01 引用 handmaven 项目包

##### 编码

* maven01\src\main\java\com\hgy\test1\Talk.java

> 这个类调用另一个项目的 HelloMaven 类

```java

package com.hgy.test1;

import  com.hgy.test.HelloMaven;

public class Talk{
    public String SayHi(){
        return new HelloMaven().Hello();
    }
}

```

* maven01\src\test\java\com\hgy\test1\TalkTest.java

```java

package com.hgy.test1;

import org.junit.*;
import org.junit.Assert.*;

public class TalkTest{
    @Test
    public void testTalk(){
        Assert.assertEquals("Hello Maven",new Talk().SayHi());
    }
}

```

* maven01\pom.xml

> 这个文件需要声明对 handmaven 项目的依赖

```xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.hgy.test1</groupId>
    <artifactId>maven01</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>

        <dependency>
            <groupId>com.hgy.test</groupId>
            <artifactId>handmaven</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>

```

##### 调试

* 将 handmaven 项目打包加入本地 maven 仓库

打开命令行界面，进入 handmaven 目录，执行 `mvn install`

* 编译

打开命令行界面，进入 maven01 目录，执行 `mvn compile`

* 测试

打开命令行界面，进入 maven01 目录，执行 `mvn test`

### 使用 archetype 插件

> 用于创建符合 maven 规范的目录骨架

#### 交互模式创建目录骨架

1. 新建项目根目录 maven02

2. 打开命令行界面，进入 maven02 目录，执行 `mvn archetype:genrate`

3. 然后按照命令提示输入 `groupId` `artifactId` `version` 等信息。

4. 命令执行完将生成目录骨架和 pom.xml 文件

#### 命令模式创建目录骨架

1. 新建项目根目录 maven02

2. 打开命令行界面，进入 maven02 目录，执行

```c
mvn archetype:generate -DgroupId=com.hgy.test2 -DartifactId=maven02 -Dversion=1.0.0-SNAPSHOT -Dpackage=com.hgy.test2
```

## Maven 标识和仓库

* Maven 中所有 java 库都有唯一的标识（groupId、artifactId 和 version 组成）

* 仓库用于管理项目依赖的 java 库。

* 仓库分为本地仓库和远程仓库。

* 全球中央仓库（The Central Repository）配置于maven*/lib/maven-model-builder*.java/pom*.xml。中央仓库包含了绝大多数开源 java 库。

* Maven 中央仓库服务器位于国内访问不便，可用国内镜像仓库。

* 镜像仓库设置文件在 maven*/conf/settings.xml: `<mirrors>` 标签

* 从远程仓库下载的 java 库和本地使用 install 添加的库都会放到本地仓库中。

* 本地仓库默认是用户目录，可在 maven*/conf/settings.xml: `<localRepository>` 修改。


## Eclipse 配置 Maven

> 注：测试用 Eclipse 版本号：Kepler Service Release 2

* Preference -> Java -> Installed JREs，添加 JDK 目录并勾选。

* 双击上一步添加的 JDK 项，设置Default VM arguments 为 -Dmaven.multiModuleProjectDirectory=$M2_HOME

* Preference -> Maven -> Installations，添加 maven 目录并勾选。

* Preference -> Maven -> User Settings，选择 maven 目录下的 settings.xml 文件

### 调试

1. Eclipse 新建 Maven 项目。
2. 右击项目 -> Run As -> Maven build...，在 Goals 输入框中输入 compile ，单击 Run，即开始编译。
3. package \ install \ clean \ test 命令也可用此方式执行。

## Maven 项目生命周期

* 完整的项目构建过程包括 清理、编译、测试、打包、集成测试、验证、部署...

* Maven 项目的声明周期：

  * clean 清理项目

  * default 构建项目

  * site 生成项目站点

* clean 清理项目的三个阶段：

  * pre-clean 执行清理前的工作

  * clean 清理上一次构建所生成的文件

  * post-clean 执行清理后的工作

* default 构建项目的主要阶段

  * compile

  * test

  * package

  * install

* site 构建项目的主要阶段

  * pre-site 处理生成项目站点之前要完成的工作

  * site 生成项目的站点文档

  * post-site 处理生成项目站点之后要完成的工作

  * site-deploy 发布生成的站点到服务器

## pom.xml

* pom.xml 是 Maven 项目的核心管理文件，用于项目描述、组织管理、依赖管理和构建信息管理。






