---
layout: post
title: Springboot整合线程池
category: 开发,spring,JAVA
tags: [JAVA,SPRING]
---

##线程池
关于线程池的概念,百度一下一大堆,在这不做赘述了.

原来的项目都是springmvc配置线程池相对简单,只需要在spring的`applicationContext.xml`文件中加入如下的内容,并引入相应的依赖.
```java
    <!-- 配置spring 同步线程池(主要用于处理启动和关闭服务器时的任务) -->
    <bean id="synTaskExecutor" class="org.springframework.core.task.SyncTaskExecutor"/>

    <!-- 配置spring 异步线程池(主要用于处理数据上报的处理任务,一些定时任务等) -->
    <bean id="taskExecutor"
          class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
        <!-- 线程池维护线程的最少数量 -->
        <property name="corePoolSize" value="500"/>
        <!-- 线程池维护线程所允许的空闲时间 -->
        <property name="keepAliveSeconds" value="300"/>
        <!-- 线程池维护线程的最大数量 -->
        <property name="maxPoolSize" value="1000"/>
        <!-- 线程池所使用的缓冲队列 -->
        <property name="queueCapacity" value="2000"/>
        <!-- 线程池对拒绝任务(无线程可用)的处理策略 -->
        <property name="rejectedExecutionHandler">
            <bean class="java.util.concurrent.ThreadPoolExecutor$CallerRunsPolicy"/>
        </property>
    </bean>
```

但是现在向springboot上转型,所以现在项目都是使用springboot来搭建的,所以我们按照如下的方式进行配置.

1.创建 `ThreadConfig` 类(此处命名可以自行修改).
```java
/**
 * Created by adam on 28/11/16.
 */
@Configuration
public class ThreadConfig {

    @Bean(name = "threadPoolTaskExecutor")
    public ThreadPoolTaskExecutor threadPoolTaskExecutorBean(){
        ThreadPoolTaskExecutor bean = new ThreadPoolTaskExecutor();
        bean.setCorePoolSize(500);
        bean.setKeepAliveSeconds(300);
        bean.setMaxPoolSize(1000);
        bean.setQueueCapacity(2000);
        bean.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        return bean;
    }

}
```
2.在需要使用线程池的地方引入
```java
@Autowired
ThreadPoolTaskExecutor taskExecutor;

taskExecutor.submit(xxxx);
```
