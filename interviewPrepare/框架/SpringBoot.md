https://www.yuque.com/atguigu/springboot

# 1. 概述

生态圈：

可以快速构建微服务、响应式、分布式

无服务开发，faas，函数即服务。不需要购买服务器，上云，根据需要计费

事件驱动，通过响应式用少量资源完成高吞吐量的业务

批处理

一个生态圈



众多集成、配置地狱

SpringBoot简化搭建，专心业务逻辑



时代：

微服务：

* 微服务是一种架构风格
* 一个应用拆分为一组小型服务
* 每个服务运行在自己的进程内，也就是可独立部署和升级
* 服务之间使用轻量级HTTP交互
* 服务围绕业务功能拆分
* 可以由全自动部署机制独立部署
* 去中心化，服务自治。服务之间可以用不同的语言开发



分布式：

微服务就会带来分布式，困难

* 远程调用
* 服务发现
* 负载均衡
* 服务容错
* 配置管理
* 服务监控
* 链路追踪
* 日志管理
* 任务调度



# 2. 入门

看官方文档

@ResponseBody + @Controller = @RestController (返回字符串的接口)

1. 依赖管理

父项目依赖管理，几乎声明了所有开发中常用的版本号

自动版本仲裁机制

spring-boot-starter-*

*-spring-boot-starter，第三方提供的简化开发的场景启动器

所有场景启动器最底层的依赖

```java
spring-boot-starter
```



2. 自动配置

自动配好mvc

自动配好web常见功能，如支持中文

默认的包结构

```java
@SpringBootApplication
等同
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```



各种配置拥有默认值，最终都是映射到MultipartProperties，最终都会绑定到某个类上，这个类会在容器中创建对象

按需加载所有自动配置类

* 引入依赖
* 配置

怎么看SpringBoot帮我们配置的web开发常见场景

```java
// 1. 返回IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(DemoApplication.class, args);

// 2. 查看容器里面的组件
        String[] beanDefinitionNames = run.getBeanDefinitionNames();
        for (String beanDefinitionName : beanDefinitionNames) {
            System.out.println(beanDefinitionName);
        }


```



# 3. 底层

## 底层注解

### @Configuration

告诉SpringBoot是一个配置类=配置文件

1. 配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
2. 配置类本身也是组件
3. proxyBeanMethods：代理bean的方法

如果@Configuration(proxyBeanMethods=true)代理对象调用方法

SpringBoot总会检查这个组件是否在容器中存在，保持组件单实例

full（proxyBeanMethods=true）

lite（proxyBeanMethods=false）

组件依赖

如果组件间没有依赖，那就调成false，这样SpringBoot启动时则会很快，因为不用检查容器中的组件

@Import 默认获取全类名组件



### @Conditional

满足指定条件时，才进行组件注入



### @ImportResource

原生spring配置文件引入



## 配置绑定

@ConfigurationProperties

只会在容器中的组件，才会有SpringBoot提供的强大功能

@EnableConfigurationProperties（x.class）

1 开启属性配置功能，2 把指定组件自动注册到容器中



## 自动配置原理

### 1、引导加载自动配置类

#### 1 @SpringBootConfiguration

@Configuration。代表当前是一个配置类



#### 2@ComponentScan

指定扫描哪些，Spring注解；



#### 3、@EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

##### 1@AutoConfigurationPackage

自动配置包？指定了默认的包规则

```java
@Import(AutoConfigurationPackages.Registrar.class)  //给容器中导入一个组件
public @interface AutoConfigurationPackage {}

//利用Registrar给容器中导入一系列组件
//将指定的一个包下的所有组件导入进来？MainApplication 所在包下。
```

##### 2@Import(AutoConfigurationImportSelector.class)

```java
1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
4、从META-INF/spring.factories位置来加载一个文件。
	默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
    spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factories
    
```

## ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602845382065-5c41abf5-ee10-4c93-89e4-2a9b831c3ceb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_29%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



### 按需开启自动配置项

```java
虽然我们127个场景的所有自动配置启动的时候默认全部加载
按照条件规则，最终会按需配置
```

