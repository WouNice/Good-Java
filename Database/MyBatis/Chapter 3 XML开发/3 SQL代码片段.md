# SQL代码片段

当多个SQL语句中存在相同的部分，可以将其定义为公共的SQL代码片段。MyBatis提供了sql和include标签来定义和引用SQL代码片段。这样调用方便、SQL整洁。比如：

```
<sql id="Base_Column_List">
    id,name,age,sex
</sql>
```

通过sql标签定义可以重新使用的SQL代码片段Base_Column_List，然后在SQL语句中引用Base_Column_List，示例代码如下：

```
<select id="selectAll" resultMap="BaseResultMap" >
    SELECT
    <include refid="Base_Column_List" />
    FROM student
</select>
```

在上面的示例中，使用include标签引用定义的Base_Column_List代码片段。

```
<sql id="Base_Column_List">
    ${alias_s}.id,${alias_s}.name,${alias_s}.age,${alias_s}.sex
</sql>
```

在上面的示例中，通过${}定义查询表的别名作为传入参数。当引用此代码片段时，可以传入表的别名，比如：

```
<select id="selectOne" resultMap="BaseResultMap" >
    SELECT
    <include refid="BaseResultMap"><property name="alias_s" value="stu"/></include>,
    FROM student stu
    WHERE stu.id=#{id}
</select>
```

在上面的示例中，使用include标签引用定义的SQL代码片段，并且通过property定义传入的参数。