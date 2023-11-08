---
category: featTest
excerpt: API网关（API Gateway）是一个位于服务端和客户端之间的中间层，用于管理和调度多个后端API服务。它充当了客户端和后端服务之间的代理，提供了一种集中式的方式来处理API请求。Gateway旨在提供一种简单而有效的方式来对API进行路由，以及提供一些强大的**过滤器**功能，例如：**熔断**、**限流**、**重试** 等
tags: [分布式 , SpringCloud , 网关 , 微服务 , Gateway] 
title: Gateway网关 
---
# Gateway网关

### 网关

API网关（API Gateway）是一个位于服务端和客户端之间的中间层，用于管理和调度多个后端API服务。它充当了客户端和后端服务之间的代理，提供了一种集中式的方式来处理API请求。

#### API网关主要功能

1.  路由与转发：API网关作为系统的入口点，负责接收外部请求并根据预定义的路由规则将请求路由到相应的微服务。它能够将请求转发给正确的服务实例，并确保请求的正确性和完整性。
2.  负载均衡：API网关可以通过负载均衡算法将请求分发到多个可用的服务实例上。这样可以平衡每个服务实例的负载，提高系统的性能和可扩展性。
3.  认证与授权：API网关可以实现用户认证和授权，以确保只有经过身份验证和授权的用户才能访问受保护的API。它可以集成各种身份验证机制，如基于令牌的认证、OAuth等，同时对请求进行权限验证和访问控制。
4.  安全性与防护：API网关可以实施安全策略，包括加密传输、防止恶意攻击（如DDoS攻击）、限制请求频率等。它可以对请求进行过滤和检查，以确保系统的安全性和可靠性。
5.  监控与日志：API网关可以收集和记录请求的相关信息，如请求量、响应时间、错误代码等，以用于系统的监控和故障排查。它可以生成日志和统计数据，帮助开发团队识别性能问题和改进系统设计。
6.  缓存与性能优化：API网关可以缓存常用的请求结果，减少对后端服务的请求次数，提高系统的响应速度和性能。它可以通过缓存策略来优化请求处理，并管理缓存的一致性和过期策略。
7.  API版本控制：API网关可以支持多个API版本的并存和管理，允许不同版本的客户端使用不同的API接口。它可以根据请求中的版本信息，将请求路由到相应的API版本。
8.  熔断：当某个服务发生故障或超时时，API网关可以检测到异常情况，并暂时中断对该服务的请求转发。这样可以防止故障服务进一步影响整个系统的正常运行。熔断器会记录故障状态，当达到一定阈值后，将直接拒绝请求，不再转发给故障服务，从而避免服务雪崩效应。
9.  重试：当某个服务的请求失败时，API网关可以尝试重新发送请求，以期望下一次尝试可以成功。重试策略可以根据需要进行配置，如设置最大重试次数、重试间隔时间等。通过重试机制，可以提高对故障服务的容错能力，增加请求的成功率。

### 简介

Gateway旨在提供一种简单而有效的方式来对API进行路由，以及提供一些强大的**过滤器**功能，例如：**熔断**、**限流**、**重试** 等

1.  反向代理
2.  &#x20;权限控制
3.  流量控制
4.  熔断
5.  日志监控
6.  。。。。。。。

### 三大核心概念

#### Route(路由)

路由是构建网关的基本模块，它由`ID`，目标`URI`，一系列的`断言`和`过滤器`组成，如果断言为true则匹配**该路由**

#### Predicate（断言）

可以匹配HTTP请求中的所有内容（例如请求头或请求参数），如果请求与断言相匹配则进行路由 **常用断言**

1.  After 之后   可放置时间 在这个时间之后可以访问
2.  Before 之前 可放置时间 在这个时间之前可以访问
3.  Between 之间  放置两个时间参数 在这两个时间之间可以访问
4.  Cookie  匹配Cookie 中的参数 `- Cookie=KEY,VALUE`
5.  Header  请求头匹配&#x20;
6.  Host  主机名匹配 只允许 这个主机名可以匹配
7.  Method   限制请求方式  GET或者POST或者其他请求方式
8.  Path  只允许这个路径访问 可以防止通配符
9.  Query  匹配参数 只有请求携带匹配参数的时候才可以访问
10. 。。。。。。

#### Filter过滤器

#### 局部过滤器和全局过滤器的优先级

1.  首先，局部过滤器会拦截并处理适用于特定接口或网络段的流量。如果局部过滤器规则匹配并确定处理该流量，则该规则优先生效。
2.  如果局部过滤器未能匹配流量或未对其进行处理，则网关会将流量传递给全局过滤器。
3.  全局过滤器会对剩余的流量进行过滤，并根据全局过滤器规则确定是否允许或阻止该流量。

**因此，局部过滤器的优先级高于全局过滤器**

#### 局部过滤器

局部过滤器是针对每个路由的过滤器官方提供了30种过滤器

**常用的局部过滤器：**

1.  `AddRequestHeader`：用于向请求头中添加指定的请求头信息。
2.  `AddRequestParameter`：用于向请求参数中添加指定的参数。
3.  `PrefixPath`：用于给请求路径添加前缀。
4.  `RemoveRequestHeader`：用于移除请求头中指定的请求头信息。
5.  `RemoveRequestParameter`：用于移除请求参数中指定的参数。
6.  `RewritePath`：用于重写请求路径，可以使用正则表达式进行匹配和替换。
7.  `RewriteResponseHeader`：用于重写响应头中指定的响应头信息。
8.  `SetPath`：用于设置请求路径，可以在请求路径中插入变量。
9.  `SetRequestHeader`：用于设置请求头中指定的请求头信息。
10. `SetRequestHostHeader`：用于设置请求头中的 Host 头信息。
11. `SetRequestParameter`：用于设置请求参数中指定的参数。
12. `SetResponseStatus`：用于设置响应状态码。

#### 全局过滤器

通过实现自定义过滤器实现 `Ordered`接口 和 `GlobalFilter`接口&#x20;

1.  `getOrder()` 方法的返回值是 每个过滤器都有一个顺序值，值越小表示优先级越高。在过滤器链中，按照顺序依次执行各个过滤器。如果多个过滤器具有相同的顺序值，则根据它们被注册的顺序来决定执行顺序。
2.  `filter()` 方法是过滤的内容&#x20;
    1.  `exchange `是请求和响应的封装对象
    2.  `chain` 用来放行请求
3.  使用`@Component` 注解将这个过滤器添加到spring容器中

### 启动配置

1.  导入依赖
    ```xml
     <!--新增gateway，不需要引入web和actuator模块-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-gateway</artifactId>
            </dependency>

    ```
2.  application配置文件
    ```yaml
    server:
      port: 9527

    spring:
      application:
        name: cloud-gateway
      cloud:
        gateway:
          routes:
           - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
             uri: http://localhost:8001   #匹配的地址路径
             predicates:
               - Path=/payment/get/**   #断言,路径相匹配的进行路由
     
           - id: payment_routh2
             uri: http://localhost:8001
             predicates:
               - Path=/payment/lb/**   #断言,路径相匹配的进行路由

    eureka:
      instance:
        hostname: cloud-gateway-service
      client:
        service-url:
          register-with-eureka: true
          fetch-registry: true
          defaultZone: http://localhost:7001/eureka


    ```
3.  通过服务名进行
    ```yaml
    server:
      port: 9527

    spring:
      application:
        name: cloud-gateway
      cloud:
        gateway:
          discovery:
            locator:
              enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名进行路由
          routes:
            - id: payment_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
              #uri: http://localhost:8001   #匹配后提供服务的路由地址
              uri: lb://cloud-payment-service
              predicates:
                - Path=/payment/get/**   #断言,路径相匹配的进行路由
     
            - id: payment_routh2
              #uri: http://localhost:8001   #匹配后提供服务的路由地址
              uri: lb://cloud-payment-service
              predicates:
                - Path=/payment/lb/**   #断言,路径相匹配的进行路由

    eureka:
      instance:
        hostname: cloud-gateway-service
      client:
        service-url:
          register-with-eureka: true
          fetch-registry: true
          defaultZone: http://localhost:7001/eureka

    ```
4.  配置局部过滤器
    ```yaml
    server:
      port: 9527
    spring:
      application:
        name: spring-cloud-gateway
      cloud:
        gateway:
          discovery:
            locator:
              enabled: true
          routes:
            - id: payment_routh_1 #路由的ID，没有固定规则但要求唯一，建议配合服务名
              uri: lb://spring-cloud-hystrix-client   #匹配后提供服务的路由地址
              predicates:
                - Path=/order/ok/**   #断言,路径相匹配的进行路由

            - id: payment_routh_2
              uri: lb://spring-cloud-hystrix
              predicates:
                - Path=/ok/**   #断言,路径相匹配的进行路由
              filters:
                -PrefixPath=/order #给路径前缀拼接
    ```
