---
category: featTest
excerpt: Spring MVC是Spring框架的一个模块，用于开发Web应用程序。它基于MVC（Model-View-Controller）设计模式，将应用程序的逻辑分为三个部分：模型（Model）、视图（View）和控制器（Controller）。
tags: [Java , 框架 , Spring , MVC , SpringMVC] 
title: SpringMVC  
---
# Spring MVC

## SpringMVC和MVC简单介绍

[MVC](https://www.notion.so/MVC-491afa9678c64a949e0a081b97976da7?pvs=21) 

Spring MVC是Spring框架的一个模块，用于开发Web应用程序。它基于MVC（Model-View-Controller）设计模式，将应用程序的逻辑分为三个部分：模型（Model）、视图（View）和控制器（Controller）。

Spring MVC的结构如下：

1. 模型（Model）：模型代表应用程序的数据和业务逻辑。它包括Java对象（也称为POJO）以及与数据库交互的数据访问层（DAO）。
2. 视图（View）：视图负责将模型的数据呈现给用户。它可以是JSP页面、Thymeleaf模板、HTML、JSON等。
3. 控制器（Controller）：控制器接收用户的请求，并处理请求。它负责解析用户输入、调用适当的业务逻辑，并将模型的数据传递给适当的视图进行显示。

Spring MVC的工作流程如下：

1. 客户端发送请求到前端控制器（DispatcherServlet）。
2. 前端控制器根据请求的URL找到对应的处理器映射器（Handler Mapping），确定请求对应的控制器。
3. 控制器处理请求，执行适当的业务逻辑，返回模型和视图的信息。
4. 前端控制器将模型和视图的信息传递给视图解析器（View Resolver），找到合适的视图。
5. 视图负责将模型的数据进行渲染，生成最终的响应结果。
6. 前端控制器将响应结果发送给客户端。

通过这种方式，Spring MVC实现了灵活、模块化的Web应用程序开发。它提供了丰富的功能，如数据绑定、表单验证、拦截器、国际化等，使得开发Web应用变得更加简单和高效。

## SpringMVC的作用

1. 接收前台提交的表单数据
2. 将后台数据响应到前台
3. 页面的分发转向

## SpringMVC的特点

1. 强大的灵活性，非侵入和可配置性
2. 提供了一个前端控制器DispatcherServlet，开发的时候不需要配置额外的开发控制器对象。
3. springMVC中所有模块都有自己的分工，每一个模块都有自己的功能，包括验证器、控制器、命令对象、模型视图等模块。
4. 可以自动绑定前台提交的表单数据，并且支持自动转化
5. 对应异步请求的处理更加方便

## SpringMVC的入门案例

### Springmvc配置文件书写

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-4.1.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop-4.1.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd ">

    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
	
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <context:component-scan base-package="com.java.web"/>
</beans>
```

### web.xnl配置

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-config.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>
```

## java类的书写

```java
**package com.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * @author lunch
 */
@Controller
public class HelloController {
    
    @RequestMapping("/hello")
    public String hello(){
        return "forward:hello";
    }
}**
```

## SpringMVC执行流程

![Snipaste_2023-07-24_10-16-58.png](../images/Spring%20MVC//Spring%20MVC%20b49bd36ed87d40b3b3d8014bf5ee12a2/Snipaste_2023-07-24_10-16-58.png)

1. 浏览器发送请求到前端控制器 前端控制器 将`URL`地址转换成为`URI`地址(URI地址没有前面的项目地址)将这个URI地址交给`HandlerMapping`控制器映射器
2. `HandlerMapping`控制器映射器 通过扫描出来注解生成的Mapping映射通过 key-value 找到具体的方法 key值是URI || value的值是对应是方法的全限定名将找到的方法全限定方法名封装到HEC：HandlerExcuteerChian处理器执行链 这个 处理器执行链 里封装了这个方法执行前需要执行的拦截器等 将 处理器执行了链 返回到前端控制器
3. 前端控制器 在交给处理器适配器处理 在这之前会执行所需要的所有拦截器以及将 req，resp交给处`HanderAdapter` 处理器适配器 ，`HanderAdapter` 处理器适配器 通过消息转换器  将前端的发来的数据进行 格式转换(类型转换数据封装) 和 数据验证 将数据交给我们的处理器
4. 处理器就是我们书写的方法 方法执行将我们需要的数据放入`Model` 对象 返回值是一个逻辑视图 这些东西会被封装成ModelAndView返回到前端控制器
5. 前端控制器交给(`ViewResolver`)视图解析器 将我们逻辑视图进行转换成为一个视图的地址 也就是进行前缀后缀的拼接
6. 前端控制器将视图进行渲染(将`model`携带的值渲染进`view`中进行展示)

## 核心配置文件解析

### web.xml配置文档

web.xml是web项目的入口配置，所有的web项目启动的第一个读取的配置

我们springMVC在web.xml中只配置了一个前端控制器，相当于springMVC框架的入口。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <!--配置的是配置文件的位置-->
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-config.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <!--
            那些路径要被SpringMVC进行拦截
                后缀拦截器 这种配置方式常见于以前的项目开发，常用的拦截规则： *.do  *.action 等
                使用/进行配置 这中配置会拦截所有除了jsp的请求 一般使用这个
                该规则对于rest风格，是一种开
                发习惯，通过不同的请求来描述CRUD操作。通过该拦截规则，会将所有的静态资源都拦截（css、js、
                html、图片）
                3、使用 / 进行配置
                拦截一切请求，包括jsp，如果使用/*作为拦截规则就不能使用jsp。
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

1、通过后缀配置
这种配置方式常见于以前的项目开发，常用的拦截规则： `*.do  **.aciton` 等
2、使用 / 进行配置
除了jsp所有的请求都会进行拦截，目前开发项目最常用的拦截规则，该规则对于rest风格，是一种开
发习惯，通过不同的请求来描述CRUD操作。通过该拦截规则，会将所有的静态资源都拦截（css、js、
html、图片）
3、使用 /* 进行配置
拦截一切请求，包括jsp，如果使用/*作为拦截规则就不能使用jsp。

### SpringMVC核心配置文件解析

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.1.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop-4.1.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd ">
    <!--注解扫描配置-->
    <context:component-scan base-package="com.java"/>

    <!--这个是控制映射器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    <!--控制是适配器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    <!--自动加载上面的两种驱动配置-->
    <mvc:annotation-driven/>

    <!--这个是视图解析器 拼接视图连接-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

## SpringMVC中请求中的参数的绑定

### 参数的绑定声明

1. 参数的绑定机制
    
    我们所有的后台表单的值就是key-value的形式并且都是String类型的所有我们需要在方法形参的形参名和前端表单提交的key值一致
    
    数据类型可以自动转换
    
2. 支持的参数
    
    基本数据类型 String  list set map集合 数组 pojo类型以及实体类以及关联的实体类
    
3. 使用的要求
    
    如果是基本数据类型 前端提交的表单数据和我们handler中的形参名称要一直(严格区分大小写)
    
    如果是pojo类型数据 前端表单提交的表单数据key要和pojo中的属性名一直 如果有关联对象使用 “.属性名”的方法
    
    如果是数组和集合
    
    --直接形参中通过数组或者集合来接收 不太建议使用，在springMVC框架中有一些集合被征用
    --将集合或者数组设置为一个对象的属性来使用，list、set和数组通过下标来区分，map通过key来区
    分
    
4. 如果前端提交的参数是一个josn字符串需要进行转化
    
    如果提交的数据不可以转换形参的数据类型就会报错（错误码为400） 
    
    如果不进行赋值 int 会错误 Integer不会
    

## 自定义类型转换器

我们在springMVC执行的流程中有一个模块HttpMessageConverter，会帮助我们将前台表单提交的
数据转换为我们handler方法中形参列表对应类型，如果参数在类型转换的时候发生错误就会报400的异常。
当项目中数据类型的转换有特殊情况的时候，我们可以自己定义一个类型转换器，如果我们自己定义数据
类型转换器，当前项目下所有的数据类型（转换器定义）都会通过自定义转换器来完成。比如我们写了一个
String转换int的自定义转换器并且在项目中配置了，以后所有的String转换int都会找我们自定义转换
器。一般情况下不太推荐使用自定义转换器（主要是因为它的作用域太大而我们自己水平有限）。
spring整个框架中对时间的转换有固定格式要求的，必须是yyyy/MM/dd HH:mm:ss,我们如果通过
input对应时间类型或者一些时间插件，提交数据基本上都是YYYY-MM-DD，时间转换就有一些问题。

### 需要书写一个类实现`converter`

```java
package com.java.converter;

import org.springframework.core.convert.converter.Converter;
import org.springframework.stereotype.Component;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * @author lunch
 */
@Component
public class StringToDateConverter implements Converter<String, Date> {
    //日期格式
    private final Map<String,SimpleDateFormat> dateSimpleDateFormats = new HashMap<>();
    //时间日期格式
    private final Map<String,SimpleDateFormat> dateTimeSimpleDateFormats = new HashMap<>();

    /**
     * 将时间字符串转换成为时间对象
     * 包含多种格式以及时间戳
     * @param source 代表时间的字符串
     * @return 转换完成的时间对象
     */
    @Override
    public Date convert(String source) {
        if(source.matches("^\\d+$")){
            return new Date(Long.parseLong(source));
        }
        Pattern pattern = Pattern.compile("[^0-9]");
        Matcher matcher = pattern.matcher(source);
        if (matcher.find()) {
            String result = String.valueOf(matcher.group().charAt(0));
            if(!dateSimpleDateFormats.containsKey(result)){
                putSimpleDateFormatMap(result);
            }
            if(!source.contains(":")){
                return stringToDate(result,source);
            }
            return stringToDateTime(result,source);
        }
        throw new RuntimeException("日期格式错误");
    }

    private void putSimpleDateFormatMap(String param){
        String format =
                "yyyy" +
                param +
                "MM" +
                param +
                "dd";
        dateSimpleDateFormats.put(param,new SimpleDateFormat(format));
        dateTimeSimpleDateFormats.put(param,new SimpleDateFormat(format + " HH:mm:ss"));
    }
    private Date stringToDate(String result,String source){
        try {
            return dateSimpleDateFormats.get(result).parse(source);
        } catch (ParseException e) {
            throw new RuntimeException(e.getMessage());
        }
    }
    private Date stringToDateTime(String result,String source){
        try {
            return dateTimeSimpleDateFormats.get(result).parse(source);
        } catch (ParseException e) {
            throw new RuntimeException(e.getMessage());
        }
    }
}
```

### 需要在配置文件添加相应的配置

```xml
		<!--
        我们需要将自己写的类型转换器交给spring管理
        需要ConversionServiceFactoryBean只需要在这个工厂类中配置就饿可以了
    -->
    <bean id="conversionServiceFactoryBean" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <ref bean="stringToDateConverter"/>
            </set>
    ~~~~</property>
    </bean>
		<!--还需要在上面自动加载驱动的位置 添加这个转换器工厂类对象-->
    <mvc:annotation-driven conversion-service="conversionServiceFactoryBean"/>
```

## SpringMVC将后端绑定的数据返回给前端

### 一  使用`model`对象进行参数的传递

```java
@RequestMapping("/model")
    public String model(Model model){
        model.addAttribute("name","Eveil");
        return "show";
    }
```

### 二 使用`map` 对象进行数据的传递

```java
@RequestMapping("/map")
    public String map(Map map){
        map.put("name","Eveil");
        return "show";
    }
```

### 三 使用`modelMap`对象进行数据传递

```java
@RequestMapping("/modelMap")
    public String modelMap(ModelMap modelMap){
        modelMap.put("name","Eveil");
        modelMap.addAttribute("age",19);
        return "show";
    }
```

### 四 使用 `modelAndView` 对象进行数据传递

```java
@RequestMapping("modelAndView")
    public ModelAndView modelAndView(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("name","Eveil");
        modelAndView.setViewName("show");
				return modelAndView;
    }
```

### 五 使用requset原生servlet进行数据传递

```java
@RequestMapping("/request")
    public String request(HttpServletRequest httpServletRequest){
        httpServletRequest.setAttribute("name","Eveil");
        return "show";
    }
```

## springmvc框架Handler的返回值类型

1. 返回值为String 返回的是一个逻辑视图会被视图解析器拼接
    1. 不使用视图解析器的办法可以返回的格式是”forward:show.jsp” 这个的意思是请求转发
    2. 不使用视图解析器的办法可以返回的格式是”disirect:show.jsp“ 这个的意思是重新定向
2. 返回的值是modeAndView对象自己封装一个modeAndView然后返回会包含数据和逻辑视图的信息
3. 没有返回值 返回值使用void，如果我们方法没有其他的操作，会默认返回uri对应逻辑视图。
一般情况下如果不写返回值，我们可以通过servlet对应reqeust和response进行转发或者重定向的操
作，或者可以通过response写出一个json字符串，进行异步交互。

## SpringMVC常用的注解

### @RequestMapping注解

这个注解可以写在方法上也可以写在类上

写在方法上就是规定该controller方法的访问路径

写在类上的则整个controller方法的url路径前都会在之间加上类上写的

该注解属性

value 属性就是我们 `url` 路径的这个属性 可以在只书写一个的是简写

value书写也可以书写多个`url` 可以被多个`url`访问

method 属性就是 该方法接受的请求的类型

同样可以写多个表示可以接受多种类型 

它的值是一个枚举类型 

params 属性可以进行提交数据的验证

在里面写入`”id“`则必须有id数据

`"!id"` 的含义是必须没有id的数据

`"id=1"` 的含义是必须id的值为1

`"!id=1"` 的含义是id必须不为1 无论有没有

```java
@RequestMapping(value = "requestMapping",method = RequestMethod.GET,params = "id")
    public String requestMapping(Integer id){
        System.out.println(id);
        return "show";
    }
```

### @GetMapping PostMapping注解等

      该注解和上面注解的使用方式类似 只是规定了 特定的请求类型

### @RequestParam注解

这个注解是写在方法的形参前面的

一般在使用Map集合或者多文件上传数组中添加该注解

该注解的属性

name属性：它的值可以完成和前端name属性完成映射关系而不是映射方法形参

required属性：它的值为true的时候可以完成params属性类似的功能但是他只可以设置是否必须要

defaultValue属性：默认值属性如果没有从前端接受到这个参数的值则会给予默认值

### @CookieValue

web项目如果需要获取一个cookie值，必须要获取到当前项目中所有的cookie，遍历cookie根据key
对比，找到需要使用的cookie获取值。CookieValue可以直接通过cookie的key获取对应值。
该注解是在形参列表参数前面使用的，作用就是根据cookie的key获取对应值并放入到参数中。

这个注解可以放在方法的形参前面 在注解可填写参数作为key寻找cookie中的值赋值给到形参

```java
@PostMapping("/CookieValue")
    public void cookieValue(@CookieValue("id") int id){
        System.out.println(id);
    }
```

### @ResponseBody注解

注解添加在handler方法上或者返回值类型前，作用就是将当前添加注解的hanlder返回的结果（可
以是任意类型的数据）自动转换为json对象。一般在java项目中添加了该注解的handler成为接口（数据接
口）。添加该注解的方法不会跳转页面。

### @ReqeustBody注解

该注解在handler的形参列表参数前面使用，形参必须是引用数据类型（简单对象或者集合），在前台异步提
交请求并且传递json数据的时候使用，一般和前端框架配合使用的

一个方法的形参中有且只能有一个ReqeustBody注解

不能在get方法中使用！！！

### @ModelAttribute注解

该注解是添加在方法上（在控制层的方法上不是handler上），如果该注解对应的方法值类似是void，
该方法在当前类型下所有的handler前执行。如果该注解对应的方法有返回值，并且在注解后添加配置，就会
在每个handler执行的时候自动将方法返回值的值放入到model中。

### @PathVariable注解

1、RestFul介绍

RESTFUL是一种网络应用程序的设计风格，它的优点结构清晰、符合标准、易于理解、扩展方便，所以
现在的互联网开发使用该风格的项目越来越多，其风格本身没有一个完全的明确标准，只是一种设计格式。
该风格对应项目没有太多的实用性，核心价值在于如何让大多数人设计并符合REST风格的网络接口，方
便不同网站相互调用。

2、RestFul特性

在风格规范中，通过不同的请求来执行不同的操作，Rest风格中通过四种不同的请求方式来实现CRUD操
作，通过get请求表示查询，通过post请求表示添加，通过put请求表示修改，通过delet请求表示删除
传统风格url对应路径：
添加： [http://localhost:8080/springMVC/user/addUser](http://localhost:8080/springMVC/user/addUser) POST
修改： [http://localhost:8080/springMVC/user/editUserById](http://localhost:8080/springMVC/user/editUserById) POST
查询： [http://localhost:8080/springMVC/user/findById?id=1](http://localhost:8080/springMVC/user/findById?id=1) GET
删除： [http://localhost:8080/springMVC/user/deleteById?id=2](http://localhost:8080/springMVC/user/deleteById?id=2) GET
REST风格对应URL路径
添加： [http://localhost:8080/springMVC/user](http://localhost:8080/springMVC/user) POST
修改： [http://localhost:8080/springMVC/user](http://localhost:8080/springMVC/user) PUT
查询: [http://localhost:8080/springMVC/user/1](http://localhost:8080/springMVC/user/1) GET
删除: [http://localhost:8080/springMVC/user/1](http://localhost:8080/springMVC/user/1) DELE

3、注解作用
在rest风格中如果get或者delete请求传参通过url路径直接传递，我们就可以通过该注解直接在路径中
获取参数。
需要我们在设置访问路径的时候通过{名称}作为占位符来使用。

## springMVC的常用配置

### post请求的乱码问题

我们之前的解决是配置一个拦截器进行每次请求和响应的编码的处理

spring已经帮我做好了编码的处理我们只需要进行配置即可

web.xml中的配置信息

```xml
<filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <!--这个类的路径-->
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <!--编码集的设置-->
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <!--映射器表示要拦截的路径-->
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 静态资源处理

如果我们前端控制器拦截的路径规则是 / 或者 /* 的时候，会将静态资源（css、js、html、图片）都拦
截。
springMVC有两种解决静态资源放行的方式：

我需要的给我们想要的资源放行可以从外界访问

根据放行的细读可以分为两种

第一种可以对特定文件夹中的资源进行放行

```xml
<!--第一个表示的是url路径中的适配   第二个表示本地资源下的文件下的资源放行-->
    <mvc:resources mapping="/js/**" location="/js"/>
```

第二种适配所有

即在找寻所有控制器后仍任找不到就会将这个请求交给tomcat处理

```xml
	<!--默认的servlet处理-->
    <mvc:default-servlet-handler/>
```

### 统一的异常处理

框架的统一异常处理是在项目上线后添加的，为了让用户有一个比较友好的交互，千万别在项目开发期
间添加。统一异常的作用就是将页面展示错误和后台区分，让用户不能直接看见500类型的错误，但是
我们可以通过统一异常处理的类进行后台操作，比如我们可以根据错误发生的类型、地点进行处理。

配置 

创建一个异常处理的类实现HandlerExceptionResolver  在resolveException方法中进行操作

```java
public class MyExCeption implements HandlerExceptionResolver {
    @Override //返回值为一个模型视图对象可以传递数据和页面跳转    请求和响应对象               Object handler是发生错误的controller的方法      Exception ex发生的异常
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        return null;
    **}
}
```

## 文件上传

### 单文件的上传

1. 添加依赖文件
2. 在配置文件中配置
    
    ```java
    <!--文件上传配置  id是必须的而且必须是这个-->
        <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
            <!--文件上传之后对于编码格式的处理-->
            <property name="defaultEncoding" value="UTF-8"/>
            <!--最大的文件大小-->
            <property name="maxUploadSize" value="4294967296"/>
        </bean>
    ```
    
3. 类的书写
    
    ```java
    public int upload(MultipartFile multipartFile){
            int accesscode = Accesscode.createAccesscode();
    //        这个方法是获取它的文件名
            String originalFilename = multipartFile.getOriginalFilename();
            assert originalFilename != null;
    //        获取路径
            String fileEx = originalFilename.substring(originalFilename.lastIndexOf("."));
            String fileName = originalFilename.substring(0,originalFilename.lastIndexOf("."));
            File fatherPath = new File("D:\\inn-warehouse\\");
            File nowPath = new File(fatherPath,DateUtils.toString(new Date()));
            File nowFilePath = new File(nowPath, accesscode + originalFilename);
            boolean mkdirs = nowPath.mkdirs();
            InnDocument innDocument = new InnDocument(null,accesscode,fileEx,fileName,nowFilePath.getAbsolutePath(),
                    Long.toString(multipartFile.getSize()),new Date(),new Date(new Date().getTime() + 24 * 60 * 60 * 1000));
            try {
    //            这个方法是进行写入我们的本地的需要传入本地的一个路径
                multipartFile.transferTo(nowFilePath);
            } catch (IOException e) {
                e.printStackTrace();
            }
            return innDocumentMapper.insert(innDocument) != 0 ? accesscode : -1;
        }
    ```
    

### 多文件上传

需要在file的input标签上加上一个nultiple属性就可以上传的时候进行多文件选择

后端只需要使用接受单文件对象的数组对象就可以了还需要加上requestparam注解才可以

## springMVC的文件下载

```java
 @RequestMapping("/download")
    public ResponseEntity<byte[]>download(int serialNumber){
        InnDocumentDto innDocumentDto =
                innDocumentService.findInnDocumentBySerialNumberReturnDto(serialNumber);
        String fileName = innDocumentDto.getInnDocument().getFilename() +
                innDocumentDto.getInnDocument().getFileextension();
        try {
//            进行文件名的重新编码 iso8859-1
            fileName = new String(fileName.getBytes(),"iso8859-1");
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        HttpHeaders headers = new HttpHeaders();
//        设置响应头
        headers.add("Content-Disposition", "attachment; filename=\"" + fileName + "\"");
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
//        设置响应码
        HttpStatus statusCode = HttpStatus.OK;
//        将对象创建然后返回
        return new ResponseEntity<>(innDocumentDto.getFileContent(), headers, statusCode);
    }
```

## springMVC的拦截器

SpringMVC拦截器和Servlet过滤器的区别

1. 实现的技术不一样，过滤器是Servlet对功能，拦截器是框架。
2. 执行过程不一样，过滤器是在Servlet运行前和运行后执行，拦截器是在过滤器之后handler方法之前和之后执行。
3. 过滤器对所有的servlet请求都有效，拦截器是在前端控制器拦截请求后才可以继续拦截的。
4. 拦截器可以访问上下文对象（可以获取项目中所有的资源），过滤器不可以。

拦截器的执行流程

preHandle方法 在handler运行前执行的，该方法返回值是boolean类型的，如果返回true就继续执行
handler，如果返回false，不允许访问handler。
postHandle方法 在handler运行后执行的，当该方法被调用的时候，handler所有的业务逻辑都处理完
毕了，我们就可以在该方法添加一些其他的功能，或者对程序进行记录等操作。
afterCompletion方法 在视图渲染后执行，当视图渲染了，一般来说model中对应的数据就没有用，就
可以进行清理动作。

java我文件

```java
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return HandlerInterceptor.super.preHandle(request, response, handler);
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
    }
}
```

配置文件

```java
<mvc:interceptors>
        <mvc:interceptor>
            <!--拦截什么-->
            <mvc:mapping path="/**"/>
            <!--排除什么-->
            <mvc:exclude-mapping path="/*.html"/>
            <!--那个拦截器-->
            <ref bean="loginInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

拦截器链中拦截器各个方法执行的规则

什么是拦截器链？
从被拦截的地址来说，如果多个拦截器同时都对某一个地址进行了拦截，在该地址上对应的拦截器就称
为拦截器链。在一个拦截器链上拦截器的执行顺序是根据配置文件由上到下的顺序执行。
我们现在说的执行规则就是在拦截器链中，三个方法的执行规则：
执行顺序；preHanlder是按照配置文件由上到下顺序执行
postHanlder和after...是和preHandler方法倒序执行。

方法的执行规则

preHandler方法： 根据配置文件由上到下是顺序执行的，如果有一个链的总方法返回false，后面的方法
都不会执行
postHabdler方法： 根据换配置文件由下到上顺序执行的，是否自行查看的所有的第一个方法返回值，如
果都返回true就会自行，如果有一个链中的方法返回false 都不会执行
afete... 方法： 根据换配置文件由下到上顺序执行的，根据当前拦截器中第一个方法返回值结果来决
定是否执行，如果返回true就执行，如果返回false就不会执行

整合SSM

1. 导入依赖
2. 编写springmvc配置文件需要值扫描controller文件夹
3. 编写spring配置文件需要排除controller注解
4. 需要在web.xml中配置监听器lister

```java
<context-param>
    <!--全局变量导入spring配置文件-->
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mybatis.xml</param-value>
  </context-param>

  <!--这个是监听器-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
```

1. 使用逆向工程生成文件
2. 编写前端和业务代码以及控制层