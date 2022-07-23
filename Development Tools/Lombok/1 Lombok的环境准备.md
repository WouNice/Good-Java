# Lombok的环境准备

1、安装lombok插件

>   为什么要安装Lombok插件？
>
>   因为lombok的引入使得java文件使用javac编译成字节码文件中包含get/set函数,但是源代码中找不到定义，IDE会报错，因此需要安装lombok插件

2、在pom.xml中引入依赖

```
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.24</version>
</dependency>
```
