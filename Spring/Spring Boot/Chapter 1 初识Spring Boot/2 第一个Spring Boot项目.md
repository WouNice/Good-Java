# 第一个Spring Boot项目

本章主要介绍如何开始Spring Boot项目，通过一个简单的helloworld程序演示Spring Boot的项目结构与启动流程。

## 创建项目

可以使用IDEA来构建Spring Boot项目，也可以通过https://start.spring.io/快速创建项目

## 引入依赖

添加项目的依赖配置信息：

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.7.0</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <version>2.7.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

添加项目的插件配置信息，在<build>中添加spring-boot-maven-plugin插件，它能够以Maven的方式为应用提供Spring Boot的支持。

```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.7.0</version>
        </plugin>
    </plugins>
</build>
```

添加编码设置：

```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

上面配置spring-boot-maven-plugin构建插件，将SpringBoot应用打包为可执行的JAR或WAR文件，然后以简单的方式运行Spring Boot应用。如果需要更改为Docker构建方式，则只要更改此部分即可。

## 创建程序

步骤01 在目录src\main\java\org\example下创建HelloController，然后添加/hello的路由地址和方法，示例代码如下：

```
@Controller
@EnableAutoConfiguration
public class HelloController
{
    @RequestMapping("/hello")
    public String hello()
    {
        return "Hello @ Spring Boot!!! ";
    }

    public static void main(String[] args)
    {
        SpringApplication.run(HelloController.class, args);
    }
}
```

步骤02 运行helloworld程序。

在终端下执行：

```
mvn spring-boot:run
```

步骤03 打开浏览器，访问http://localhost:8080/hello地址，查看页面返回的结果。