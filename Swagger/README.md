# Swagger简介

Swagger 是一个用于生成、描述和调用 RESTful 接口的 Web 服务。通俗的来讲，Swagger 就是将项目中所有（想要暴露的）接口展现在页面上，并且可以进行接口调用和测试的服务。

>   Swagger 遵循了 OpenAPI 规范，OpenAPI 是 Linux 基金会的一个项目，试图通过定义一种用来描述 API 格式或 API 定义的语言，来规范 RESTful 服务开发过程。

Swagger 官网：https://swagger.io/

Swagger Editor：https://editor.swagger.io/

参考：https://www.zhihu.com/question/63803795/answer/1780508067

## swagger-codegen

添加swagger依赖：

```
<dependency>
  <groupId>io.swagger.parser.v3</groupId>
  <artifactId>swagger-parser</artifactId>
  <version>2.1.1</version>
</dependency>
<dependency>
  <groupId>io.swagger.codegen.v3</groupId>
  <artifactId>swagger-codegen</artifactId>
  <version>3.0.34</version>
</dependency>
```

## yaml

Yaml定义中可参考对应的OpenAPI 规范

-   2.0规范文档见 https://swagger.io/specification/v2/ 或者 https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md 
-   3.0规范文档见 https://swagger.io/specification/ 或者 https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.1.md
