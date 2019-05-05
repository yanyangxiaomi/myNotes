## 序列化

#### 1、序列化与反序列化的概念

***序列化：把对象转换为字节序列的过程称为对象的序列化。
反序列化：把字节序列恢复为对象的过程称为对象的反序列化。***
对象序列化的作用：

- 把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中；
- 在网络上传送对象的字节序列。

#### 2、类库中序列化相关的API

- 只有实现了Serializable和Externalizable接口的类的对象才可以进行序列化操作。Externalizable接口继承自 Serializable接口，实现Externalizable接口的类完全由自身来控制序列化的行为，而仅实现Serializable接口的类可以 采用默认的序列化方式 。
- 使用java.io.ObjectOutputStream的writeObject(Object obj)方法可对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中；使用java.io.ObjectInputStream的readObject()方法从一个源输入流中读取字节序列，再把它们反序列化为一个对象，并将其返回。

#### 3、serialVersionUID的作用

​		serialVersionUID: 字面意思上是序列化的版本号，凡是实现Serializable接口的类都有一个表示序列化版本标识符的静态变量，若实现Serializable接口的类如果类中没有添加serialVersionUID编辑器都会给予警告。
如果没有显式声明serialVersionUID静态变量，在运行时环境根据类的内部细节自动生成，如果类发生修改对应的serialVersionUID也变化了，而序列化和反序列化就是通过对比其serialVersionUID来进行的，一旦serialVersionUID不匹配，反序列化就无法成功。当显式定义后类的修改不影响serialVersionUID的值，可以确保反序列化的成功。

​		***为了提高serialVersionUID的独立性和确定性，强烈建议在一个可序列化类中显式的定义serialVersionUID，为它赋予明确的值。***

