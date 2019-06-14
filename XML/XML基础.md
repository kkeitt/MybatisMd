XML全称为：可扩展标记语言(Extensible Markup Language)

XML是一种描述结构化信息的技术，它可以表示层次结构，这比属性文件（.properties）更灵活。

XML和HTML非常类似，但又有许多不同之处。它们都属于**标准通用标记语言**SGML(Standard Generalized Markup Language)的衍生语言。

XML和HTML的区别：

- XML是大小写敏感的，例如H1和h1在HTML中是同义的，但在XML是不同的元素。
- HTML中有时可以省略结束标签，而在XML中必须有结束标签或开始标签以 / 结尾：\<img src = 'a.jpg'/>。
- 在XML中，标签的属性必须用引号括起来，而HTML中引号不是必须的。
- 在HTML中，属性可以没有值，例如\<input type='radio' checked/>,而XML中属性必须有值。

#### XML的文档结构

XML文档应该以一个文档头开始，例如

```xml
<?xml version="1.0"?>
```

或者

```xml
<?xml version="1.0" encoding="UTF-8" ?>
```

文档头是可选的，一般建议加上。

文档头之后通常是文档类型定义DTD（Document Type Definition),例如mybatis的DTD

```xml-dtd
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
```

最后，XML文档的正文包括根元素，根元素包括其他子元素，例如

```xml
<root>
	<first>
    	<name></name>
        <size></size>
    </first>
    <second>
    	<name></name>
        <size></size>
    </second>
</root>
```

元素可以包含子元素或文本或两者皆有

```xml
<p>
	title
    <size>16px</size>
</p>
```

在设计XML文档结构时，最好让元素只包含文本或子元素，这样可以简化解析过程。

元素中可以包含属性

```xml
<size unit="px">36</size>
```

属性一般情况下，最好只用来描述值，而不是用来指定值,例如下面的例子

```xml
<font>
	<size unit="px">36</size>
</font>
```

我们当然可以直接将属性unit的值设为36px，但是这样px并不是固定的，这时就需要对字符串进行解析，将值作为文本，单位作为属性，可以更方便的解析。

#### 文本，元素之外的其他元素

除了文本，元素，还有以下元素也会出现在XML文档中

- 字符引用（character reference）：形式为：&#十进制值;，或：&#x十六进制值;。例如小于号\<可以用  \&#60;  或  \&#x3c;来表示。
- 实体引用（entity reference）：形式为：\&name，例如小于号\<可以用 \&lt; 来表示。
- CDATA部分（CDATA Section）：即不应由XML解析器进行解析的文本数据（Unparsed Character Data），用\<!CDATA[和]]\>来限定其界限。可以用来包含会被XML解析器解析的字符，如\<,\>,\&等。
  例：<!CDATA[ < > & ]]\>
  另外CDATA部分不能包含字符串]]>。
- 处理指令（processing instruction）：指专门在处理XML文档的应用程序中使用的指令，由\<?和?>来限定
  例如：\<?xml-stylesheet href="mystyle.css" type="text/css" ?>
  XML的文档头
  \<? xml version="1.0"?>
- 注释：（comment）：用\<!-- 和-->限定。