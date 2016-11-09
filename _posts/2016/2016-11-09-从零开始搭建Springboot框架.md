---
layout: post
title: 从零开始搭建Springboot框架
category: 开发
tags: [JAVA,springboot,shiro]
---

## 什么是Springboot

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。从我最直观的感受来看，从一个最直观的角度来看，使用springboot可以减少很多配置文件，更加方便。

## 快速入门

###系统要求
- jdk1.7及以上
- Spring Framework 4.1.5及以上

#####本文采用jdk8,spring boot 1.3.1.RELEASE

###构建maven工程
 1. 通过`spring initializr工具生产`
    1.访问`http://start.spring.io/`
    2.选择构建工具,spring版本,以及一些工程信息,可参考下图
    ![springio.png](/assets/images/2016/springio.png)
    3.点击`Generate Project`下载工程压缩包
    
 2. 自己新建一个maven工程.
 
###添加springboot的引用
在pom文件中增加引用
```java
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.3.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```
因为我们需要编写一个web工程 所以我们需要引入web模块
- `spring-boot-starter`: 核心模块
- `spring-boot-starter-test`: 测试模块
- `spring-boot-web`: web模块

```java
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```
#编写Hello world服务
- 创建`HelloController`类
```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String index() {
        return "Hello World";
    }
}
```

#编写启动类
我们新建application.class文件 在文件头增加```@SpringBootApplication```表示这是springboot的启动文件 然后运行main方法即可启动 
```java
@SpringBootApplication
@EnableScheduling
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```
- 启动主程序，打开浏览器访问`http://localhost:8080/hello`，可以看到页面输出`Hello World`

至此一个简单的springboot工程已经搭建完成
