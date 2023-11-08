---
category: featTest
excerpt: 动态服务发现对以服务为中心的（例如微服务和云原生）应用架构方式非常关键。Nacos支持DNS-Based和RPC-Based（Dubbo、gRPC）模式的服务发现。Nacos也提供实时健康检查，以防止将请求发往不健康的主机或服务实例。借助Nacos，您可以更容易地为您的服务实现断路器。动态配置服务让您能够以中心化、外部化和动态化的方式管理所有环境的配置。动态配置消除了配置变更时重新部署应用和服务的需要。配置中心化管理让实现无状态服务更简单，也让按需弹性扩展服务更容易。
tags: [分布式 , SpringCloud , Nacos , 微服务 , 注册中心 ,  配置中心 , CAP] 
title: Nacos 
---
# Nacos

#### CAP

CAP 是分布式系统设计中的三个核心概念，分别代表一致性（Consistency）、可用性（Availability）和分区容错性（Partition tolerance）

CAP 定理指出，在一个分布式系统中，无法同时满足 CAP 的三个特性，只能选择其中的两个

1.  一致性（Consistency）：在分布式系统中的所有节点上，数据的副本保持一致。即当对系统进行修改时，所有节点都会同步更新到最新状态。这要求系统能够提供强一致性的数据访问，但可能会牺牲可用性。
2.  可用性（Availability）：系统能够保证在任意时刻都能够正常响应用户请求，并提供正确的数据信息。即系统必须保持高可用性，尽可能不发生故障或停机，但可能会导致节点之间的数据不一致。
3.  分区容错性（Partition tolerance）：系统能够在网络分区或节点故障的情况下继续运行，而不会导致整个系统崩溃。即系统可以将节点分割成多个分区，并且能够处理节点之间的通信中断或消息丢失的情况。

Nacos的CAP默认模式AP 也就是舍弃了一致性 但是它可以通过发送请求的方式更换 模式

`PUT请求 URL：$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP`

#### 作用

1.  服务发现和管理

    动态服务发现对以服务为中心的（例如微服务和云原生）应用架构方式非常关键。Nacos支持DNS-Based和RPC-Based（Dubbo、gRPC）模式的服务发现。Nacos也提供实时健康检查，以防止将请求发往不健康的主机或服务实例。借助Nacos，您可以更容易地为您的服务实现断路器。
2.  动态配置服务

    动态配置服务让您能够以中心化、外部化和动态化的方式管理所有环境的配置。动态配置消除了配置变更时重新部署应用和服务的需要。配置中心化管理让实现无状态服务更简单，也让按需弹性扩展服务更容易。
3.  动态DNS服务

    通过支持权重路由，动态DNS服务能让您轻松实现中间层负载均衡、更灵活的路由策略、流量控制以及简单数据中心内网的简单DNS解析服务。动态DNS服务还能让您更容易地实现以DNS协议为基础的服务发现，以消除耦合到厂商私有服务发现API上的风险。

#### Nacos服务

1.  gitHub下载
2.  启动 `bin/startup.cmd`&#x20;
    1.  启动指令&#x20;

        单机启动 `startup.cmd -m standalone`

        默认为集群启动
3.  打开 `http://${srverip}:8848/nacos` (8848为默认端口)

#### Nacos注册发现配置

1.  引入依赖
    ```xml
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```
2.  application配置文件
    ```yaml

    #端口
    server:
      port: 8101
    #  在注册中心的服务吗
    spring:
      application:
        name: spring-cloud-alibaba-provider
      cloud:
        nacos:
          discovery:
    #        注册中心的地址
            server-addr: http://127.0.0.1:8848
    ```
3.  主启动类添加注解
    ```java
    @SpringBootApplication
    @EnableDiscoveryClient
    public class SpringCloudAlibabaProvider8101 {
        public static void main(String[] args) {
            SpringApplication.run(SpringCloudAlibabaProvider8101.class,args);
        }
    }
    ```

#### Navos的配置中心的配置

1.  Nacos同springcloud-config一样，在项目初始化时，要保证先从配置中心进行配置拉取，拉取配置之后，才能保证项目的正常启动
2.  springboot中配置文件的加载是存在优先级顺序的，bootstrap优先级高于application
    ```yaml
    server:
      port: 3377 

    spring:
      application:
        name: nacos-config-client
      cloud:
        nacos:
          discovery:
            server-addr: localhost:8848 #服务注册中心地址
          config:
            server-addr: localhost:8848 #配置中心地址
            file-extension: yaml #指定yaml格式的配置（yml和yaml都可以）

    #${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}
    #nacos-config-client-dev.yaml  (一定要与file-extension值保持一致) 

    ```
3.  可以在云端进行配置修改发布 他的名字是  服务名 + 环境 + 配置文件的扩展名
4.  `@RefreshScope` 配置到Controller 层可以动态的更新配置文件
5.  Nacos分类配置为三层 命名空间 / 组  / 服务实例
    ```text
     Nacos默认的命名空间是public，Namespace主要用来实现隔离。
    比方说我们现在有三个环境：开发、测试、生产环境，我们就可以创建三个Namespace，不同的 Namespace之间是隔离的。
     Group默认是DEFAULT_GROUP，Group可以把不同的微服务划分到同一个分组里面去。Service就是微服务；一个Service可以包含多个Cluster(集群)，Nacos默认Cluster是DEFAULT，Cluster是对指定微服务的一个虚拟划分。
    比方说为了容灾，将Service微服务分别部署在了杭州机房和广州机房，这时就可以给杭州机房的Service微服务起一个集群名称(HZ)，给广州机房的Service微服务起一个集群名字(GZ)，还可以尽量让同一个机房的微服务互相调用，以提升性能。
    最后是Instance，就是微服务的实例。

    ```
