---
category: featTest
title: SpringBoot
excerpt: Spring Boot是一个开源的Java框架，用于简化Spring应用程序的创建和部署。它的主要目标是使得Spring应用程序的初始搭建变得更加简单，并且能够快速地进行原型设计和开发。Spring Boot的一个关键特性是其自动配置，这意味着它会根据你添加的依赖自动配置你的Spring应用。例如，如果你在项目中添加了数据库连接的依赖，Spring Boot会自动配置一个数据库连接池。这样，你就可以避免手动进行大量的配工作。此外，Spring Boot还内置了一个嵌入式的服务器，如Tomcat或Jetty，这意味着你无需额外部署你的应用到一个独立的服务器上，你的应用就可以运行。这大大简化了应用的部署过程。总的来说，Spring Boot通过提供一种快速、简洁的方式来创建Spring应用，使得Java开发者能够更专注于编写业务逻辑代码，而不是花费大量时间在环境配置和服务器设置上。
tags: [Spring , SpringBoot , 微服务 , 框架 , Java , 分布式] 
---
# SpringBoot

## 微服务

### 单体应用介绍

我们当前项目开发的模式，将所有的功能都在一个项目中，一个单块的应用系统就是以一个单个的单元的
方式构建的，单体应用在企业级的项目开发中包括三个模块：客户端用户界面，数据库和服务器应用系统。在
开发过程中逻辑比较清晰，如果我们需要修改项目，或者在项目中添加一个新的内容，需要我们对整个项目都进行更改。
单体应用的缺点：软件变更和后期维护受到很多限制，应用程序一小部分需要进行更新，就需要整个项目
整体的进行修改，项目的扩展性和维护性比较差。

### 微服务介绍

微服务属于一种项目架构方案，有别于单体应用，我们将一个整体的单体应用拆分，每一个项目中负责的部分少了，将部分服务创建的项目称为微服务，在整体的项目中，每一个微服务的服务器都进行单独部署，每一个微服务的项目都负责项目的一部分，我们所说的微服务就是将分布式项目中每一个小的项目的一个统称

## Spring Boot简介

1.随着微服务架构的设计思想逐渐浮出水面，要搭建一个微服务，运维和部署都变得非常复杂，spring提供
了一套解决方案：即通过springBoot来快速构建单个服务。
2.Spring-Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及
开发过程。个人理解来说Spring-Boot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像
maven整合了所有的jar包，Spring-Boot整合了其他相关联框架。

### Spring Boot优势以及核心功能

1、独立运行Spring项目
Spring Boot 可以以jar包形式独立运行，运行一个Spring Boot项目只需要通过java -jar
xx.jar来运行。
2、内嵌servlet容器
Spring Boot可以选择内嵌Tomcat、jetty或者Undertow,这样我们无须以war包形式部署项目。
3、提供starter简化Maven配置
spring提供了一系列的start pom来简化Maven的依赖加载，例如，当你使用了spring-boot-
starter-web，会自动加入如图5-1所示的依赖包。
4、自动装配Spring
Spring Boot会根据在类路径中的jar包，类、为jar包里面的类自动配置Bean，这样会极大地减少我
们要使用的配置。当然，Spring Boot只考虑大多数的开发场景，并不是所有的场景，若在实际开发中我们需要配置Bean，而Spring Boot灭有提供支持，则可以自定义自动配置。
5、准生产的应用监控
Spring Boot提供基于http ssh telnet对运行时的项目进行监控。
6、无代码生产和xml配置
Spring Boot不是借助与代码生成来实现的，而是通过条件注解来实现的，这是Spring4.x提供的新特
性。

### Spring Boot入门

1. 创建一个maven项目
2. 修改pom文件
3. 继承官方提供配置文件
    
    ```xml
    <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.7.14</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
    ```
    
4. 引入我们需要的配置
    
    ```xml
    				<dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
    ```
    
5. 需要我们书写启动类
    
    ```java
    //需要这个注解进行启动类配置
    @SpringBootApplication
    public class Demo1Application {
    
        public static void main(String[] args) {
            //这样书写
            SpringApplication.run(Demo1Application.class, args);
        }
    }
    ```
    
    需要注意的是这个启动类会自动扫描他这个包下的所有类的注解不需要我们在配置扫描
    
    但是这个最好放在所以需要注解的父路径上
    

## SpringBoot快速创建

1. 在pom中添加从插件可以打成jar包快速启动

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

1. 使用maven指令package打包成jar包
2. 使用 java -jar 启动项目

### 使用idea或者官网创建一个springboot项目

使用官网 创建 或者使用idea自己创建

需要使用2.几版本的3.0以上需要17版本

### springBoot的配置文件

`src/main/java` 写java代码的地方
`src/main/resources` 配置文件存放地址
`src/main/resources/static` web资源存放地址
`src/main/resources/public` web资源存放地址
`src/main/resources/templates` 存放模板页面的地址 （相当于jsp模板）

### springBoot配置文件的作用和规范

springBoot框架中不支持xml配置文件，使用全局的配置文件，支持两种配置文件格式：properties和yaml格式
springBoot要求配置文件必须放在resources目录下，而且名称必须是application或者application-**

配置文件的作用：
springBoot默认就添加了很多配置，如果和其他框架整合，需要配置配置，需要连接数据库、redis等
资源的时候我们也需要指定资源，配置文件的作用就是修改默认的配置。

### yaml语法规则

yaml是一个以数据为中心的语法结构，比xml或者json更加清洗的表达数据结构。
语法规则：

1、以缩进表示层级关系
2、在使用缩进的时候不要使用tab键，使用空格来写缩进
3、使用空格数量不重要，在同一个层级下使用空格数量一致就可以
4、配置文件中所有的数据大小写都是敏感的
5、配置数据的时候，数据前面必须有空格 key:  value

### yaml语法具体的配置方式

1. 语法结构
    1. 字面量 (基本数据类型 + String)
    2. 对象或者map集合
    3. 数组或者list集合

### 通过yaml配置案例

```java
@ConfigurationProperties("person")
@Component
public class Person {
    private String name;
    private int age;
    private List list;
    private Map map;
```

```java
person:
  name: wxs
  age: 18
  list:
    - a1
    - b1
  map:
    a1: a1
    b1: b2

person: {name: 王小帅,age: 19 ,list: [1,2,3] ,map: {a1: a1,b1: b1}}
```

可以直接进行赋值

## 整合mybatis

1. 添加依赖
    
    ```java
    <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.3.1</version>
            </dependency>
    
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <optional>true</optional>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter-test</artifactId>
                <version>2.3.1</version>
                <scope>test</scope>
            </dependency>
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>1.2.10</version>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.33</version>
            </dependency>
        </dependencies>
    ```
    
2. 配置配置问价
    
    ```xml
    spring:
    #  数据库连接池
      datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.222.128:3306/USER?serverTimezone=UTC&characterEncoding=utf8&useUnicode=true&useSSL=false
        username: root
        password: root
    #    解决依赖循环问题
      main:
        allow-circular-references: true
    
    #日志
    logging:
      level:
        com:
          java:
            mapper: debug
    
    #配置分页插件
    pagehelper:
      helper-dialect: mysql
      reasonable: true
      support-methods-arguments: true
    
    #配置别名扫描
    mybatis:
      type-aliases-package: com.java.mapper
    ```
    
3. 给启动类配置扫描mapper包
    
    ```java
    package com.java;
    
    import org.mybatis.spring.annotation.MapperScan;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    @MapperScan({"com.java.mapper"})
    public class SpringBoot01Application {
    
        public static void main(String[] args) {
            SpringApplication.run(SpringBoot01Application.class, args);
        }
    
    }
    ```
    

## springBoot的thymeleaf模板使用

### 模板引擎概述

springBoot框架中，舍弃了jsp动态页面技术，我们如果需要写动态页面怎么办？springboot框架提
供一种新的动态页面技术thymeleaf，就是一种模板引擎的技术。
模板引擎：是一个基于模板和数据的动态页面技术，和jsp很像，不一样的地方在于，jsp中通过表达式
和jstl标准标签库进行数据合并，需要通过servlet进行转换才可以使用，模板引擎是通过模板页面对应标
签属性进行数据合并，可以直接使用html、xml等页面作为模板，如果没有数据的时候可以直接展示页面添
加数据后和通过模板引擎合成一个新的页面。

## thymeleaf模板入门案例

1. 添加依赖
    1. 可以在创建项目的时候选择依赖：
2. 修改配置文件，添加配置：
    
    ```java
    spring:
      #  禁用缓存
      thymeleaf:
        cache: false
    ```
    
3. 在idea创建模板引擎的模板 
    
    ```java
    所有的模板页面默认都需要放在templates目录下：
    3、创建一个模板页面
    <!doctype html>
    <!--suppress ThymeleafVariablesResolveInspection -->
    <html xmlns="http://www.w3.org/1999/xhtml"
    xmlns:th="http://www.thymeleaf.org">
    <head>
    <meta charset="UTF-8">
    <title>title</title>
    </head>
    <body>
    </body>
    </html>
    ```
    
4. 创建一个模板页面
5. thymeleaf模板引擎语法
    
    ![Untitled](SpringBoot%20fa6cde29a40f4fee893a2b51c94e2777/Untitled.png)
    

## thymeleaf模板使用

### 展示复杂对象

```java
<!doctype html>
<!--suppress ThymeleafVariablesResolveInspection -->
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>title</title>
</head>
<body>
    <input type="text" th:value="${person.name}">
    <input type="text" th:value="${person.name}">
    <input type="text" th:value="${person.name}">
</body>
</html>
```

### list

```java
<table>
        <tr>
            <td>id</td>
            <td>姓名</td>
            <td>薪水</td>
        </tr>
        <tr th:each="person,vars : ${list}">
            <td th:text="${person.name}"></td>
        </tr>
    </table>
```

### 判断

```java
<span th:if="${falg}">false</span>
    <span th:unless="${falg}">true</span>
    <ul th:switch="${text}">
        <li th:case="1">1</li>
        <li th:case="2">2</li>
        <li th:case="3">3</li>
    </ul>
```

*`if` true则显示 false则不显示*

*`unless` 与 if 相反*

*`switch` 发生在父子级标签中*  `switch`  为值 `case` 为匹配的值

## SpringBoot拦截器

1. 书写一个拦截器
    
    ```java
    public class LoginInterceptor implements HandlerInterceptor {
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            return true;
        }
    }
    ```
    
2. 书写一个springboot的配置类
    
    ```java
    //表明这是一个配置类
    @Configuration
    //需要实现接口WebMvcConfigurer重写方法addInterceptors
    public class InterConfig implements WebMvcConfigurer {
        @Override
        public void addInterceptors(InterceptorRegistry registry) {
    //        使用registry添加一个拦截器并配置路径和排除路径
            registry.addInterceptor(new LoginInterceptor())
                    .addPathPatterns("/**")
                    .excludePathPatterns("/");
        }
    }
    ```
    

## springBoot整合redis

springBoot通过模板对象将redis对应操作进行了细分，现在我们可以通过创建不同的数据对象操作对
应数据类型，根据redis数据类型，有五个实体类：

| String类型   | valueOperatrions |
| ------------ | ---------------- |
| list列表类型 | ListOperatrions  |
| 哈希map类型  | HashOperatrions  |
| set类型      | SetOperatrions   |
| zset类型：   | ZsetOperatrions  |
1. 开启redis服务器
2. 添加redis依赖
    
    ```xml
    <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
            </dependency>
    ```
    
3. 配置文件中添加配置
    
    ```yaml
    spring:
      data:
        redis:
          port: 6379
          password: root
          host: 192.168.888.128
    ```
    
4. 测试
    
    ```java
    @SpringBootTest
    class Springboot04ApplicationTests {
        private final StringRedisTemplate stringRedisTemplate;
    
        Springboot04ApplicationTests(StringRedisTemplate stringRedisTemplate) {
            this.stringRedisTemplate = stringRedisTemplate;
        }
    
        @Test
        void contextLoads() {
            ValueOperations<String, String> stringStringValueOperations =
                    stringRedisTemplate.opsForValue();
        }
    }
    ```