## Spring Boot项目配置

* 默认情况下，Spring Boot项目会尝试根据添加的jar依赖自动配置Spring应用。例如，如果classpath下存在`HSQLDB`，并且你没有手动配置任何数据库连接的beans，那么Spring Boot将自动配置一个内存型的数据库。
* 实现自动配置有两种可选方式，分别是将`@EnableAutoConfiguration`或`@SpringBootApplication`注解到`@Configuration`类上。
* 手动配置，有基于Java的配置和XML配置两种方式。

### 导入其他配置类

* 你不需要将所有的`@Configuration`放进一个单独的类，`@Import`注解可以导入其他的配置类。
* 另外，也可以使用`@ComponentScan`注解自动收集所有的Spring组件，包括`Configuration`类。

### 导入XML配置类

如果必须使用XML配置，建议你仍从一个`@Configuration`类开始，然后使用`@ImportResource`注解加载XML配置文件。

### 逐步替换自动配置

自动配置是非侵入性的，任何时候你都可以定义自己的配置类来替换自动配置的特定部分。例如，如果添加自己的` DataSource `bean，默认的内嵌数据库支持将不被考虑。

如果需要查看当前应用启动了哪些自动配置项，你可以在运行应用时打开`--debug`开关，这将为核心日志开启debug日志级别，并将自动配置相关的日志输出到控制台。

### 禁用特定的自动配置项

如果发现启用了不想要的自动配置项，你可以使用`@EnableAutoConfiguration`注解的`exclude`属性禁用他们：
```java
package com.tribes.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;

@SpringBootApplication
@ComponementScan
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class})
public class DemoApplication {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## Spring Beans和依赖注入

你可以自由地使用任何标准的Spring框架技术去定义beans和他们注入的依赖。简单起见，我们经常使用`@ComponentScan`注解搜索beans，并结合`@Autowired`构造器注入。

如果遵循以上的建议组织代码结构（将应用的main类放到包的最上层），那么就可以添加`@ComponmentScan`注解而不需要任何参数，所有应用组件（`@Component`, `@Service`, `@Repository`, `@Controller`等）都会自动注册成Spring Beans。

```java
package com.tribes.demo.service.Impl;

import com.tribes.demo.dao.UserRepository;
import com.tribes.demo.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

/**
 * Created by duzhiqiang on 2017/6/19.
 */
@Service
public class AccountServiceImpl implements AccountService {

    private final UserRepository userRepository;

    @Autowired
    public AccountServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }


    @Override
    public String findUserById(Integer id) {
        return this.userRepository.findUserById(1);
    }
}
```

**Note**

注意使用构造器注入允许`userRepository`字段被标记为`final`，这意味着`userRepository`后续是不能改变的。

## 使用@SpringBootApplication注解

`@SpringBootApplication`注解等价于以默认属性使用`@Configuration`, `@EnableAutoConfiguration`和`@Componement`：

```java
package com.tribes.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;

@SpringBootApplication // same as @EnableAutoConfiguration @Configuration @ComponentScan
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class})
public class DemoApplication {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## 程序运行方式

### 从IDE中运行（略）

### 作为一个打包后的应用运行

如果使用Spring Boot Maven或Gradle插件创建一个可执行jar，你可以使用`java -jar`运行应用。例如：

`$ java -jar target/demo-0.0.1-SNAPSHOT.jar`

Spring Boot支持以远程调试模式运行一个打包的应用，下面的命令可以为应用关联一个调试器：

`$ java -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=8000,suspend=n -jar target/demo-0.0.1-SNAPSHOT.jar`

### 使用Maven插件运行

Spring Boot Maven插件包含一个`run`目标，可用来快速编译和运行程序，并且跟在IDE运行一样支持热加载。

`$ mvn spring-boot:run`

### 使用Gradle运行（略）

## 开发者工具

Spring Boot包含了一些额外的工具集，用于提升Spring Boot应用的开发体验。

```xml
        <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
```

**Note**

在运行一个完整的，打包过的应用时，开发者工具（devtools）会被自动禁用。如果应用使用`java -jar`或特殊的类加载器启动，都会被认为是一个产品级的应用（Production Application），从而禁用开发者工具。

### 开发者工具支持的默认属性

[属性列表](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-devtools/src/main/java/org/springframework/boot/devtools/env/DevToolsPropertyDefaultsPostProcessor.java)

### 自动重启

#### 排除资源

#### 查看其他路径

#### 禁用重启功能

### LiveReload

### 全局设置devtools

### 远程应用

