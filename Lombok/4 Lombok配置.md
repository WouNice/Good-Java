# Lombok配置

Lombok 除了可以在注解中进行一些参数配置之外，还可以通过 `lombok.config` 配置文件指定默认参数配置。

1.  可以在任何目录中创建。作用于该目录和其子目录
2.  `lombok.config`中配置项`config.stopBubbling=true`指明`lombok`的根目录为当前配置文件所在目录
3.  配置文件是分层的，原则是接近源文件的配置设置优先
4.  根目录的子目录中可以创建`lombok.config`配置文件，来覆盖根目录的配置文件
5.  在配置文件的顶部，可以导入其他配置文件。`import ../conf/aaa.config`

官方文档：https://www.projectlombok.org/features/configuration

常用配置项：

```
# 默认: false，lombok默认对boolean类型字段生成的get方法使用is前缀, 通过此配置则使用get前缀
lombok.getter.noIsPrefix=true

# 默认: false，默认的set方法返回void设置为true返回调用对象本身
lombok.accessors.chain=true

# 默认: false，如果设置为 true，则get/set方法将不带get/set前缀，直接以字段名为方法名
lombok.accessors.fluent=true

# 默认: log，设置log类注解返回的字段名称
lombok.log.fieldName=logger
```