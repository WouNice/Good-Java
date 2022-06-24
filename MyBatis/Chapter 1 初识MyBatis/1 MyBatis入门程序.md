# MyBatis入门程序

新建一个Maven项目

导入maven依赖

```
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.10</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.29</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
    </dependency>
</dependencies>
```

创建数据库连接信息配置文件`mybatis.properties`：

```
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.url=jdbc:mysql://localhost:3306/mybatis
mysql.username=root
mysql.password=root
```

搭建实验数据库：

```
CREATE DATABASE `mybatis`;

USE `mybatis`;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user`
(
    `id`   int(20) NOT NULL,
    `name` varchar(30) DEFAULT NULL,
    `pwd`  varchar(30) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8;

insert into `user`(`id`, `name`, `pwd`)
values (1, '用户1', '123456'),
       (2, '用户2', 'abcdef'),
       (3, '用户3', '987654');
```

创建POJO实体:

```
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class User
{
    private int id;  //id
    private String name;  //姓名
    private String pwd;  //密码
}
```

创建Mybatis的核心配置文件`mybatis-config.xml`

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--    环境配置-->
    <!--    加载类路径下的属性文件-->
    <properties resource="mybatis.properties"/>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--    数据库连接相关配置，mybatis.properties文件中的内容-->
            <dataSource type="POOLED">
                <property name="driver" value="${mysql.driver}"/>
                <property name="url" value="${mysql.url}"/>
                <property name="username" value="${mysql.username}"/>
                <property name="password" value="${mysql.password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

创建映射文件UserMapper.xml:

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.example.UserMapper">
    <select id="getUserList" resultType="org.example.User">
        select * from mybatis.user
    </select>
</mapper>
```

编写测试类:

```
public class UserTest
{
    @Test
    public void userTest()
    {
        String resources = "mybatis-config.xml";
        //创建流
        Reader reader = null;
        try
        {
            //读取mybatis-config.xml文件内容到reader对象中
            reader = Resources.getResourceAsReader(resources);
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
        //初始化Mybatis数据库，创建SqlSessionFactory类的实例
        SqlSessionFactory sqlMapper = new SqlSessionFactoryBuilder().build(reader);
        //创建SqlSession实例
        SqlSession session = sqlMapper.openSession();
        List<User> userList = session.selectList("getUserList");
        for (User user : userList)
        {
            System.out.println(user);
        }
        //关闭session
        session.close();
    }
}
```

在 MyBatis 中有些资源只需要加载一次，并且每次做查询时都是大量相同的代码，因此我们可以抽取出一个工具类，专门用来加载资源。

创建一个 MybatisUtils.java 文件作为工具类：

```
public class MybatisUtils
{
    public static final SqlSessionFactory sessionFactory;

    static
    { // 由于这些东西只需要加载一次，所以放入 static 代码块中
        // 1.获取 SqlSessionFactoryBuilder
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 2.加载映射文件
        InputStream resourceAsStream = null;
        try
        {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        }
        catch (IOException e)
        {
            e.printStackTrace();
        }
        // 3.获取 sessionFactory
        sessionFactory = sqlSessionFactoryBuilder.build(resourceAsStream);
    }

    public static SqlSession openSession()
    {
        return sessionFactory.openSession();
    }

}
```

抽取出工具类之后，之前的代码就变得十分简洁了。

修改测试类 MyTest.java：

```
public class UserTest
{
    @Test
    public void userTest()
    {
        // 调用Mybatis工具类
        SqlSession session = MybatisUtils.openSession();

        List<User> userList = session.selectList("getUserList");
        for (User user : userList)
        {
            System.out.println(user);
        }
        //关闭session
        session.close();
    }
}
```

