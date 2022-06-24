# 使用MyBatis注解

MyBatis注解的最大特点是取消了Mapper的XML配置，通过@Insert、、@Update、@Select、@Delete等注解将SQL语句定义在Mapper接口方法中或SQLProvider的方法中，从而省去了XML配置文件。这些注解和参数的使用与mapper.xml配置文件基本一致。

下面就来演示使用MyBatis注解实现数据查询。

### 1. 修改配置文件

首先创建Spring Boot项目，集成MyBatis的过程与XML配置方式一样。

使用注解方式只需要在application.properties中指明实体类的包路径，其他保持不变，配置示例如下：

```
mybatis.type-aliases-package=org.example.mapper
```

在上面的示例中，配置了mapper接口的包路径，无须配置mapper.xml文件的路径。

### 2. 添加mapper接口

使用MyBatis提供的SQL语句注解无须再创建mapper.xml映射文件，创建mapper接口类，然后添加相关的方法即可：

```
public interface StudentMapper {
    @Select("select * from student")
    List<Student> selectAll();
}
```

在上面的示例中，使用@Select注解定义SQL查询语句即可实现查询所有学生列表的功能，无须再定义mapper.xml映射文件。
