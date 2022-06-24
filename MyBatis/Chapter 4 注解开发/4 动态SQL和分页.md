# 动态SQL和分页

## 动态SQL语句

在实际项目开发中，除了使用一些常用的增、删、改、查方法之外，也会遇到复杂的业务需求，可能需要执行一些自定义的动态SQL语句。我们知道XML配置可以使用if、choose、where等标签动态拼接SQL语句，那么使用注解方式如何实现呢？

MyBatis除了提供@Insert、@Delete这些常用的注解外，还提供了@InsertProvider、@UpdateProvider、@DeleteProvider和@SelectProvider等注解用来建立动态SQL语句。下面以按字段更新来演示动态SQL的功能。

### 1. 创建拼接SQL语句

首先创建UserSqlProvider类，并创建动态拼接SQL语句的方法。示例代码如下：

```
public class StudentSqlProvider {
    public String updateByPrimaryKeySelective(Student record) {
        BEGIN();
        UPDATE("student");
        if (record.getName() != null) {
            SET("name = #{name,jdbcType=VARCHAR}");
        }
        if (record.getSex()>=0) {
            SET("sex = #{sex,jdbcType=VARCHAR}");
        }
        if (record.getAge() >0) {
            SET("age = #{age,jdbcType=INTEGER}");
        }
        WHERE("id = #{id,jdbcType=VARCHAR}");
        return SQL();
    }
}
```

在上面的示例中，首先通过UPDATE("sys_user")方法生成Update语句，然后根据具体传入的参数动态拼接要更新的set字段，最后通过SQL()方法返回完整的SQL语句。

### 2. 调用updateByPrimaryKeySelective()方法

在provider中定义了updateByPrimaryKeySelective()方法后，就可以在mapper中引用该方法。

```
@UpdateProvider(type=StudentSqlProvider.class, method="updateByPrimaryKeySelective")
int updateByPrimaryKeySelective(Student record);
```

在上面的示例中，@UpdateProvider注解定义调用的自定义SQL方法，type参数定义动态生成SQL的类，method参数定义类中具体的方法名。

### 3. 验证测试

增加单元测试方法，验证是否生效。示例代码如下：

```
@Test
public void testUpdateByPrimaryKeySelective() {
    Student student = new Student();
    student.setId(1L);
    student.setName("weiz动态修改");
    studentMapper.updateByPrimaryKeySelective(student);
    Student stu = studentMapper.selectById(1L);
    System.out.println("name:"+stu.getName()+",age:"+stu.getAge()+",sex:"+ stu.getSex());
} 
```

查看单元测试结果，结果表明单元测试方法运行成功，根据输出结果可以看到只有name字段被修改，其他字段不受影响。说明通过@UpdateProvider动态生成Update语句验证成功。

使用SqlProvider动态创建SQL语句，除了使用@UpdateProvider注解之外，还有@InsertProvider、@SelectProvider、@DeleteProvider注解供插入、查询、删除时使用。

## 分页查询

分页查询是日常开发中比较常用的功能。MyBatis框架下也有很多插件实现分页功能，比如pageHelper。这是一款非常简单、易用的分页插件，能很好地集成在Spring Boot中。pageHelper是一款基于MyBatis的数据库分页插件，所以我们在使用它时需要使用MyBatis作为持久层框架。下面通过示例演示pageHelper实现分页查询。

### 1.添加pageHelper依赖

修改pom.xml文件，添加pageHelper依赖：

```
<!-- pagehelper -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
</dependency>
```

pageHelper提供了针对Spring Boot框架的依赖库pagehelper-spring-boot-starter，对于Spring Boot项目可以使用pagehelper-spring-boot-starter组件。

### 2. 增加pageHelper配置

在application.properties中增加pageHelper配置。

```
# 分页框架
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql
```

上面的示例是pageHelper分页框架的初始配置，主要用于指定数据库类型。

-   helperDialect：指定数据库类型，可以不配置，pageHelper插件会自动检测数据库的类型。
-   reasonable：分页合理化参数，默认为false，当该参数设置为true时，当pageNum≤0时，默认显示第一页，当pageNum超过pageSize时，显示最后一页。
-   supportMethodsArguments：分页插件会根据查询方法的参数，自动在params配置的字段中取值，找到合适的值会自动分页。
-   params：用于从对象中根据属性名取值，可以配置pageNum、pageSize，count不用配置映射的默认值。

### 3. 调用测试

增加单元测试方法，验证分页功能是否生效。示例代码如下：

```
@Test
public void testSelectListPaged() {
    PageHelper.startPage(1, 5);
    List<Student> students = studentMapper.selectAll();
    PageInfo<Student> pageInfo = new PageInfo<Student>(students);
    System.out.println("总页数:"+pageInfo.getPages()+",总条数:"+pageInfo.getTotal()+",当前页:"+pageInfo.getPageNum());
    for (Student stu : students){
        System.out.println("name:"+stu.getName()+",age:"+stu.getAge());
    }
}
```

在上面的示例中，分页的核心只有一行代码，PageHelper.startPage(page,pageSize);就标识开始分页。添加此行代码之后，pageHelper插件会通过其内部的拦截器将执行的SQL语句转化为分页的SQL语句。

查看单元测试结果，结果表明单元测试方法运行成功，控制台输出了当前页的数据列表、总页数、数据总条数和当前页。这说明使用pageHelper插件成功实现了分页功能。

使用时PageHelper.startPage(pageNum, pageSize)一定要放在列表查询的方法中，这样在查询时会查出相应的数据量以及总数。