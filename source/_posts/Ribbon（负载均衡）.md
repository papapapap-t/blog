---
category: featTest
excerpt: Ribbon是Netflix开源的一个基于HTTP和TCP客户端的负载均衡器。它为微服务架构中的服务消费者提供了一种简单且可定制的负载均衡解决方案。Ribbon的核心功能是根据一定的规则从一组服务实例中选择一个合适的目标实例，以实现请求的负载均衡。它通过维护服务实例列表和执行负载均衡算法来实现这一功能。Ribbon可以与服务注册中心（如Eureka、Consul）集成，自动获取当前可用的服务实例列表，并根据配置的负载均衡策略进行选择。
tags: [分布式 , SpringCloud , 负载均衡 , 微服务] 
title: Ribbon 
---
# Ribbon（负载均衡）

### 简介

1.  集中式

    即在服务的消费方和提供方之间使用独立的负载均衡设施（可以是硬件，如F5，也可以是软件，如Nginx）,由该设施负责把访问请求通过某种策略转发至服务的提供方；
2.  进程内

    将负载均衡逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。

    Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。
3.  Ribbon**的本地负载均衡客户端** **VS** Nginx**服务端负载均衡区别**：

    Nginx是服务器负载均衡，客户端所有请求都会交给Nginx，然后，由nginx实现转发请求。即负载均衡是由服务器端完成的。

    Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程服务调用

### 开启

如果使用的是`RestTemplate` 加上`@LoadBalanced` 注解可以让 `RestTemplate` 对象支持负载均衡

### 更换默认负载均衡策略

1.  书写一个配置类
    ```java
    @Configuration
    public class MyRule {
        @Bean
        // IRule 是所有策略的父接口
        public IRule iRule(){
         //  这个是随机的策略
            return new RandomRule();
        }
    }
    ```
2.  这个类需要放置到`@ComponentScan` 注解扫描不到的地方 如果被扫描到该客户端的所有的访问的服务提供方都会使用该负载均衡策略失去了对不同服务的针对性
3.  在springboot启动类中书写注解
    ```java
    // 可以配置多个对服务提供者的负载均衡策略
    @RibbonClients(value = {
            // 服务提供方                                // 均衡策略
            @RibbonClient(name = "spring-cloud-8001" , configuration = MyRule.class)
    })
    ```

### Ribbon的负载均衡策略

1.  **RoundRobinRule**（轮询）：按顺序逐个选择可用的服务实例，循环进行请求分发。
2.  **RandomRule**（随机）：随机选择一个可用的服务实例来处理每个请求。
3.  **RetryRule**（重试）：在一定时间内进行重试，选择一个可用的服务实例。默认情况下，每个服务实例将在30秒内重试2次。
4.  **WeightedResponseTimeRule**（加权响应时间）：根据服务实例的平均响应时间和权重来选择实例。响应时间越短且权重越高的实例被选中的概率越大。
5.  **BestAvailableRule**（最佳可用）：排除故障实例，并选择并发量最低的可用实例进行请求。
6.  **AvailabilityFilteringRule**（可用性过滤）：根据可用性过滤掉故障实例，然后按照指定策略（默认为轮询）选择一个实例。
7.  **ZoneAvoidanceRule**（区域避免）：根据区域来选择可用的服务实例，避免选择同一区域的实例。
8.  **Custom Rule**（自定义规则）：您可以根据自己的需求实现自定义的负载均衡规则。

**ZoneAvoidanceRule**（区域避免）是RIbbon的默认策略 只是在单机环境效果与 **RoundRobinRule**（轮询）一样
