# MyBatis Generator插件

使用ORM框架比较麻烦的一点是不仅要创建数据库和表，还要创建对应的POJO实体类和mapper映射文件，而且表结构、实体类和mapper文件三者必须保持一致。如果数据库字段发生变化，则需要同步修改这三个文件。整个过程非常烦琐，而且容易出错。

因此，MyBatis提供了强大的代码自动插件MyBatisGenerator，只需简单几步就能生成POJO实体类和mapper映射文件和mapper接口文件。

官网：http://mybatis.org/generator/configreference/xmlconfig.html

下面开始演示MyBatis Generator插件的使用。

导入依赖：

```
<dependencies>
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.4.1</version>
    </dependency>
</dependencies>
```

在pom.xml文件的build标签中添加MyBatis-generator-maven-plugin插件：

```
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.4.1</version>
            <configuration>
                <configurationFile>src/main/resources/mybatis-generator/generatorConfig.xml</configurationFile>
                <overwrite>true</overwrite>
                <verbose>true</verbose>
            </configuration>
        </plugin>
</build>
```

在resources/mybatis-generator文件夹下创建逆向工程文件generatorConfig.xml：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 数据库驱动配置-->
    <classPathEntry location="D:\lib\mysql-connector-java-8.0.29.jar"/>
    <context id="mysql" targetRuntime="MyBatis3">

        <!--不生成注释-->
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!-- Mysql数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis"
                        userId="root"
                        password="root">
        </jdbcConnection>
        <!--    默认为false，把JDBC DECIMAL 和NUMERIC类型解析为Integer；    -->
        <!--    为true时，把JDBC DECIMAL和NUMERIC类型解析为java.math.BigDecimal    -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!--model（实体）层接口映射 targetPackage：映射到项目中的具体位置；
                targetProject：是放到src/main/java下，还是放到src/main/resources下，通常选择是src/main/java-->
        <!-- targetProject：生成POJO类的位置 -->
        <javaModelGenerator targetPackage="org.example.model" targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!--XML层映射 targetPackage：映射到项目中的具体位置；
                targetProject：是放到src/main/java下，还是放到src/main/resources下，通常选择是src/main/resources-->
        <!-- targetProject：mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!--mapper层接口映射 targetPackage：映射到项目中的具体位置；
        targetProject：是放到src/main/java下，还是放到src/main/resources下，通常选择是src/main/java-->
        <!-- targetProject：mapper接口生成的的位置 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="org.example.mapper" targetProject="src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!-- 指定数据表 对应的类名-->
        <table tableName="user" domainObjectName="User"/>

    </context>
</generatorConfiguration>
```

在generatorConfig.xml中，主要配置了数据库连接、POJO实体类、Mapper接口和Mapper映射文件的保存路径等，主要具有如下内容：

- classPathEntry：数据库驱动的绝对地址
- jdbcConnection：配置数据库连接地址。
- javaModelGenerator：生成的POJO类的包名和位置。
- javaClientGenerator：生成的Mapper接口的位置。
- sqlMapGenerator：配置mapper.xml映射文件。
- table：要生成哪些表。

在IntelliJ IDEA中Maven拓展中，选择MyBatis Generator插件，进行mybatis-generator:generate生成

查看输出结果，发现还需要导入ibatis依赖项：

```
<dependency>
    <groupId>org.apache.ibatis</groupId>
    <artifactId>ibatis-core</artifactId>
    <version>3.0</version>
</dependency>
```
