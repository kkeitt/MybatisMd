#### `<where>`标签

如果该标签包含的元素中有返回值，就插入一个where；如果where后面的字符串是以AND,OR开头的，就将它们剔除。

例：

```xml
<select id="select" resultType="user">
	select id,username,password
    from user
    <where>
    	<if test="username!=null and username!=''">
        	and username like concat('%',#{username},'%')
        </if>
    </where>
</select>
```

当if标签的test条件不成立时，where标签中没有内容，所以不会插入where。

如果成立，插入一个where，同时因为标签内的内容以AND开头，where会自动将其剔除。

#### ``<set>``标签

如果该标签包含的元素中有返回值，就插入一个set，如果set后面的字符串是以逗号结尾的，将这个逗号剔除。

例：

```xml
<update id="update">
    update user
    <set>
    	<if test="username!=null and username!=''">
        	username = #{username},
        </if>
        <if test="password!=null and password!=''">
        	password = #{password},
        </if>
        id = #{id},
    </set>
    where id = #{id}
</update>
```

如果if条件成立，插入一个set，如果字符串以逗号结尾，剔除逗号。

如果不成立，则不插入set。

#### ``<trim>``标签

where标签和set标签都可以用trim标签来实现，实际上底层就是通过trim实现的。

trim标签有以下属性：

- prefix：当trim元素内包含内容时，会给内容增加prefix指定的前缀
- prefixOverrides：当trim元素内包含内容时，会把内容中匹配的前缀字符串去掉
- suffix：当trim元素内包含内容时，会给内容增加suffix指定的后缀
- suffixOverrides：当trim元素内包含内容时，会去掉内容中匹配的后缀字符串

#### ``<foreach>``标签

foreach标签可以对数组，Map，或实现了Iterable接口的对象（List，Set）进行遍历。

数组在处理时会转为List处理，因此实际上可以用foreach元素遍历的对象分为两大类：

- 实现了Iterable接口的类
- Map

foreach标签包含以下属性：

- collection：要迭代的属性名
- item：临时变量名，值为迭代出的每一个值
- index：当前迭代值的索引的属性名，如果迭代对象是数组或集合，为下标，如果是Map，这个值为Map的key
- open：循环内容字符串的前缀
- close：循环内容字符串的后缀
- separator：分隔符

例：

```xml
<delete id="deleteByIds">
    delete from user
    where id in
    <foreach collection="ids" item="item" separator="," open = "(" close=")">
    	#{item}
    </foreach>
</delete>
```

ids是一个List，里面有三个整数（1，2，3）解析出的实际SQL为

```sql
delete from user where id in
(
	1,2,3
)
```

如果不通过**@Param**注解指定参数名，则List参数的默认参数名为**list**，数组类型参数的默认参数名为**array**，Map类型参数的默认参数名为**_parameter**。

#### ``<bind>``标签

bind标签可以使用OGNL表达式创建一个变量并将其绑定到上下文中。

例如：我们在模糊匹配字符串的时候，通常会使用concat函数。

```sql
select id ,username,password
from user 
where username like concat('%',#{username},'%')
```



但是不同数据库的concat函数实现是不一样的，MySQL中concat函数支持多个参数，而Oracle中concat只支持两个参数。

如果数据库由MySQL更换到Oracle，上面的SQL语句就不能运行了。

这时，我们可以用bind标签来避免这种问题

```xml
<select id="selectByUserName" resultType="user">
	<bind name="likeStr" value ="'%'+username+'%'"/>
    select id ,username,password
    from user 
    where username like #{likeStr}
</select>
```

在这里，我们手动拼接的模糊匹配的参数，并将其绑定到上下文中名为likeStr的变量中，然后就可以在下面直接使用了。