# README

Spring Boot是Spring生态下的一个子项目，用于快速、敏捷地开发新一代基于Spring框架的应用程序。同时，它将目前各种比较成熟的服务框架和第三方组件组合起来（如Redis、MongoDB、JPA、RabbitMQ、Quartz等），按照“约定优于配置”的设计思想封装成Starters组件。这样，我们在SpringBoot应用中几乎可以零配置地使用这些组件，达到开箱即用的效果，从而从繁杂的配置中解放出来，更加专注于业务逻辑的开发。

## Spring Boot的特性

Spring Boot的一系列特性使得微服务架构的落地变得非常容易，对于目前众多的技术栈，Spring Boot是Java领域微服务架构的最优落地技术。

Spring Boot 具有以下特点：

1. 独立运行的 Spring 项目

Spring Boot 可以以 jar 包的形式独立运行，Spring Boot 项目只需通过命令“ java–jar xx.jar” 即可运行。

2. 内嵌 Servlet 容器

Spring Boot 使用嵌入式的 Servlet 容器（例如 Tomcat、Jetty 或者 Undertow 等），应用无需打成 WAR 包 。

3. 提供 starter 简化 Maven 配置

Spring Boot 提供了一系列的“starter”项目对象模型（POMS）来简化 Maven 配置。

4. 提供了大量的自动配置

Spring Boot 提供了大量的默认自动配置，来简化项目的开发，开发人员也通过配置文件修改默认配置。

5. 自带应用监控

Spring Boot 可以对正在运行的项目提供监控。

6. 无代码生成和 xml 配置

Spring Boot 不需要任何 xml 配置即可实现 Spring 的所有配置。

## Spring Boot的优点

Spring Boot继承了Spring一贯的优点和特性，同时增加了一些新功能和新特性，这让Spring Boot非常容易上手，也让编程变得更加简单。

总结起来Spring Boot有如下几个优点：

-   遵循“约定优于配置”的原则，使用Spring Boot只需要很少的配置或使用默认的配置。
-   使用JavaConfig，避免使用XML的烦琐。
-   提供Starters（启动器），简化Maven配置，避免依赖冲突。

-   提供内嵌Servlet容器，可选择内嵌Tomcat、Jetty等容器，不需要单独的Web服务器。这意味着不再需要启动Tomcat或其他任何中间件。
-   提供了一系列项目中常见的非功能特性，如安全监控、应用监控、健康检测等。
-   与云计算、微服务的天然集成。

从软件发展的角度来讲，越简单的开发模式越流行，越有活力，其可以让开发者将精力集中在业务逻辑本身，提高软件开发效率。Spring Boot就是尽可能地简化应用开发的门槛，让应用开发、测试、部署变得更加简单。
