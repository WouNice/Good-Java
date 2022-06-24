# Web简介

Spring Boot将传统Web开发的mvc、json、validation、tomcat等框架整合，提供了spring-boot-starter-web组件，简化了Web应用配置、开发的难度，将初学者从繁杂的配置项中解放出来，专注于业务逻辑的实现。

## spring-boot-starter-web介绍

Spring Boot自带的spring-boot-starter-web组件为Web应用开发提供支持，它内嵌的Tomcat以及Spring MVC的依赖使用起来非常方便。

Spring Boot创建Web应用非常简单，先创建一个普通的Spring Boot项目，然后修改pom.xml文件添加spring-boot-starter-web依赖就可以创建Web应用。

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>2.7.0</version>
</dependency>
```

## Web项目结构

Spring Boot的Web应用与其他的Spring Boot应用基本没有区别，只是resources目录中多了static静态资源目录以及templates页面模板目录。Spring Boot Web项目结构如下所示。

Spring Boot建议的目录结构如下：

```
project-name
    +-src
        +- main
            +- java
                +- org.example.project-name
                    +- comm
                    +- model
                    +- repository
                    +- service
                    +- web
                    +- Application.java
            +- resources
                +- static
                +- templates
                +- application.properties
            +- test
    +-pom.xml
```

如上所示，其实就是把java、resources、test三大基础目录进行细化，定义每个子目录存放的文件和作用。

1）java目录下的org.example.project-name为后台java文件的根目录，包括：

-   Application.java：建议放到根目录下，是项目的启动类，注意Spring Boot项目只能有一个main()方法入口。
-   comm：建议放置公共的类，如全局的配置文件、工具类等。
-   model：主要用于实体（Entity）。
-   repository：主要是数据库访问层代码。
-   service：主要是业务类代码。
-   web：负责前台页面访问的Controller路由。

2）resources目录下包括：

-   static：存放Web访问的静态资源，如JS、CSS、图片等。
-   templates：存放页面模板。
-   application.properties：存放项目的配置信息。

3）test目录存放单元测试的代码，目录结构和java目录保持一致。

4）pom.xml用于配置项目依赖包以及其他配置。

采用Spring Boot推荐的默认配置可以省掉很多设置。当然，也可以根据技术规范进行调整。

## 实现简单的Web请求

Spring Boot不像传统的MVC框架那样必须继承某个基础类才能处理HTTP请求，只需要在类上声明@Controller注解，标注这是一个控制器，然后使用@RequestMapping注解把HTTP请求映射到对应的方法即可。具体使用如下：

```
@RestController
@EnableAutoConfiguration
public class HelloController
{
    @RequestMapping("/hello")
    public String hello()
    {
        return "Hello @Spring Boot!!! ";
    }

    public static void main(String[] args)
    {
        SpringApplication.run(HelloController.class, args);
    }
}
```

启动项目，在浏览器中访问http://localhost:8080/hello，就可以看到页面返回“Hello @Spring Boot!!!”。

启动类上必须添加注解@EnableAutoConfiguration 或 @SpringBootApplication，不加会报错：

```
org.springframework.context.ApplicationContextException: Unable to start web server; nested exception is org.springframework.context.ApplicationContextException: Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
... ...
```

@EnableAutoConfiguration就是借助@Import来收集所有符合自动配置条件的bean定义，并加载到IOC容器中。@EnableAutoConfiguration用于类或接口上，在spring boot中注解位于@SpringBootApplication注解上。

@SpringBootApplication:是Sprnig Boot项目的核心注解，目的是开启自动配置。

@SpringBootApplication注解的代码如下，这些注解中有关SpringBoot的注解只有三个，分别是：

-   SpringBootConfiguration

-   EnableAutoConfiguration

-   ComponentScan()


实际上@SpringBootApplication这个注解是这三个注解的复合注解。每次写三个会显得极其麻烦，将其整合。

@RestController 是@controller和@ResponseBody 的结合

-   @Controller 将当前修饰的类注入SpringBoot IOC容器，使得从该类所在的项目跑起来的过程中，这个类就被实例化。
-   @ResponseBody 它的作用简短截说就是指该类中所有的API接口返回的数据，甭管你对应的方法返回Map或是其他Object，它会以Json字符串的形式返回给客户端

@RequestMapping注解用于定义请求的路由地址，既可以作用在方法上，又可以作用在类上。

## @Controller和@RestController

Spring Boot提供了@Controller和@RestController两种注解来标识此类负责接收和处理HTTP请求。如果请求的是页面和数据，使用@Controller注解即可；如果只是请求数据，则可以使用@RestController注解。

### @Controller的用法

Spring Boot提供的@Controller注解主要用于页面和数据的返回。示例代码如下：

```
@RestController
@SpringBootApplication
public class WebController
{
    @RequestMapping("/index")
    public String index()
    {
        ModelMap map = new ModelMap();
        map.addAttribute("name", "thymeleaf-index");
        return "thymeleaf/index";
    }

    public static void main(String[] args)
    {
        SpringApplication.run(WebController.class, args);
    }
}
```

上面的示例用于请求/index地址，返回具体的index页面和name=thymeleaf-index的数据。在前端页面中可以通过${name}参数获取后台返回的数据并显示到页面中。

在@Controller类中，如果只返回数据到前台页面，需要使用@ResponseBody注解，否则会报错。

### @RestController的用法

Spring Boot提供的@RestController注解用于实现数据请求的处理。默认情况下，@RestController注解会将返回的对象数据转换为JSON格式。示例代码如下：

```
@RequestMapping("/people")
public Map<String, String> people()
{
    HashMap<String, String> map = new HashMap<>();
    map.put("name", "yao");
    map.put("age", "18");
    return map;
}
```

在上面的示例中，定义/people接口返回JSON格式的User数据。同时，@RequestMapping注解可以通过method参数指定请求的方式。如果请求方式不对，则会报错。

显示结果：

```
{"name":"yao","age":"18"}
```

### @RestController和@Controller的区别

@Controller和@RestController注解都是标识该类是否可以处理HTTP请求，可以说@RestController是@Controller和@ResponseBody的结合体，是这两个注解合并使用的效果。虽然二者的用法基本类似，但还是有一些区别，具体如下：

-   @Controller标识当前类是Spring MVC Controller处理器，而@RestController则只负责数据返回。
-   如果使用@RestController注解，则Controller中的方法无法返回Web页面，配置的视图解析器InternalResourceViewResolver不起作用，返回的内容就是Return中的数据。
-   如果需要返回指定页面，则使用@Controller注解，并配合视图解析器返回页面和数据。如果需要返回JSON、XML或自定义内容到页面，则需要在对应的方法上加上@ResponseBody注解。
-   使用@Controller注解时，在对应的方法上，视图解析器可以解析返回的JSP、HTML页面，并且跳转到相应页面。若返回JSON等内容到页面，则需要添加@ResponseBody注解。
-   @RestController注解相当于@Controller和@ResponseBody两个注解的结合，能直接将返回的数据转换成JSON数据格式，无须在方法前添加@ResponseBody注解，但是使用@RestController注解时不能返回JSP、HTML页面，因为视解析器无法解析JSP、HTML页面。

总之，在Web系统中使用@Controller较多，而在Web API中基本使用@RestController注解。

## @RequestMapping

@RequestMapping注解主要负责URL的路由映射。它可以添加在Controller类或者具体的方法上，如果添加在Controller类上，则这个Controller中的所有路由映射都将会加上此映射规则，如果添加在方法上，则只对当前方法生效。@RequestMapping注解包含很多属性参数来定义HTTP的请求映射规则。常用的属性参数如下：

-   value：请求URL的路径，支持URL模板、正则表达式。
-   method：HTTP请求的方法。
-   consumes：允许的媒体类型，如consumes="application/json"为HTTP的Content-Type。
-   produces：相应的媒体类型，如consumes="application/json"为HTTP的Accept字段。
-   params：请求参数。
-   headers：请求头的值。

以上属性基本涵盖了一个HTTP请求的所有参数信息。其中，value和method属性比较常用。

## @ResponseBody

@ResponseBody注解主要用于定义数据的返回格式，作用在方法上，默认使用Jackson序列化成JSON字符串后返回给客户端，如果是字符串，则直接返回。

@responseBody注解的作用是将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据，需要注意的呢，在使用此注解之后不会再走试图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。

在Controller中有时需要返回JSON格式的数据，如果想直接返回数据体而不是视图名，则需要在方法上使用@ResponseBody。使用方式如下：

```
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/getUser")
    @ResponseBody
    public User getUser(){
        User u = new User();
        u.setName("weiz222");
        u.setAge(20);
        u.setPassword("weiz222");
        return u;
    }
}
```

在上面的示例中，请求/user/getUser时，返回JSON格式的User数据。这与@RestController的作用类似。

需要注意的是，使用@ResponseBody注解时需要注意请求的类型和地址，如果期望返回JSON，但是请求URL以html结尾的页面，就会导致Spring Boot认为请求的是HTML类型的资源，而返回JSON类型的资源，与期望类型不一致，因此报出如下错误：

```
There was an unexpected error (type=Not Acceptable, status=406). Could not find acceptable representation
```

根据RESTful规范的建议，在Spring Boot应用中，如果期望返回JSON类型的资源，URL请求资源后缀就使用json；如果期望返回视图，URL请求资源后缀就使用html。