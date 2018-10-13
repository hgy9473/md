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

  > Gradle 兼容 Maven

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

## Gradle 项目目录结构

```c

├─build
├─build.gradle
└─src
    ├─main
    │  ├─java
    │  ├─resources
    │  └─webapp
    └─test
        ├─java
        └─resources


```

## Gradle 概念

* 一个项目代表一个正在构建的组件。
* 当构建启动后，Gradle 会基于 build.gradle 实例化一个 org.gradle.api.Project 类。build.gradle 文件中的每一项都在 org.gradle.api.Project 中有对应的方法。
* 项目的标识（坐标）：version、name、group 
* `apply` 声明要应用的插件 `dependencies` 声明项目依赖 `repositories` 声明放置依赖的仓库 `task` 声明项目的任务

## 示例：自定义创建目录结构的任务

```groovy

def createDir = {
    path ->
        File dir = new File(path);
        if(!dir.exists()){
            dir.mkdirs();
        }
}

task mkJavaGradleDir() {
    def paths = [
            'src/main/java',
            'src/main/resources',
            'src/test/java',
            'src/test/resources'
            ]
    doFirst{
        paths.forEach(createDir);
    }

}

task mkWebGradleDir(){
    dependsOn 'mkJavaGradleDir'
    def paths = [
            'src/main/webapp',
            'src/main/webapp/WEB-INF',
            'src/test/webapp'
    ]

    doLast{
        paths.forEach(createDir)
    }

}

```

## 构建生命周期

1. 初始化

2. 配置：生成 task 的执行顺序。

3. 执行

