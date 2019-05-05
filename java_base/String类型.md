## String数据类型

- String是Java中一个特殊的包装数据类，使用了final修饰，不能被继承。String是常量，其对象一旦构造就不能再被改变。换句话说，String对象是不可变的，每一个看起来会修改String值的方法，实际上都是创造了一个全新的String对象，以包含修改后的字符串内容。而最初的String对象则丝毫未动。String对象具有只读特性，指向它的任何引用都不可能改变它的值，因此，也不会对其他的引用有什么影响。但是字符串引用可以重新赋值。具有两种创建方式：
（1）String i = "a";
（2）String i = new String("a);
方式一创建时会先检查字符串常量池中是否会有字符串a，如果有则 i 直接指向常量池中的字符串对象，如果没有则在常量池中创建并指向该字符串对象。此方式最多创建一个对象。
方式二会先在堆中申请一块内存存储字符串a，i 再指向堆中的对象，然后检查字符串常量池中是否有字符串a，如果没有则添加至字符串常量池。此方式最多会创建两个对象

- JDK6和JDK7中substring方法的不同
JDK6中substring()调用了String(int offset,int count,char value[])构造方法创建的截取字符，而该构造方法中新生成的String 的value 简单的使用了原 String 的value的引用（value是一个final 数组），只是修改了offset 和count，如果原字符较大，由于value被引用无法回收易造成内存泄漏；JDK7中对构造函数进行了改进，使用Arrays.copyOfRange(value, offset, offset + count)方法对元数组进行了拷贝。JDK6效率较高、但有安全问题；JDK7为了安全而损失了部分性能。
- replace、replaceAll、replaceFirst之间的区别
replace方法是将目标字符替换为指定字符，若目标字符出现多次则全部替换
replaceAll方法是将通过正则表达式匹配的目标字符替换为指定字符，如匹配到多个目标字符则全部替换
replaceFirst方法是将通过正则表达式匹配的第一个目标字符替换为指定字符，若匹配到多个目标字符，只替换第一个目标字符
- String 对“+”的重载、字符串拼接的几种方式的区别
字符串拼接的方式：
（1）使用“+”进行拼接
String对“+”进行了重载，当“+”两边都是字符串常量时，编译时进行了合并已知量，如若是String s = “a”+"b",编译时合并已知量就是String s = "ab"。当有一边是变量时，则会创建StringBuilder对象，调用append()方法进行拼接，然后用toString()方法输出。
（2）使用concat()方法
使用concat方法时，String对象不能为空。将指定字符串连接到此字符串的结尾。如果参数字符串的长度为 0，则返回此 String 对象。否则，创建一个新的 String 对象，用来表示由此 String，对象表示的字符序列和参数字符串表示的字符序列连接而成的字符序列。
（3）使用StringBuilder或者StringBuffer的append()方法
StringBuilder或StringBuffer类中使用可变的字符数组实现对字符串的拼接。
- String.valueOf和Integer.toString的区别
二者都可以将数字转为字符串，区别在于：
（1）toString方法在Object类就有，所以对任何严格意义上的java对象都可以调用此方法。但在使用时要注意，必须保证object不是null值，否则将抛出NullPointerException异常。
（2）valueOf(Object obj)对null值进行了处理，不会报任何异常。但当object为null 时，String.valueOf（object）的值是字符串”null”，而不是null。
- switch对String的支持
Java1.7之前switch只能局限于int 、short 、byte 、char四类做判断条件。在JVM内部实际大部分字节码指令只有int类型的版本。在使用switch的时候，如果是非int型，会先转为int型，再进行条件判断。***Java1.7之后switch对字符串的支持其实是一种语法糖，编译后比较是字符串的哈希值，如果哈希值相同再通过equals进行比较。***
- 字符串常量池、常量池(运行时常量池、Class常量池)、intern
（1）Class常量池用于存放编译器生成的各种字面量(Literal)和符号引用(Symbolic References)。字面量相当于Java语言层面常量的概念，如文本字符串，声明为final的常量值等；符号引用则属于编译原理方面的概念，包括了如下三种类型的常量：类和接口的全限定名、字段名称和描述符、方法名称和描述符。
（2）运行时常量池是方法区的一部分，在类加载后将Class常量池中的内容放入方法区的运行时常量池中存放。
运行时常量池相对于CLass文件常量池的另外一个重要特征是具备动态性，Java语言并不要求常量一定只有编译期才能产生，也就是并非预置入CLass文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用比较多的就是String类的intern()方法。
（3）Java为了提高性能，静态字符串（字面量/常量/常量连接的结果）在字符串常量池中创建，并尽量使用同一个对象，重用静态字符串。对于重复出现的字符串直接量，JVM会首先在字符串常量池中查找，如果常量池中存在即返回该对象，不存在再创建。
（4）当调用 intern()方法时，如果字符串常量池中已经包含一个等于此 String 对象的字符串（该对象由 equals(Object) 方法确定），则返回字符串常量池中的字符串对象的引用；否则，将此 String 对象添加到常量池中，并且返回此 String 对象的引用。
- 使用“==”对字符串进行比对，“==”比较的是变量的值是否相等，equals()方法比较的是对象内容是否相等。如果在编译期字符串可以确定，编译时会合并已知量，那么使用“==”比对时返回true。例如：
```java
String a1 = "test1";
String a2 = "test" + 1;
String a3 = new String("test1");
System.out.println(a1 == a2); //true
System.out.println(a1 == a3); //false

final int i = 1;
int j = 1;
String a4 = "test" + i;
String a5 = "test" + j;

System.out.println(a1 == a4); //true
System.out.println(a1 == a5); //false

System.out.println(a5.intern() == a1); //true
```