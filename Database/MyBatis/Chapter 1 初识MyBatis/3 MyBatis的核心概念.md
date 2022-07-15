# MyBatis的核心概念

## Mapper配置文件

可以使用基于XML的Mapper配置文件来实现，也可以使用基于MyBatis注解来实现，甚至可以直接使用MyBatis提供的API来实现。

MyBatis的配置文件包括主配置文件、映射文件：

- SqlConfig.xml是MyBatis的主配置文件，它是全局的，保存着数据库的连接信息、运行环境等。
- mapper.xml 是sql 映射文件，里面记录了对数据库进行操作的sql语句，映射文件需要在主配置文件SqlConfig.xml里显式的加载。

## SqlSessionFactory（会话工厂）

SqlSessionFactory是会话工厂，用来创建各种会话，它实际上一个接口，在该接口中定义了openSession的不同的加载方法，一般用单例模式来管理SqlSessionFactory，便于重复使用。

SqlSessionFactory是单个数据库映射关系经过编译后的内存镜像，其对象的实例可以通过SqlSessionFactoryBuilder对象类获得。

## SqlSessionFactoryBuilder（构建器）

用于解析配置文件，包括属性配置、别名配置、拦截器配置、数据源和事务管理器等，可以从XML配置文件或一个预定义的配置实例进行构建。

## SqlSession（会话）

 SqlSession是面向用户的，即一个用户，对应一个会话，它定义了数据库的操作方法。

类似于JDBC中的连接（Connection），SqlSession对象完全包含数据库相关的所有执行SQL操作的方法，它的底层封装了JDBC连接，可以用SqlSession实例来直接执行被映射的SQL语句。

每个线程都应该有它自己的SqlSession实例。SqlSession的实例不能共享使用，因为它不是线程安全的。若打开了一个SqlSession，则在使用完毕后就需要关闭它。通常，把这个sqlsession.close()关闭操作，放到finally{}块里，以确保每次都能正确关闭。

## Executor（执行器）

MyBatis底层自定义了Exector执行器接口来具体操作数据库。Exector接口有两个实现，一个基本执行器（默认），一个是缓存执行器，SqlSession底层是通过Exector接口操作数据库。

## MappedStatement映射器

MyBatis的一个底层封装对象，它包装了MyBatis配置信息与sql映射信息等。mapper.xml中的insert/select/update/delete标签对应一个MappedStatement对象。标签的id就是MappedStatement的id。

MappedStatement对sql执行输入参数进行定义，包括HashMap、基本类型、pojo、Executor通过MappedStatement在执行sql前将输入的Java对象映射至sql中，输入参数映射就是JDBC编程对preparedStatement设置参数。

MappedStatement对sql执行输出结果进行定义，包括HashMap、基本类型、pojo，Executor通过MappedStatement在执行sql后将输出结果映射至Java对象中，输出结果映射就是JDBC编程对结果的解析处理过程。

## Mapper接口

是指自定义的数据操作接口，类似于通常所说的DAO接口。早期的Mapper接口需要自定义去实现，现在MyBatis会自动为Mapper接口创建动态代理对象。Mapper接口的方法通常与Mapper配置文件中的select、insert、update、delete等XML节点一一对应。