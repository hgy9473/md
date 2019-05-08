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


## Spring Boot 热部署

1. 配置 pom.xml

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<optional>true</optional>
		</dependency>
```

2. 配置application.properties

```python

# 热部署生效
spring.devtools.restart.enabled=true
#设置监听目录
spring.devtools.restart.additional-paths=src/main/java

```

3. 启动项目，修改代码后项目会自动重启


## Spring Boot 项目发布

* 方式一：生成 jar 包，命令行运行。

  * 生成 jar 文件：右击项目 -> 【Run As】-> 【Maven Install】
  * 运行 jar 文件：java -jar `<file>.jar`

* 方式二：生成 war 包，发布到 Tomcat 运行。
  * 修改 pom.xml 中的 `<packaging>jar</packaging>` 为 `<packaging>war</packaging>`
  * 在 pom.xml 中添加 tomcat 依赖
  ```xml
    <dependency>
		    <groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		    <scope>provided</scope>
		</dependency>
  ```
  * 修改启动类
    * 让启动类继承 SpringBootServletInitializer
    * 在启动类增加方法
    ```java
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(Demo03Application.class);
    }
    ```
  * 生成 war 文件：右击项目 -> 【Run As】-> 【Maven Install】。
  * 启动项目：拷贝 war 文件到 tomcat/webapps 目录下，启动 tomcat, 访问项目（注意项目的路径要有 war 文件名）。


## 发布、运行后 src/main/webapp 中的资源不能访问

1. 在 Package Explorer 视图中右击项目 -> 【Build Path】-> 【Configure Build Path】, 【Source】 -> 【Add Folder】, 勾选 src/main/webapp 。

2. 在 pom.xml 中添加配置

```xml
<build>
    <resources>
        <resource>
            <directory>${basedir}/src/main/webapp</directory>
      <targetPath>META-INF/resources</targetPath>
            <includes>
                <include>**/**</include>
            </includes>
        </resource>
        <resource>
            <directory>${basedir}/src/main/resources</directory>
            <includes>
                <include>**/**</include>
            </includes>
        </resource>
    </resources>
</build>
```

3. 重新执行 Maven install



## 整合 Mybatis 



## 关于 Spring Boot 中 pom.xml

### Spring Boot 父级依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

有了这个，当前的项目就是 Spring Boot 项目了，spring-boot-starter-parent 是一个特殊的 starter,它用来提供相关的 Maven 默认依赖，使用它之后，常用的包依赖可以省去 version 标签。

### starter 依赖

Spring Boot 提供了很多 “开箱即用” 的依赖模块，都是以 spring-boot-starter-xx 作为命名的。


## 关于 application.properties

Spring Boot 使用了一个全局的配置文件 application.properties 。

application.properties 提供自定义属性的支持。在要使用的地方通过注解 @Value(value="${config.name}")就可以绑定到想要的属性上。

```java

//...
@MapperScan("com.example.dao")
public class DataSourceConfiguration {
    @Value("${jdbc.driver}")
    private String jdbcDriver;
    @Value("${jdbc.url}")
    private String jdbcUrl;
    @Value("${jdbc.username}")
    private String jdbcUsername;
    @Value("${jdbc.password}")

//...

```

在 application.properties 中的各个参数之间也可以直接引用来使用。

```python

com.ex.name="zhangsanfeng"
com.ex.age="23"
com.ex.aliasname=${com.ex.name}


```

如果不希望把所有配置都放在 application.properties 里面，可以另外定义一个，通过注解 @Configuration 和 @PropertySource 引入。

配置文件中 ${random} 可以用来生成各种不同类型的随机值

## 参考资料

【1】http://tengj.top/2017/04/24/springboot0/
【2】http://blog.geekidentity.com/spring/spring_boot_translation/
https://github.com/DocsHome/springboot/blob/master/pages/getting-started.md
