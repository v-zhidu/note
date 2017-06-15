## Spring Boot 简介

Spring Boot简化了基于Spring的应用开发，你只需要`run`就能创建一个独立的，产品级别的Spring应用。我们为Spring平台及第三方库提供开箱即用的设置，多数Spring Boot应用只需要很少的配置。

你可以使用Spring Boot创建Java应用，并使用`java -jar`启动或采用传统的`war`部署方式。我们也提供一个运行Spring脚本的命令行工具。

Spring Boot的主要目标是：

* 为所有Spring开发提供一个从根本上更快，且随处可见的入门体验。
* 开箱即用，意思为很少的配置。
* 提供一系列大型项目常用的非功能性特征，比如：内嵌的服务器，安全，指标，健康检测，外部化配置。
* 没有代码生成，也没有XML配置。

## 系统要求

默认情况下，Spring Boot 1.5.4 RELEASE需要Java 7+，Spring Framework 4.3.9 RELEASE。如果使用Java 6的话需要一些额外的配置。Maven 3.2和Gradle 2.9以上已经明确提供了构建的支持。

虽然你可以在Java6或Java7环境下使用Spring Boot，通常建议尽可能使用Java8。

### 内置Servlet容器

[Servlet介绍](Servlet介绍)

|名称|Servlet版本|Java版本|
|--|----------|------|
|Tomcat 8|3.1|Java 7+|
|Tomcat 8|3.0|Java 6+|
|Jetty 9.3|3.1|Java 8+|
|Jetty 9.2|3.1|Java 7+|
|Jetty 9.1|3.0|Java 6+|
|Undertow 1.3|3.1|Java 7+|

## 安装Spring Boot

### 适用于开发者的安装说明

### Maven安装

[Maven官方网站](maven.apache.org)

**Mac OS**

`brew install maven`

**ubuntu**

`sudo apt-get install maven`

#### 一个最简单的`pom.xml`文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.tribes</groupId>
    <artifactId>spring-example</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.0.RELEASE</version>
    </parent>

    <!-- Add typical dependencies for a web application -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <!-- Package as an executable jar -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

* `spring-boot-starter-parent`

> springboot官方推荐我们使用spring-boot-starter-parent，spring-boot-starter-parent包含了以下信息：

> 1、使用java6编译级别

> 2、使用utf-8编码

> 3、实现了通用的测试框架 (JUnit, Hamcrest, Mockito).

> 4、智能资源过滤

> 5、智能的插件配置(exec plugin, surefire, Git commit ID, shade).

### 其它方式安装Spring Boot

* Gradle安装
* Spring Boot CLI安装

## 第一个Spring Boot应用

### 在开始前，需要检查Maven和Java的版本是否可用

![1.png](../../img/1.png)