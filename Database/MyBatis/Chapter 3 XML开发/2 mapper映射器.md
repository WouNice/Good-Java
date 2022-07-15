# XML 映射器

在映射文件中，<mapper>元素是映射文件的根元素，其他元素都是它的子元素。

## 定义SQL语句

MyBatis提供了insert、update、delete和delete四个标签来定义SQL语句。接下来就从SQL语句开始介绍每个标签的用法。

### select

select是MyBatis常用的元素之一，MyBatis在查询和结果映射中做了相当多的改进。一个简单查询的select元素是非常简单的，比如：

```
<select id="selectOne" parameterType="Long" resultType="hashmap">
    SELECT name,age FROM student WHERE id = #{id}
</select>
```

在上面的示例中，通过id查询学生的姓名和年龄。定义方法名为selectOne，接收一个Long类型的参数，并返回一个HashMap类型的对象。HashMap的键是列名，值是结果集中的对应值。#{id}为传入的参数符号。

select标签允许配置很多属性来配置每条语句的行为细节，比如参数类型、返回值类型等，包含的属性如表所示。

| 属性             | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| `id`             | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType`  | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| ~~parameterMap~~ | ~~用于引用外部 parameterMap 的属性，目前已被废弃。请使用行内参数映射和 parameterType 属性。~~ |
| `resultType`     | 期望从这条语句中返回结果的类全限定名或别名。 注意，如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身的类型。 resultType 和 resultMap 之间只能同时使用一个。 |
| `resultMap`      | 对外部 resultMap 的命名引用。结果映射是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。 resultType 和 resultMap 之间只能同时使用一个。 |
| `flushCache`     | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。 |
| `useCache`       | 将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。 |
| `timeout`        | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `fetchSize`      | 这是一个给驱动的建议值，尝试让驱动程序每次批量返回的结果行数等于这个设置值。 默认值为未设置（unset）（依赖驱动）。 |
| `statementType`  | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `resultSetType`  | FORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖数据库驱动）。 |
| `databaseId`     | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |
| `resultOrdered`  | 这个设置仅针对嵌套结果 select 语句：如果为 true，将会假设包含了嵌套结果集或是分组，当返回一个主结果行时，就不会产生对前面结果集的引用。 这就使得在获取嵌套结果集的时候不至于内存不够用。默认值：`false`。 |
| `resultSets`     | 这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔。 |

select标签虽然有很多属性，但是常用的是id、parameterType、resultType、resultMap这4个属性。需要注意的是，resultMap是MyBatis的强大特性之一，如果对其理解透彻，许多复杂的映射问题都能迎刃而解。

### insert

insert标签主要用于定义插入数据的SQL语句，例如：

```
<insert id="insert" parameterType="org.example.model.Student" >
    INSERT INTO
    student
    (id,name,sex,age)
    VALUES
    (#{id},#{name}, #{sex}, #{age})
</insert>
```

在上面的示例中，插入语句的配置规则更加复杂，同时提供了额外的属性和子元素用来处理主键的生成方式。

如果数据库包含自动生成主键的字段，那么可以设置useGeneratedKeys="true"，然后把keyProperty设置为目标属性。比如，上面的Student表已经在id列上使用了自动生成主键，那么语句可以修改为：

```
<insert id="insert" useGeneratedKeys="true" keyProperty="id" parameterType="org.example.model.Student" >
    INSERT INTO
    student
    (name,sex,age)
    VALUES
    (#{name}, #{sex}, #{age})
</insert>
```

在上面的示例中，设置useGeneratedKeys="true"，然后设置keyProperty="id"对应的主键字段。

insert标签包含的属性如表所示。

| 属性               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| `id`               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType`    | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| ~~`parameterMap`~~ | ~~用于引用外部 parameterMap 的属性，目前已被废弃。请使用行内参数映射和 parameterType 属性。~~ |
| `flushCache`       | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：（对 insert、update 和 delete 语句）true。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `statementType`    | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用 getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（`unset`）。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`        | （仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `databaseId`       | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |

常用的属性有id、parameterType、useGeneratedKeys、keyProperty等，部分属性和select标签是一致的。

### update

update标签和insert标签类似，主要用来映射更新语句，示例代码如下：

```
<update id="update" parameterType="org.example.model.Student" >
    UPDATE
    student
    SET
    name = #{name},
    sex = #{sex},
    age = #{age}
    WHERE
    id = #{id}
</update>
```

如果需要根据传入的参数来动态判断是否进行修改，可以使用if标签动态生成SQL语句，示例代码如下：

```
<update id="update" parameterType="org.example.model.Student" >
    UPDATE
    student
    SET
    <if test="name != null">name = #{name},</if>
    <if test="sex != null">sex = #{sex},</if>
    age = #{age}
    WHERE
    id = #{id}
</update>
```

在上面的示例中，通过if标签判断传入的name参数是否为空，实现根据参数动态生成SQL语句。

update标签包含的属性和insert标签基本一致。

### delete

delete标签用来映射删除语句。

```
<delete id="delete" parameterType="Long" >
    DELETE FROM
    student
    WHERE
    id =#{id}
</delete>
```

delete标签除了少了useGeneratedKeys、keyProperty和keyColumn三个属性之外，其余的和insert、update标签一样。

## 结果映射

结果映射是MyBatis重要的功能之一。对于简单的SQL查询，使用resultType属性自动将结果转换成Java数据类型。不过，如果是复杂的语句，则使用resultMap映射将查询结果转换为数据实体关系。

### resultType

前面介绍select标签的时候提到，select标签的返回结果可以使用resultMap和resultType两个属性指定映射结构。下面就来演示resultType的用法。

```
<select id="selectOne" resultType="org.example.model.Student">
  select *
  from student
  where id = #{id}
</select>
```

通过设置resultType属性，MyBatis会自动把查询结果集转换为Student实体对象。当然，如果只查询部分字段，则可以返回HashMap，比如：

```
<select id="selectName" parameterType="Long" resultType="hashmap">
    select name,age from student where id = #{id}
</select >
```

上述语句，Mybatis会自动将所有的列映射成HashMap对象。

### resultMap

在日常开发过程中，在大部分情况下resultType就能满足。但是使用resultType需要数据库字段和属性字段名称一致，否则就得使用别名，这样就使得SQL语句变得复杂。

所以MyBatis提供了resultMap标签定义SQL查询结果字段与实体属性的映射关系。下面演示resultMap的使用。

首先，定义resultMap：

```
<resultMap id="BaseResultMap" type="org.example.model.Student" >
    <id column="id" property="id" jdbcType="BIGINT" />
    <result column="name" property="name" jdbcType="VARCHAR" />
    <result column="sex" property="sex" javaType="INTEGER"/>
    <result column="age" property="age" jdbcType="INTEGER" />
</resultMap>
```

在上面的示例中，我们使用resultMap标签定义了BaseResultMap映射关系，将数据库中的字段映射为Student实体对象。

然后，在SQL语句中使用自定义的BaseResultMap映射关系，设置select标签的resultMap="BaseResultMap"，示例代码如下：

```
<select id="selectOne" parameterType="Long" resultMap="BaseResultMap" >
    SELECT
    *
    FROM student
    WHERE id = #{id}
</select>
```

在上面的示例中，将之前的resultType改为resultMap="BaseResultMap"。

### resultMap的结构

resultMap标签的结构比较复杂，包含很多子属性和子标签。resultMap标签包含id和type属性：

-   id定义该resultMap的唯一标识。
-   type为返回值的类名。

同样，resultMap还可以包含多个子标签，包括：

-   id标签用于设置主键字段与领域模型属性的映射关系，此处主键为id，对应数据库字段中的主键ID。
-   result标签用于设置普通字段与领域模型的属性映射关系。
-   association标签用于配置一对一结果映射，可以关联resultMap中的定义，或者对其他结果映射的引用。
-   collection标签用于配置一对多结果映射，可以关联resultMap中的定义，或者对其他结果映射的引用。

下面是完整的resultMap元素的结构：

```
<resultMap id="StudentAndClassMap" type="org.example.model.Student" >
    <id column="id" property="id" jdbcType="BIGINT" />
    <result column="name" property="name" jdbcType="VARCHAR" />
    <result column="sex" property="sex" javaType="INTEGER"/>
    <result column="age" property="age" jdbcType="INTEGER" />
    <!--association 用户关系映射 查询单个完整对象-->
    <association property="classes" javaType="org.example.model.Classes">
        <id column="id" property="id"/>
        <result column="class_name" property="name" jdbcType="VARCHAR" />
        <result column="memo" property="memo" jdbcType="VARCHAR" />
    </association>
</resultMap>
```
