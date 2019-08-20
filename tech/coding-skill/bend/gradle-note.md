# Gradle 学习笔记

* 原生开发：
  * jar 包：成百上千（各个版本的、有用的没用的）
  * 测试：能不写就不写。
  * 打包：用Eclipse 生成 war jar
  * 部署：FTP 上传

* 构建工具的作用：
  * 管理 jar 包
  * 自动测试、打包、发布

* 主流构建工具
  * Ant： 编译、测试、打包
  * Maven： Ant + 依赖管理、发布
  * Gradle: Maven + Groovy

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

## 依赖管理

> 几乎所有的基于 JVM 的软件项目都需要依赖外部类库来重用现有功能。自动化的依赖管理可以明确外部类库的版本，可以解决因传递性依赖带来的版本冲突。

> gradle 没有自己的公共仓库，需要使用 Maven 的仓库。


> 工件坐标：group、name、version

* 常用仓库： 
  * 公共仓库（中心仓库）：mavenCentral、jcenter
  * 本地仓库：mavenLocal
  * 自定义(私有) maven 仓库
  * 文件仓库：本地机器上的文件路径也可以作为仓库。不常用。不推荐使用。

* 依赖的传递性：如果 A 依赖 B , B 依赖 C ,那么 A 依赖 C

* 依赖的阶段：compile、runtime、testCompile、testRuntime
```c
compile 
	用来编译项目源代码的依赖
runtime
	在运行时被生成的类使用的依赖。 默认的, 也包含了compile时的依赖。
testCompile
	编译测试代码的依赖。 默认的, 包含runtime时的依赖和compile时的依赖。
testRuntime
	运行测试所需要的依赖。 默认的, 包含上面三个依赖。
```

* 依赖配置

```groovy

// 设置依赖下载仓库
// 可填写：mavenLocal() mavenCentral() maven{ url:'私有库地址' }
repositories {
    mavenCentral()
}

// 配置依赖
dependencies {
    testCompile group: 'junit', name: 'junit', version: '4.12'
    compile 'ch.qos.logback:logback-classic:1.2.1'
}
// 依赖格式有两种：
// "group:name:version"
// group:"",name:"",version:""

```

## 依赖本地 jar 包

Gradle也可以从本地目录中引入JAR包依赖，可以单一引入指定的某一JAR包，也可以引入某目录下所有的JAR包

```groovy
dependencies {
	compile files('dir/file.jar')
	compile fileTree(dir: 'libs', include: '*.jar')
}
```


## 处理依赖冲突

> gradle 默认的冲突解决方式：统一使用最高版本。

* 查看版本冲突报错需要修改默认策略

```groovy
// 示例：修改默认策略
configurations.all {
    resolutionStrategy{
        failOnVersionConflict()
    }
}
```

1. 去掉传递性依赖

```groovy
// 示例：去掉传递性依赖
dependencies{
  compile('org.hibernate:hibernate-core:3.6.3.Final'){
    exclue group:"org.slf4j",module:"slf4j-api
  }
}

```

2. 强制指定版本

```groovy

configurations.all {
    resolutionStrategy{
      failOnVersionConflict()
      force:"org.slf4j:slf4j-api:1.7.22"
    }
}

```

## 多项目构建

> 企业级项目中常常把一个项目拆分成多个子项目（模块化）。

* 配置要求
  * 所有项目应用 Java 插件
  * web 子项目打包成 war
  * 所有项目添加 logback 日志功能
  * 统一配置公共属性

* 项目范围：allprojects == root + subprojects

* settings.gradle: 当项目包含多个模块时，settings.gradle 中需要用 include 语句包含子项目。

* 统一配置可以写在 allprojects 或 subprojects 中。各模块的个性化配置写在模块目录下的 build.gradle

* 统一配置可以写在 gradle.properties 中。

```groovy
// gradle.properties 示例

group=com.hgy.gradle1
version=1.0-SNAPSHOT

```



## 测试

* 测试工具配置

```groovy
// 示例：测试工具配置
dependencies{
    testCompile 'junit:junit:4.11'
}

```

* 测试任务流程：

compileJava -> processTest -> classes -> jar
-> compileTestJava -> processTestResources -> testClasses -> test
-> check -> build

* 测试标志
  * 继承 junit.framework.TestCase 或 groovy.util.GroovyTestCase。
  * @RunWith 注解
  * @Test 注解


## 发布

> 为了把自己的写的代码提供给别人使用，可以将成果打包发布到本地仓库、私有仓库和中心仓库。

发布配置：

```groovy

allprojects{
    //////示例：发布配置
    apply plugin: 'maven-publish'

    publishing{
        publications{
            myPublish(MavenPublication){
                from components.java
            }
        }
        repositories {
            maven{
                name "repo-name"
                url  ""
            }
        }
    }
}

```

> 发布到的本地仓库的包可以再 用户根目录\.m2\repository 下找到 

## 参考资料
1. [《新一代构建工具gradle》](https://www.imooc.com/learn/833).慕课网.www.imooc.com
