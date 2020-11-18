数据库类型BLOB对应的javaType是ByteArrayInputStream，即二级制数据流。

数据库类型date,time,datetime对应的jdbcType是DATE,TIME,TIMESTAMP

#### 使用JDBC方法获取自增主键

insert方法有两个属性用于获取自增主键

1. **useGeneratedKeys**
2. **keyProperty**

useGeneratedKeys设为true后，mybatis会使用JDBC的getGeneratedKeys方法取出数据库生成的自增主键，这个值会被赋给keyProperty对应的属性。

当需要设置多个属性时，同时需要设置keyColumn属性，按顺序指定数据库的列，列的值和Property一一对应。

示例代码：

```java
@Test
    public void testGenerateKey(){
        SqlSession session = session();
        UserMapper userMapper = session.getMapper(UserMapper.class);

        User user = new User();

        user.setUserName("test1");

        user.setUserPassword("123456");

        user.setUserEmail("test1@163.com");

        user.setUserInfo("test info");

        user.setHeadImg(new byte[]{1,2,3});

        user.setCreateTime(new Date());

        int res = userMapper.insert(user);

        Assert.assertEquals(1,res);

        Assert.assertNotNull(user.getId());

        session.commit();

        System.out.println(user.getId());

        session.close();
    }
```

输出：

> DEBUG [main] - ==>  Preparing: insert into user(user_name,user_password,user_email,user_info,head_img,create_time) values ( ?, ?, ?, ?, ?, ? ) 
> DEBUG [main] - ==> Parameters: test1(String), 123456(String), test1@163.com(String), test info(String), java.io.ByteArrayInputStream@65e98b1c(ByteArrayInputStream), 2018-11-28 22:35:11.786(Timestamp)
> DEBUG [main] - <==    Updates: 1
> 7

另外，这种方法支持**批量返回新增的主键**，但是目前只有**MySQL**数据库完美支持。

#### 使用selectKey返回主键值

使用JDBC回写主键的方法只适合于支持自增主键的数据库，而像Oracle数据库就不支持自增主键，而是使用序列得到一个主键值。

对于这种情况，可以使用<selectKey>标签来获取主键值。

```xml
<insert id="insert">
        insert into user(user_name,user_password,user_email,user_info,head_img,create_time)
        values (
          #{userName},
          #{userPassword},
          #{userEmail},
          #{userInfo},
          #{headImg,jdbcType=BLOB},
          #{createTime,jdbcType = TIMESTAMP}
        )
        <selectKey order="AFTER" keyProperty="id" resultType="Long" keyColumn="id">
          select last_insert_id()
        </selectKey>
    </insert>
```

这里的order属性和具体的数据库有关，如果主键是在插入操作完成后才可以获取，设为AFTER,如果是在查询操作进行之前就能获取到（Oracle的序列），设为BEFORE。

另外，在Oracle的插入语句中，必须在插入列中申明主键，因为主键此时是有值的，并且非空。

selectKey标签中的语句就是获取主键的SQL语句

1. MySQL：SELECT LAST_INSERT_ID()

2. DB2：VALUES IDENTITY_VAL_LOCAL()

3. SQL SERVER：SELECT SCOPE_IDENTITY()

4. ORACLE:SELECT 序列名.nextval from dual

5. CLOUDSCAPE:VALUES IDENTITY_VAL_LOCAL()

6. DERBY:VALUES IDENTITY_VAL_LOCAL()

7. HSQLDB:CALL IDENTITY

8. SYBASE:SELECT @@IDENTITY

   ​	

