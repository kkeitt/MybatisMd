#### OGNL表达式

mybatis常用的OGNL表达式如下：

1. e1 or e2
2. e1 and e2
3. e1 == e2 , e1 eq e2 ：两种都表示等于
4. e1 !=e2 , e1 neq e2：两种都表示不等于
5. e1 lt e2：e1小于e2
6. e1 gt e2：e1大于e2
7. e1 lte e2：e1小于等于e2
8. e1 gte e2：e1大于等于e2
9. e1 + e2,e1 - e2,e1 * e2,e1 / e2,e1 % e2：加，减，乘，除，取余
10. !e , not e ：非，取反
11. e.method(args)：调用对象方法
12. e.property：对象属性值
13. e1[e2] ：按索引取值，数组，List，Map
14. @class@method(args) ：调用类的静态方法
15. @class@field ：调用类的静态属性）

通过这些表达式，可以实现一些特殊的功能，比如说要实现打印SQL执行时的某个参数

```java
package util;
/**新建一个静态方法，打印传入的参数
*/
public class PrintUtil {
    public static void printParam(String param){
        System.out.println("参数值为："+param);
    }
}

```

```xml
<!-- bind标签的value值是一个OGNL表达式，调用了PrintUtil类的静态方法printParam -->
<select id="selectByUser" result="user">
	<bind name="userNameParam" value="@util.PrintUtil@printParam(_parameter)"
     select * from user where user_name like concat('%',_parameter,'%')          
</select>
```

输出：

参数为：admin

