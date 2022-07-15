# SqlSession

SqlSession是应用程序与持久层之间执行交互操作的一个单线程对象，其主要作用是执行持久化操作。

SqlSession对象包含数据库中所有执行SQL操作的方法，底层封装了JDBC连接，所以可以直接使用其实例来执行已映射的SQL语句。

SqlSession实例是不能被共享的，也是线程不安全的，因此其使用范围最好限定在一次请求或一个方法中，绝不能将其放在一个类的静态字段、实例字段或任何类型的管理范围中使用。使用完SqlSession对象之后，要及时将它关闭，通常可以将其放在finally块中关闭。

SqlSession对象常用方法如下所示。

-   `<T> T selectOne(String statement)`：查询方法。参数statement是在配置文件中定义的<select>元素的id。该方法返回执行SQL语句查询结果的一个泛型对象。
-   `<T>I selectOne(String statement, Object parameter)`：查询方法。参数statement是在配置文件中定义的<select>元素的id，parameter是查询所需的参数。该方法返回执行SQL语句查询结果的一个泛型对象。
-   `<E> List<E> selectList( String statement)`：查询方法。参数statement是在配置文件中定义的<select>元素的id。该方法返回执行SQL语句查询结果的泛型对象的集合。
-   `<E> List<E> selectList( String statement, Object parameter)`：查询方法。参数statement是在配置文件中定义的<select>元素的id，parameter是查询所需的参数。该方法返回执行SQL语句查询结果的泛型对象的集合。
-   `e <E> List<E> selectList(String statement, Object parameter,RowBounds rowBounds)`：查询方法。参数statement是在配置文件中定义的<select>元素的id，parameter是查询所需的参数，rowBounds是用于分页的参数对象。该方法返回执行SQL语句查询结果的泛型对象的集合。
-   `void select(String statement, Object parameter, ResultHandlerhandler)`：查询方法。参数statement是在配置文件中定义的<select>元素的id，parameter是查询所需的参数，ResultHandler对象用于处理查询返回的复杂结果集，通常用于多表查询。
-   `int insert( String statement)`：插入方法。参数statement是在配置文件中定义的<insert>元素的id。该方法返回执行SQL语句所影响的行数。
-   `int insert(String statement, Object parameter)`：插入方法。参数statement是在配置文件中定义的<insert>元素的id，parameter是插入所需的参数。该方法返回执行SQL语句所影响的行数。
-   `int update(String statement)`：更新方法。参数statement是在配置文件中定义的<update>元素的id。该方法返回执行SQL语句所影响的行数。
-   `int update(String statement, Object parameter)`：更新方法。参数statement是在配置文件中定义的<update>元素的id，parameter是更新所需的参数。该方法返回执行SQL语句所影响的行数。
-   `int delete(String statement)`：删除方法。参数statement是在配置文件中定义的<delete>元素的id。该方法返回执行SQL语句所影响的行数。
-   `int delete(String statement, Object parameter )`：删除方法。参数statement是在配置文件中定义的<delete>元素的id，parameter是删除所需的参数。该方法返回执行SQL语句所影响的行数。
-   `void commit()`：提交事务的方法。
-   `void rollback()`：回滚事务的方法。
-   `void close()`：关闭SqlSession对象。
-   `<T>T getMapper(Class<T> type)`：返回 Mapper接口的代理对象，该对象关联了Sqlsession对象，开发人员可以使用该对象直接调用方法操作数据库。参数type是Mapper的接口类型。
-   `Connection getConnection()`：获取JDBC数据库连接对象的方法。

>   注意
>
>   为了简化开发，可以将构建SqlSessionFactory对象、创建SqlSession对象等重复性代码封装到一个工具类中，然后通过工具类来创建SqlSession。
