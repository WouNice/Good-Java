# Web配置

本节介绍Spring Boot Web中非常重要的类：WebMvcConfigurer。有时我们需要自定义Handler、Interceptor、ViewResolver、MessageConverter实现特殊的Web配置功能，通过WebMvcConfigurer接口即可实现项目的自定义配置。

## WebMvcConfigurer简介

在Spring Boot 1.5版本都是靠重写WebMvcConfigurerAdapter的方法来添加自定义拦截器、消息转换器等。Spring Boot 2.0以后，该类被标记为@Deprecated（弃用）。官方推荐直接实现WebMvcConfigurer接口或者直接继承WebMvcConfigurationSupport类。

WebMvcConfigurer配置类其实是Spring内部的一种配置方式，采用JavaBean的形式来代替传统的XML配置文件形式进行针对框架的个性化定制，可以自定义Handler、Interceptor、ViewResolver、MessageConverter。基于java-based方式的Spring MVC配置需要创建一个配置类并实现WebMvcConfigurer接口。

## 跨域访问

出于安全的考虑，浏览器会禁止Ajax访问不同域的地址，而在如今微服务横行的年代，跨域访问是非常常见的。这就需要应用系统既要保证系统安全，又要对前端跨域访问提供支持。所以W3C提出了CORS（Cross-Origin-Resource-Sharing）跨域访问规范，并被主流浏览器所支持。

Spring Boot可以基于CORS解决跨域问题，CORS是一种机制，告诉后台哪边（Origin）来的请求可以访问服务器的数据。WebMvcConfigurer配置类中的addCorsMappings()方法是专门为开发人员解决跨域而诞生的接口，其中构造参数为CorsRegistry，示例代码如下：

```
@Override
public void addCorsMappings(CorsRegistry registry) {
    super.addCorsMappings(registry);
    registry.addMapping("/cors/**")
            .allowedHeaders("*")
            .allowedMethods("POST","GET","DELETE","PUT")
            .allowedOrigins("*");
}
```

从上面的示例代码可以看出，将pathPattern设置为/**，即整个系统支持跨域访问。当然也可以根据不同的项目路径定制访问行为。CorsRegistry提供了registrations属性，通过getCorsConfigurations()方法设置特定路径的跨域访问。

## 数据转换配置

Spring Boot支持对请求或返回的数据类型进行转换，常用到的是统一对返回的日期数据自动格式化。配置如下：

```
//定义时间格式转换器
@Bean
public MappingJackson2HttpMessageConverter jackson2HttpMessageConverter() {
    MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
    ObjectMapper mapper = new ObjectMapper();
    mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    mapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
    converter.setObjectMapper(mapper);
    return converter;
}

//添加转换器
@Override
public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
    //将我们定义的时间格式转换器添加到转换器列表中
    //这样jackson格式化时但凡遇到Date类型就会转换成我们定义的格式
    converters.add(jackson2HttpMessageConverter());
}
```

在上面的示例中，首先创建一个MessageConverter时间格式转换器，将设置时间的格式为"yyyy-MM-dd HH:mm:ss"，然后configureMessageConverters方法将转换器添加到系统中。这样JSON数据格式化时，统一将时间类型转换成我们定义的格式。

## 静态资源

在开发Web应用的过程中，需要引用大量的JS、CSS、图片等静态资源。Spring Boot默认提供静态资源的目录置于classpath下，目录名规则如下：

```
/static
/public
/resources
/META-INF/resources
```

比如，我们可以在src/main/resources/目录下创建static，在该位置放置一个文件名为xx.jpg的图片。启动程序后，访问http://localhost:8080/xx.jpg即可访问该图片，无须其他额外配置。

Spring Boot同样支持自定义静态资源目录，如果需要自定义静态资源映射目录，只需重写addResourceHandlers()方法即可，示例代码如下：

```
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    // 处理静态资源，例如图片、JS、CSS等
    registry.addResourceHandler("/images/**").addResourceLocations("classpath:/images/");
}
```

在上面的示例中，创建的webconfig类继承自WebMvcConfigure类，重写了addResourceHandler()方法，通过addResourceHandler添加映射路径，然后通过addResourceLocations来指定路径。

-   addResourceLocations指的是文件放置的目录。
-   addResoureHandler指的是对外暴露的访问路径。

## 跳转指定页面

以前编写Spring MVC的时候，如果需要访问一个页面，必须要在Controller类中编写一个页面跳转的方法。Spring Boot重写WebMvcConfigurer中的addViewControllers()方法即可达到同样的效果。示例代码如下：

```
@@Override
 public void addViewControllers(ViewControllerRegistry registry) {
     super.addViewControllers(registry);
     registry.addViewController("/").setViewName("/index");
     // 实现一个请求到视图的映射，无须编写controller
     registry.addViewController("/login").setViewName("forward:/index.html");
 }
```

值得指出的是，在这里重写addViewControllers()方法并不会覆盖WebMvcAutoConfiguration中的addViewControllers（在此方法中，Spring Boot将“/”映射至index.html），这就意味着我们自己的配置和Spring Boot的自动配置同时有效，这也是推荐添加自己的MVC配置的原因。