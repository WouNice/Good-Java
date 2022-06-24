# SpringMVC组件解析

Spring MVC 涉及到的组件的功能说明如下。

## 前端控制器：DispatcherServlet

DispatcherServlet 是整个流程控制的中心，它就相当于 MVC 模式中的 C，Spring MVC 的所有请求都要经过 DispatcherServlet 来统一分发。

DispatcherServlet 相当于一个转发器或中央处理器，控制整个流程的执行，对各个组件进行统一调度，以降低组件之间的耦合性，有利于组件之间的拓展。

## 处理器映射器：HandlerMapping

HandlerMapping 是处理器映射器，其作用是根据请求的 URL 路径，通过注解或者 XML 配置，寻找匹配的处理器（Handler）信息。

## 处理器适配器：HandlerAdapter

HandlerAdapter 是处理器适配器，其作用是根据映射器找到的处理器（Handler）信息，按照特定规则执行相关的处理器（Handler）。

## 处理器：Handler

Handler 是处理器，和 Java Servlet 扮演的角色一致。其作用是执行相关的请求处理逻辑，并返回相应的数据和视图信息，将其封装至 ModelAndView 对象中。

它就是我们开发中要编写的具体业务控制器。

## 视图解析器：ViewResolver

View Resolver 是视图解析器，其作用是进行解析操作，通过 ModelAndView 对象中的 View 信息将逻辑视图名解析成真正的视图 View（如通过一个 JSP 路径返回一个真正的 JSP 页面）。

View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名，即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

## 视图：View

View，其本身是一个接口，实现类支持不同的 View 类型（JSP、FreeMarker、Excel 等）。

**以上组件中，需要开发人员进行开发的是处理器（Handler，常称Controller）和视图（View）。通俗的说，要开发处理该请求的具体代码逻辑，以及最终展示给用户的界面。**