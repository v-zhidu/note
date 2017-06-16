# 使用Spring Boot

## 构建系统

强烈建议你选择一个支持依赖管理，能消费发布到“Maven中央仓库”的artifacts的构建系统，比如Maven或Gradle。使用其他构建系统也是可以的，比如Ant，但他们得不到很好的支持。

### 依赖管理

Spring Boot每次发布时都会提供一个它所支持的精选依赖列表。实际上，在构建配置里你不需要提供任何依赖的版本，因为Spring Boot已经替你管理好了。当更新Spring Boot时，那些依赖也会一起更新。

**Note**

* 如果有必要，你可以指定依赖的版本来覆盖Spring Boot默认版本。

* Spring Boot每次发布都关联一个Spring框架的基础版本，所以强烈建议你不要自己指定Spring版本。

### Maven

springboot官方推荐我们使用spring-boot-starter-parent，Maven用户可以继承`spring-boot-starter-parent`项目来获取合适的默认设置。 spring-boot-starter-parent包含了以下信息：

* 使用java 1.6编译级别

* 使用utf-8编码

* 实现了通用的测试框架 (JUnit, Hamcrest, Mockito).

* 智能资源过滤

* 智能的插件配置(exec plugin, surefire, Git commit ID, shade).

* 一个`Dependency management`节点，允许省略常见依赖的`<version>`标签，继承自[spring-boot-dependencies](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-dependencies/pom.xml)POM。

* 对`application.properties`和`application.yml`进行筛选，包括特定的`profile`文件，比如`application-foo.properties`和`application-foo.yml`。

#### 继承starter parent

如果你想配置项目，让其继承自`spring-boot-starter-parent`，只需将`parent`按如下配置：

```xml
    <!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.0.RELEASE</version>
    </parent>
```

按照以上设置，你可以在自己的项目中通过覆盖属性来覆盖个别的依赖。例如，你可以将以下设置添加到`pom.xml`中来升级Spring Data到另一个发布版本。

```xml
    <properties>
        <spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
    </properties>
```

#### 不使用starter parent

如果你不想使用`spring-boot-starter-parent`，通过设置`scope=import`的依赖，你仍能获取到依赖管理的好处：

```xml
    <dependencyManagement>
		<dependencies>
			<dependency>
				<!-- Import dependency management from Spring Boot -->
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>1.4.0.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
```

#### 改变Java版本

`spring-boot-starter-parent`选择了相当保守的Java兼容策略，可以添加一个`java.version`属性：

```xml
    <properties>
        <java.version>1.8</java.version>
    </properties>
```

### Starters

以下应用程序starters是Spring Boot在`org.springframework.boot`group下提供的：

**Spring Boot应用程序Starters**

|名称|描述|POM|
|--|----------|:------:|
|sping-boot-starter-test|用于测试Spring Boot应用，支持常用测试类库，包括JUnit, Hamcrest和Mockito|[POM](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-starters/spring-boot-starter-test/pom.xml)|
|spring-boot-starter-jdbc|对JDBC的支持（使用Tomcat JDBC连接池）|[POM](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-starters/spring-boot-starter-jdbc/pom.xml)|

**Spring Boot技术性Starters**

|名称|描述|POM|
|--|----------|:------:|
|spring-boot-starter-undertow|用于使用Undertow作为内嵌Servlet容器，可使用`spring-boot-starter-tomcat`替代|[POM](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-starters/spring-boot-starter-undertow/pom.xml)|
|spring-boot-starter-logging|用于食用logback记录日志，默认的日志starter|[POM](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-starters/spring-boot-starter-logging/pom.xml)|

**Spring Boot生产级别Starters**

|名称|描述|POM|
|--|----------|:------:|
|spring-boot-starter-undertow|用于使用Undertow作为内嵌Servlet容器，可使用`spring-boot-starter-tomcat`替代|[POM]()

> 查看Github上位于`spring-boot-starters`模块内的[README文件](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-starters/README.adoc)，可以获取到一个社区贡献的其他starters列表。

## 代码结构

```
|--- {project-name}
|   |--- src
|       |--- main
|           |--- java
|               |--- {com.example.demo}
|                   |--- domain
|                       |--- User.java
|                       |--- UserRepository.java
|                   |---service
|                       |--- UserService.java
|                   |--- web
|                       |--- HomeController.java
|                   |--- Application.java
|           |--- resources
|       |--- test
|           |--- java
|               |--- {com.example.demo}
|                   |--- ApplicationTests.java
|--- .gitignore
|--- README
|--- pom.xml
```

通常建议将应用的`main`类放到其他类所在包的顶层（root package），并将`@EnableAutoConfiguration`注解到你的类上，这样的隐式地定义了一个基础的包搜索路径（search package），以搜索某些特定的注解实体（比如`@Service`，`@Component`，`@Entity`等）。例如，如果你正在编写一个JPA的应用，Spring将搜索`@EnableAutoConfiguration`注解的类所在包下的`@Entity`实体。

采用root package方式，你就可以使用`@ComponementScan`注解而不需要指定`basePackage`属性，也可以适用`@SpringBootApplication`注解，主要将main类放到root package中。

`Application.java`将声明main方法，还有基本的`@Configuration`。
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
//import org.springframework.boot.autoconfigure.SpringBootApplication;

//@SpringBootApplication
@Configuration
@EnableAutoConfiguration
@ComponementScan
public class DemoApplication {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```