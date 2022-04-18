概述：

1. Spring是轻量级开源的javaee框架

2. 核心IOC、AOP
3. 特点
   1. 解耦、方便开发
   2. AOP编程
   3. 方便测试
   4. 封装
   5. 方便事务



核心jar包 beans core content expression

创建对象：配置文件

```java
// 1. 加载配置文件;
ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
// 2. 获取配置创建的对象;
User user = context.getBean("user", User.class);
```



### IOC

1. 底层原理
2. beanFactory
3. IOC操作bean管理，基于xml，基于注解



什么是IOC

1. 控制反转，把对象创建和对象之间的调用过程，交给Spring管理
2. 目的：为了耦合度降低

实现原理：

1. xml解析、工厂模式、反射



接口：

1. IOC容器底层就是对象工厂
2. Spring提供IOC容器实现两种方式
   1. BeanFactory：IOC容器基本实现，Spring内部使用，不提供给开发人员使用
      * 加载配置文件的时候不会创建对象，使用时才创建
   2. ApplicationContext：BeanFactory子接口，提供更多更强大功能，开发使用
      * 加载配置文件的时候创建对象
   3. ApplicationContext实现类
      1. FileSystemXmlApplicationContext
      2. ClassPathXmlApplicationContext



IOC操作bean管理

1. 什么是bean管理
   1. Spring创建对象
   2. Spring注入属性

​	xml配置文件，id，class全路径，创建对象时，使用无参构造

​	DI：依赖注入， set、有参

​	p名称空间注入，简化set注入

​	注解方式



FactoryBean

1. Spring有两种类型bean，普通bean，工厂bean
2. 普通bean：在配置文件中定义bean的类型就是返回的类型
3. 工厂bean：定义bean类型可以和返回类型不一样



bean的作用域

1. singleton，加载配置文件的时候就会创建单实例对象
2. prototype，在调用getBean方法时创建对象
3. request
4. session



bean的生命周期

1. 通过构造器创建bean实例
2. 为bean的属性设置值和对其他bean的引用
3. 调用bean的初始化方法
4. 使用bean
5. 容器关闭时，调用bean的销毁方法



bean的后置处理器

1. 通过构造器创建bean实例
2. 为bean的属性设置值和对其他bean的引用
3. **把bean实例传递给bean后置处理器的方法**
4. 调用bean的初始化方法
5. **把bean实例传递给bean后置处理器的方法**
6. 使用bean
7. 容器关闭时，调用bean的销毁方法



自动装配



注解

1. @Component
2. @Service
3. @Controller
4. @Repository

开启组件扫描



注解方式属性注入

1. @Autowired：根据属性类型
2. @Qualifier：根据属性名称
3. @Resource：both
4. @Value：注入普通类型属性



完全注解开发

创建配置类，替代xml文件

@Configuration

@ComponentScan
AnnotationConfigApplicationContext





### AOP

1. 什么是AOP

   面向切面编程，业务逻辑隔离，提高重用性

2. 原理：

   动态代理：

   1. 有接口，使用JDK动态代理，Proxy类

      类加载器、增强方法所在类接口、代理对象

   2. 没有接口，使用CGLIB动态代理

3. 术语

   * 连接点，方法
   * 切入点，实际被增强的方法
   * 通知（增强），实际增强的逻辑部分。前置、后置、环绕、异常、最终
   * 切面，把通知应用到切入点

4. AOP操作

   1. 一般基于AspectJ实现AOP操作

      一个独立AOP框架

   2. 切入点表达式：execution

   3. 相同切入点抽取到Pointcut，在通知的value中使用对应方法

   4. 多个增强类通知优先级，增强类增加@Order





### 事务

1. 概念

   逻辑上的一组操作，要么都成功，要么都失败

   原子性

   一致性，操作前，操作后一致性

   隔离性

   持久性，表数据变化

2. 声明式事务管理

   1. 注解
   2. xml

3. Spring事务管理API

   1. 接口，不同实现类，DataSourceTransactionManager

4. 操作

   1. 创建事务管理器，注入数据源
   2. 开启事务注解
   3. @Transactional

5. 参数

   1. propagation，事务传播行为

      多事务方法调用

      1. required
      2. required_new

      

   2. isolation，事务隔离级别

      1. 脏读，读未提交
      2. 不可重复读，读已提交
      3. 幻读，读已提交增加

   3. timeout，超时时间

   4. readOnly，是否只读

   5. rollbackFor，回滚

   6. noRollbackFor，不回滚





### Webflux

1. 概述

   