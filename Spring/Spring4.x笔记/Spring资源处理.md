## 1.资源访问抽象接口
Spring提供了**Resource**接口，该接口有对于不同资源类型的实现类，其主要方法有：

- boolean exists()：资源是否存在
- boolean isOpen()：资源是否打开
- URL getURL()throws IOException：如果资源可以用URL表示，返回对应的URL对象。
- File getFile()throws IOException：如果资源对应一个文件，返回对应的File对象。
- InputStream getInputStream()throws IOException：返回资源对应的输入流。