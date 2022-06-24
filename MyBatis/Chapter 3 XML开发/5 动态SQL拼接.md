# 动态SQL拼接

MyBatis提供了if、foreach、choose等标签动态拼接SQL语句，使用起来特别简单灵活。下面一一介绍这些标签的使用。

## if标签

if标签通常用于WHERE语句中，通过判断参数值来决定是否使用某个查询条件，它也经常用于UPDATE语句中，用于判断是否更新某一个字段，还可以在INSERT语句中使用，用来判断是否插入某个字段的值，比如：

```
<select id="selectStudentListLikeName" parameterType="org.example.model.Student" resultMap="BaseResultMap">
    select * from student
    <if test="name!=null and name!='' ">
        where name like CONCAT(CONCAT('%', #{name}),'%')
    </if>
</select>
```

在上面的示例中，使用if标签先进行判断，如果传入的参数值为null或等于空字符串，就不进行此条件的判断。

## foreach标签

foreach标签主要用于构建in条件，它可以在sql标签中对集合进行迭代。通常可以将其用到批量删除、添加等操作中。使用方法如下：

```
<delete id="deleteBatch">
　delete from student where id in
　<foreach collection="array" item="id" index="index" open="(" close=")" separator=",">
　　　#{id}
　</foreach>
/delete>
```

在上面的示例中，通过foreach标签将传入的id数组拼接成in(1,2,3)，实现批量删除的功能。

foreach标签包含如下几个属性：

-   collection：有3个值，分别是list、array和map，分别对应Java中的类型为：List集合、Array和Map三种数据类型。示例中传的参数为数组，所以值为array。
-   item：表示在迭代过程中每一个元素的别名。
-   index：表示在迭代过程中每次迭代到的位置（下标）。
-   open：前缀。
-   close：后缀。
-   separator：分隔符，表示迭代时每个元素之间以什么分隔。

## choose标签

MyBatis提供了choose标签，用于按顺序判断when中的条件是否成立，如果有一个成立，则choose结束。如果choose中的所有when条件都不满足，则执行otherwise中的SQL语句。类似于Java中的switch语句，choose为switch，when为case，otherwise为default。

Choose适用于从多个选项中选择一个条件的场景，比如：

```
<select id="selectStudentListChoose" parameterType="org.example.model.Student" resultMap="BaseResultMap">
    select * from student
    <where>
        <choose>
            <when test="name!=null and name!='' ">
                 name like CONCAT(CONCAT('%', #{name}),'%')
            </when>
            <when test="sex! null ">
               AND sex = #{sex}
            </when>
            <when test="age!=null">
                AND age = #{age}
            </when>
            <otherwise>
            
            </otherwise>
        </choose>
    </where>
</select>
```

在上面的示例中，把所有可以限制的条件都写上，匹配其中一个符合条件的拼接成where条件。

## 格式化输出

MyBatis提供了where、set、trim等标签处理查询条件和update语句，使得在mapper.xml中拼接SQL语句变得非常简单高效，从而避免出错。

## set标签

```
<!-- 查询学生list，like姓名，=性别 -->
<select id="selectStudentListWhere" parameterType="org.example.model.Student" resultMap="BaseResultMap">
     select * from student
    <where>
        <if test="name!=null and name!='' ">
            name like CONCAT(CONCAT('%', #{name}),'%')
        </if>
        <if test="sex!= null ">
            AND sex= #{sex}
        </if>
    </where>
</select>
```

在上面的示例中，如果参数name为null或''，则不会生成这个条件，同时还会自动把后面多余的AND关键字去掉。这样就避免了手动判断的麻烦。

## set标签

与where标签类似，使用set标签可以为update语句动态配置set关键字，同时自动去除追加到条件末尾的任何逗号。使用方法如下：

```
<<!-- 更新学生信息 -->
<update id="updateStudent" parameterType="org.example.model.Student">
    update student
    <set>
        <if test="name!=null and name!='' ">
            name = #{name},
        </if>
        <if test="sex!=null">
            sex= #{sex},
        </if>
        <if test="age!=null ">
            age = #{age},
        </if>
        <if test="classes!=null and classes.id!=null">
            class_id = #{classes.class_id}
        </if>
    </set>
    WHERE id = #{id};
</update> >
```

在上面的示例中，使用了set和if标签，如果某个传入参数不为null，则生成该字段，无须额外处理末尾的逗号，整个update语句更加清晰简洁。

##  trim标签

trim标签的作用是在包含的内容前加上某些前缀，也可以在其后加上某些后缀，对应的属性是prefix和suffix；可以把内容首部的某些内容覆盖掉，即忽略，也可以把内容尾部的某些内容覆盖掉，对应的属性是prefixOverrides和suffixOverrides。

trim标签比Java中的trim方法功能还要强大。正因为trim有这样的功能，所以我们可以非常简单地利用trim来代替where元素的功能，比如：

```
<!-- 查询学生list，like姓名，=性别 -->
<select id="selectStudentListTrim" parameterType="org.example.model.Student" resultMap="studentResultMap">
   select * from student
    <trim prefix="WHERE" prefixOverrides="AND|OR">
        <if test="name!=null and name!='' ">
            name like CONCAT(CONCAT('%', #{name}),'%')
        </if>
        <if test="sex!= null ">
            AND sex = #{sex}
        </if>
    </trim>
</select>
```

从上面的示例可以看到，trim标签和where标签的功能类似，会自动去掉多余的AND或OR关键字。

trim标签与where标签或set标签不同的地方是，trim除了支持在首尾去除多余的字符外，还可以追加字符，比如：

```
<insert id="insertSelective" parameterType=" org.example.model.Student">
        insert into student
        <trim prefix="(" suffix=")" suffixOverrides=",">
                <if test="id!=null">
                        id,
                </if>
                <if test="name!=null">
                        name,
                </if>
                <if test="age!=null">
                        age,
                </if>
        <if test="sex!=null">
                        sex,
                </if>
        </trim>
        <trim prefix="values (" suffixOverrides=",">
                <if test="id!=null">
                        #{id,jdbcType=INTEGER},
                </if>
                <if test="name!=null">
                        #{name,jdbcType=VARCHAR},
                </if>
                <if test="age!=null">
                        #{age,jdbcType=INTEGER},
                </if>
<if test="sex!=null">
                        #{sex,jdbcType=INTEGER},
                </if>
        </trim>
</insert>
```

在上面的示例中，使用trim标签生成insert语句，通过prefix和suffix属性在首尾加上“(”和“)”，同时通过suffixOverrides属性去掉拼接字段时多余的“,”。这样省去了很多判断，使得insert语句非常简单清晰。