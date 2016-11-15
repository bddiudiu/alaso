---
layout: post
title: Spring Cloud配置多个service
category: 开发
tags: [JAVA,springcloud]
---

## 配置springcloud注册中心

搭建一个配置好的springcloud注册中心 Eureka工程 并运行
> http://localhost:1111

## 服务提供者
> 因为测试多个service,所以此处新建一个service-A工程并复制一份相同的工程service-B

###新增一个业务 完成2个数字相加

```java
@RestController
public class ComputeController {

    private final Logger logger = Logger.getLogger(getClass());

    @Autowired
    private DiscoveryClient client;

    @RequestMapping(value = "/add" ,method = RequestMethod.GET)
    public Integer add(@RequestParam Integer a, @RequestParam Integer b) {
        ServiceInstance instance = client.getLocalServiceInstance();
        Integer r = a + b + 1;
        logger.info("/add, host:" + instance.getHost() + ", service_id:" + instance.getServiceId() + ", result:" + r + "from service-A");
        return r;
    }

}
```
>在service-B中修改logger输出为from service-B

###配置application.class

```java
@EnableDiscoveryClient
@SpringBootApplication
public class ComputeServiceApplication {

	public static void main(String[] args) {
		new SpringApplicationBuilder(ComputeServiceApplication.class).web(true).run(args);
	}

}
```

###配置application.properties
```
spring.application.name=compute-service

server.port=2222

eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/
```

>复制工程为service-B 修改 server.port=3333 

运行 `Eureka工程` `service-A` `service-B`

可以在`http://localhost:1111` 中看到如下内容
![springcloud.png](/assets/images/2016/springcloud.png)

访问`http://localhost:4444/add` 可以看到我们后台的输出

此刻就包含了2个可用的服务
所以此刻的输出则会在service-A和service-B上进行负载均衡
如果此刻一个服务停用,所有的请求则会转到另外一个service上

>下一步将会把高效网关整合进来
