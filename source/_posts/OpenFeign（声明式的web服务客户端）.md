---
category: featTest
excerpt: OpenFeign是一个**声明式的**、**基于注解的HTTP客户端框架**，用于**简化和优化服务之间的通信**。它是Spring Cloud生态系统中的一部分，旨在提供一种更简单、更优雅的方式来编写基于HTTP的服务调用代码。使用接口映射，通过动态代理让服务之间的通信像调用本地方法一样方便。默认支持Ribbon 负载均衡
tags: [分布式 , SpringCloud , 服务通信 , 微服务 , OpenFeign] 
title: OpenFeign 
---
# OpenFeign（声明式的web服务客户端）

### 简介

OpenFeign是一个**声明式的**、**基于注解的HTTP客户端框架**，用于**简化和优化服务之间的通信**。它是Spring Cloud生态系统中的一部分，旨在提供一种更简单、更优雅的方式来编写基于HTTP的服务调用代码。

使用接口映射，通过动态代理让服务之间的通信像调用本地方法一样方便。默认支持Ribbon 负载均衡

### 主要特征

1.  声明式API：通过定义标准的Java接口和注解，您可以对远程服务的API进行描述，而不需要手动编写客户端代码。
2.  内置负载均衡：OpenFeign集成了Ribbon负载均衡器，在服务调用时可以自动进行负载均衡和多实例选择。
3.  容错支持：通过与Hystrix的集成，OpenFeign可以提供对远程服务的容错支持，例如超时处理、断路器、降级等。
4.  整合服务注册与发现：OpenFeign可以与Eureka或其他服务注册中心集成，使得服务消费方能够轻松地发现和调用远程服务。
5.  自动编码和解码：OpenFeign能够自动将HTTP请求和响应转换为Java对象，简化了数据传输的过程。

### Feign和OpenFeign两者区别

Feign 是一个独立的项目，而 `OpenFeign `是 `Spring Cloud 在 Feign` 基础上进行了扩展和封装。

1.  组织结构：Feign 是一个独立的项目，由 Netflix 开发和维护。而 OpenFeign 是 Spring Cloud 中的一个子项目，它在 Feign 的基础上进行了扩展和封装，以便更好地集成到 Spring Cloud 生态系统中。
2.  注解支持：OpenFeign 提供了比 Feign 更丰富的注解支持。例如，OpenFeign 支持使用 `@RequestMapping`、`@GetMapping`、`@PostMapping` 等常见的 Spring MVC 注解，这样可以更好地与 Spring Boot 应用程序集成。
3.  自动装配：OpenFeign 可以通过 `@EnableFeignClients` 注解自动进行客户端的注册和装配。这个注解可以在 Spring Boot 应用程序的主类上使用，无需手动创建 Feign 客户端的实例。而 Feign 则需要显式地创建 Feign 客户端的实例并注册到 Spring 容器中。
4.  默认配置：OpenFeign 有一组默认的配置属性，可以通过配置文件或代码进行自定义。这些默认配置包括连接超时、读取超时等。而 Feign 则需要手动配置相应的属性。
5.  兼容性：由于 `OpenFeign `是 Spring Cloud 的一部分，它与其他 Spring Cloud 组件（如 `Ribbon`、`Hystrix`、`Eureka `等）的集成更加方便和无缝。而 Feign 原生的集成可能需要更多的配置和额外的工作。

### 使用

1.  引入依赖
    ```xml
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-openfeign</artifactId>
            </dependency>
    ```
2.  配置映射接口
    ```java
    //服务的名字
    @FeignClient(value = "SPRING-CLOUD-8001")
    public interface PaymentFeignService {
    //    url路径包括层级
        @PostMapping("/payment")
    //    方法名无所谓 返回值和形参（类型 名称 位置）需要一致
        CommonResult<Payment> insertPayment(Payment payment);

        @GetMapping("/payment/{id}")
        CommonResult<Integer> selectById(@PathVariable(value = "id") Long id);
        @GetMapping("/payment")
        public String timeOutTest();
    }
    ```
    1.  需要映射的服务在注册中心的名称
    2.  方法的请求的URL路径需要一致包括层级
    3.  方法的返回值和形参[^注释1]需要一致
3.  在启动主类添加一个注解`@EnableFeignClients(basePackages = "com.java.service")`&#x20;
    ```java
    @SpringBootApplication
    @EnableEurekaClient
    @RibbonClients(value = {
            @RibbonClient(name = "spring-cloud-8001" , configuration = MyRule.class)
    })
    @EnableFeignClients(basePackages = "com.java.service")
    public class MySpringBootApplication {

        public static void main(String[] args) {
            SpringApplication.run(MySpringBootApplication.class,args);
        }

    }
    ```

### 日志打印

1.  配置类配置日志对象
    ```java
        import feign.Logger;
        
        @Bean
        public Logger.Level level(){
            return Logger.Level.FULL;
        }
    ```
2.  OpenFeign的四种日志级别
    ```text
    NONE：没有日志记录
    BASIC：记录请求方法、URL以及响应状态代码和执行时间
    HEADERS：记录基本信息以及请求和响应头信息
    FULL：记录基本信息以及请求和响应头信息、请求和响应体信息

    ```
3.  配置`application.yml`文件
    ```java
    logging:
      level:
        com:
          java:
            service:
              PaymentFeignService: debug
    ```

### 超时控制

#### Ribbon 超时控制

修改`application.yml` 配置文件

```java
ribbon:
  ReadTimeout: 4000 #响应最大超时时间
  ConnectTimeout: 4000 #连接建立最大超时时间
  MaxAutoRetries: 1 #同一台实例最大重试次数,不包括首次调用
  MaxAutoRetriesNextServer: 1 #重试负载均衡下次的实例最大重试次数,不包括首次调用 重试其他的实例重试重试的实例并不一定是同一个实例
  如果 一共有5个服务实例  重试会进行一定算法轮询 
  OkToRetryOnAllOperations: false #是否所有操作都重试 如果是false只是重试读的操作 true 读写都会重试
```

`MaxAutoRetries + MaxAutoRetriesNextServer + (MaxAutoRetries * MaxAutoRetriesNextServer) 即重试3次 则一共产生4次调用`

#### 关于重试其他实例配置的解释（`MaxAutoRetriesNextServer`）

如果有五台实例可用，MaxAutoRetriesNextServer参数设置为3，那么在每个请求中，Ribbon会首先选择其中的一台实例处理该请求。如果该实例处理请求失败，Ribbon会尝试选择另一台可用实例，重试次数为1。如果第二台实例也失败，Ribbon会再次尝试选择另一台可用实例，重试次数为2。如果第三台实例仍然失败，Ribbon会再次尝试选择另一台可用实例，重试次数为3。如果第四台实例还是失败，Ribbon会再次尝试选择另一台可用实例，但由于已经达到了最大重试次数，将返回错误信息。

#### OpenFeign超时控制

```yaml
feign:
  client:
    config:
      # 提供方的服务名
      spring-cloud-8001:
        #请求日志级别
        loggerLevel: full
        contract: feign.Contract.Default #设置为默认的契约（还原成原生注解）
        # 连接超时时间，默认2s，设置单位为毫秒
        connectTimeout: 5000
        # 请求处理超时时间，默认5s，设置单位为毫秒。
        readTimeout: 3000
```

[^注释1]: 类型 名称 位置都要一致
