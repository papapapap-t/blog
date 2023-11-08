---
category: featTest
excerpt: Eureka是Netflix开源的一款基于REST的服务治理方案，主要功能包括服务注册与发现、服务健康检查等。它是Spring Cloud中比较重要的组件之一，可以很方便地实现微服务的注册、发现和调用。
tags: [分布式 , SpringCloud , 注册中心 , 微服务 , CAP] 
title: Eureka 
---
# Eureka

### 什么是Eureka

Eureka是Netflix开源的一款基于REST的服务治理方案，主要功能包括服务注册与发现、服务健康检查等。它是Spring Cloud中比较重要的组件之一，可以很方便地实现微服务的注册、发现和调用。

### Eureka的架构

Eureka的架构包括两部分，即Eureka Server和Eureka Client。其中，Eureka Server是服务注册中心，负责维护所有服务实例的信息，而Eureka Client则是服务提供者和服务消费者，它会将自己注册到Eureka Server上，并从中获取其他服务的信息。

### Eureka的核心概念

-   注册中心 &#x20;

    注册中心是服务发现的核心，它在Eureka中被称为Eureka Server。所有的服务都需要在注册中心进行注册，注册中心会维护这些服务的元数据信息，如服务名称、实例ID、IP地址、端口号等。
-   服务提供者 &#x20;
    服务提供者也称为服务实例，它是一个Web应用程序，向注册中心注册自己的服务地址和元数据信息，并向外界提供自己的服务能力。
-   服务消费者 &#x20;

    服务消费者是客户端应用程序，它向注册中心获取服务提供者的信息，并通过调用服务提供者的接口来实现业务逻辑。
-   注册表 &#x20;

    注册表是Eureka Server维护的所有服务实例的信息集合，包括服务名称、实例ID、IP地址、端口号等。注册表存储在Eureka Server的内存中，可以通过REST API进行访问和操作。
-   心跳机制&#x20;

    Eureka使用心跳机制来检测服务是否可用。服务提供者会周期性地向注册中心发送心跳信息，告诉注册中心自己还活着。如果一个服务实例在一定时间内没有发送心跳信息，那么它就会被视为不可用，并从注册表中删除。

### 作用

以实现`服务调用`，`负载均衡`、`容错`等，实现`服务发现`与`注册`。

### Eureka的两大组件

#### **EurekaServer提供服务注册服务**

这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

```xml
        <!-- 引入依赖 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

```

```yaml
#端口设置
server:
  port: 8003
#  这个配置则是本服务注册中心的名称
spring:
  application:
    name: spring-cloud-8001
eureka:
#  Eureka服务器的地址
  instance:
    hostname: 127.0.0.1
  client:
#    因为eureka的服务端也是客户端所以需要配置
#    表示该服务提供者不会将自身的服务注册到 Eureka 注册中心中。
    register-with-eureka: false
#    表示服务提供者将不会从注册中心获取其他服务的注册信息
    fetch-registry: false
#    eureka服务器路径 如果这个是服务器将他的路径指向自己
    service-url:
      defaultZone: https://${eureka.instance.hostname}:9002/eureka

```

```java
@SpringBootApplication
@EnableEurekaServer
public class SpringBootApplicationEureka {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootApplicationEureka.class,args);
    }
}
```

#### Eureka 的高可用

如果只有一个Eureka  的服务端 且服务端发生了故障 会导致所有的客户端无法使用

可以配置多个Eureka 的服务端互相注册 一个故障了可以使用其他的&#x20;

#### Eureka 的客户端注册

```xml
        <!--引入依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

```yaml
eureka:
  client:
    register-with-eureka: true
    fetchRegistry: true
    service-url:
      defaultZone: http://127.0.0.1:9001/eureka,http://127.0.0.1:9002/eureka
```

```java
@SpringBootApplication
@EnableEurekaClient
public class MySpringBootApplication {
    public static void main(String[] args) {
        SpringApplication.run(MySpringBootApplication.class,args);
    }
}
```
