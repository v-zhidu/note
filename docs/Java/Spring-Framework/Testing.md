# Part IV. 测试

采用测试驱动方法（Test-Driven-Development）进行软件开发当然是Spring所提倡的, Spring对单元测试以及集成测试提供了支持以及一些最佳的实践。我们发现正确使用IOC可以使得单元测试和集成测试更加简单（不用在容器中注册服务，类中的setter方法和适合的构造函数就可以方便的连接在一起进行测试）

## Spring测试框架简介

### 单元测试

#### Mock Object

**Environment**

`org.springframework.mock.env`包包含了`Environment`和`PropertySource`抽象的模拟实现。`MockEnvironment`和`MockPropertySource`对out-of-container代码开发的测试是非常有用的，因为这些代码往往依赖于特定环境的属性。

**JNDI**

**Servlet API**

用于Spring MVC框架中对Web上下文和控制器进行测试。

**Portlet API**

用于Spring的Portlet MVC框架的使用。

#### 单元测试支持类

**通用组件**

`org.springframework.test.util`包包含`ReflectionTestUtils`类，这个类提供一套基于反射的工具方法。

### 集成测试

* 上下文管理和缓存
* 测试夹具的依赖注入
* 事务管理


##相关注解