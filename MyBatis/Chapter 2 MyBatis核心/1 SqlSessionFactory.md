# SqlSessionFactory

SqlSessionFactory是单个数据库映射关系经过编译后的内存镜像，用于创建SqlSession。

SqlSessionFactory对象的实例通过SqlSessionFactoryBuilder对象来构建，通过XML配置文件或一个预先定义好的Configuration实例构建出SqlSessionFactory的实例。

通过XML配置文件构建出SqlSessionFactory实例的实现代码如下：

```
try
{
    InputStream inputStream = Resources.getResourceAsStream("配置文件位置");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
}
catch (IOException e)
{
    throw new RuntimeException(e);
}
```

SqlSessionFactory对象是线程安全的，一旦被创建，在整个应用执行期间都会存在。

如果多次创建同一个数据库的SqlSessionFactory，那么此数据库的资源将很容易被耗尽。所以在构建SqlSessionFactory实例时，建议使用单列模式。