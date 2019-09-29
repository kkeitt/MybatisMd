## 1.类装载器
类装载器的用途是寻找类的字节码文件并构造出类在JVM内部表示对象。

在Java中，类装载器加载一个类到JVM中，需要以下步骤：

1. 装载：查找和导入.class文件
2. 链接：执行校验，准备和解析步骤（解析步骤可选）
    1. 校验：检查.class文件数据的正确性
    2. 准备：为类的静态变量分配空间
    3. 解析：将符号引用转为直接引用

3. 初始化：对类的静态变量，静态代码块执行初始化操作

类的装载操作由ClassLoader及其子类负责，JVM会在运行时产生三个ClassLoader:
1. **根装载器**：由C++实现，不是ClassLoader的子类，在Java中无法访问，负责装载JRE的核心类库，如`rt.jar`,`charsets.jar`等。
2. **ExtClassLoader**:扩展类装载器，负责装载JRE扩展目录ext中的jar包。
3. **AppClassLoader**:负责装载Classpath路径下的jar包。

这三个类装载器之间存在父子层级关系，ExtClassLoader是根装载器的子类，AppClassLoader是ExtClassLoader的子类。

可以通过ClassLoader类的`getParent`方法获取类加载器的父类，下面的代码证明了上述层级关系
```java
public static ClassLoaderTest{
    public static void main(String [] args){
        ClassLoader currLoader = Thread.currentThread().getContextClassLoader();

        System.out.println("current classloader is "+currLoader);
        System.out.println("parent classloader is "+currLoader.getParent());
        System.out.println("grandparent classloader is "+currLoader.getParent().getParent());

    }
}
```
> current classloader is sun.misc.Launcher\$AppClassLoader@18b4aac2
parent classloader is sun.misc.Launcher$ExtClassLoader@2c7b84de
grandparent classloader is null

因为根装载器是C++实现的，所以得到的是null。

JVM在装载类时，采用`全盘负责委托机制`，`全盘负责`是指当一个ClassLoader在加载类时，除非显式使用另一个ClassLoader，该类所依赖及引用的类也由这个ClassLoader加载；`委托机制`是指ClassLoader在加载前会先委托父加载器查找，如果找不到，才会在自己的类路径下查找目标。

## 2.自定义装载器
除了JVM默认的3个ClassLoader之外，还可以自己编写第三方类加载器，以实现特殊的需求。

当类文件被装载并解析后，在JVM内会有一个对于的**java.lang.Class**类描述对象，该类的所有实例都拥有指向这个类描述对象的引用，而类描述对象又拥有指向类加载器的引用。

java.lang.Class对象提供了类结构信息的描述。**数组，枚举，注解，void及基础数据类型**都有对应的Class对象。Class对象由JVM在装载类时调用ClassLoader的defineClass方法创建的。
