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

@SpringBootApplication // same as @EnableAutoConfiguration @Configuration @ComponentScan
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class})
public class DemoApplication {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## Spring Beans和依赖注入

你可以自由地使用任何标准的Spring框架技术去定义beans和他们注入的依赖。简单起见，我们经常使用`@ComponentScan`注解搜索beans，并结合`@Autowired`构造器注入。

如果遵循以上的建议组织代码结构（将应用的main类放到包的最上层），那么就可以添加`@ComponmentScan`注解而不需要任何参数，所有应用组件（`@Component`, `@Service`, `@Repository`, `@Controller`）

## 使用@SpringBootApplication注解

## 程序运行方式

## 开发者工具