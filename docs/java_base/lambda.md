## Lambda表达式

Lambda表达式也称为闭包，是Java 8的新特新之一。它允许把函数作为一个方法的参数(函数作为参数传递进方法中)。使用Lamba表达式可以让代码变的更加简洁紧凑。

#### 1、Lambda表达式的语法

```java
(arg1,arg2,...) -> { body }
(type1 arg1,typ2 arg2,...) -> { body }
```
例如：
```java
(int a,int b) ->{return a + b}
() -> System.out.println("Hello!")
(String str) -> {System.out.println(str)}
```
#### 2、Lambda表达式的特征 

**可选类型申明：**不需要申明参数类型，编译器可以同一识别参数值。

```java
Arrays.asList("a","b","c").forEach( e -> System.out.println(e));
```

这里的e的类型就是由编译器推理得到的，不需要申明类型，也可以声明类型，比如：

```java
Arrays.asList("a","b","c").forEach((String e) -> System.out.println(e));
```
但是并不是所有的类型都可以推断出来，此时就需要我们像上面那样显示的声明参数类型。

**可选的参数圆括号：**当只有一个参数且无需声明参数类型时，可以省略参数的圆括号。当有多个参数或者参数需要声明类型时需要圆括号。事例如上。

**可选的大括号：**如果代码主体只包含一个语句，可以省略大括号，反之则需要使用大括号包起来。

```
Arrays.asList("a","b","c").forEach(e -> {
	System.out.println(e);
	System.out.println(e + "---");
});
```

可选的返回关键字：如果主体只有一个表达式返回值，则编译器会自动返回值，大括号需要指明表达式返回了一个数值。如果表达式中的语句块只有一行，则可以不用使用return关键字。

```java
Arrays.asList("a","b","c").sort((e1,e2) -> e1.compareTo(e2));
//以上的写法等同于以下的写法
Arrays.asList("a","b","c").sort((e1,e2) -> {
    int result = e1.compareTo(e2);
    return result;
});
```

#### 3、Lambda表达式中变量的作用域

Lambda表达式只能引用外部被final修饰的局部变量，即我们不能修改定义在外部的局部变量。如果引用的外部变量没有显式的使用final修饰，Lambda表达会默认该变量为final。

```java
public static void main(String[] args){
	//str2变量会被Lambda表达式默认为final
	final String str1 = "hello ";
	String str2 = " !";
	
    Arrays.asList("a","b","c").forEach(e -> System.out.println(str1 + e + str2));
}
```

#### 4、函数式接口

在Java中，Lambda表达式是对象，它必须依赖于一类特别的对象类型--函数式接口（Function Interface）。函数式接口的定义：如果一个接口中，有且只有一个抽象的方法（Object类中的方法不包括在内），这个接口就可以被看成函数式接口。如：Runnable接口。

```Java
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
```

在Runnable接口上有一个@FunctionalInterface注解，该注解用于表明该接口是一个函数式接口。如果编写的接口没有添加@FunctionalInterface注解，且符合函数式接口的要求时，编译器也会将起当作函数式接口看待。

#### 5、函数式接口实例的创建方式

在介绍函数式接口实例创建方式之前先写一段传统的写法，通过匿名内部类的方式。如下：

```java
public static void main(String[] args){
	List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8);
    list.forEach(new Consumer<Integer>(){
       @Override
        public void accept(Integer integer){
            System.out.println(integer);
        }
    });
}
```

这段代码是初始化一个Integer集合，然后遍历打印每个元素。其中forEach()方法是Java8新增到Iterable接口中的默认方法（关于接口中的默认方法此处不过多说明）。该方法需要一个Consumer类型的参数，查看源码可知，这是一个函数式接口，如下：

```Java
@FunctionalInterface
public interface Consumer<T> {

    /**
     * Performs this operation on the given argument.
     * 对给定的参数T执行定义的操作
     * @param t the input argument
     */
    void accept(T t);

    /**
     * Returns a composed {@code Consumer} that performs, in sequence, this
     * operation followed by the {@code after} operation. If performing either
     * operation throws an exception, it is relayed to the caller of the
     * composed operation.  If performing this operation throws an exception,
     * the {@code after} operation will not be performed.
     * 对给定的参数T执行定义的操作执行再继续执行after定义的操作
     * @param after the operation to perform after this operation
     * @return a composed {@code Consumer} that performs in sequence this
     * operation followed by the {@code after} operation
     * @throws NullPointerException if {@code after} is null
     */
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```

###### 5.1 Lambda表达式

由示例代码可知forEach()方法需要Consumer类型的参数,使用Lambda表达式创建接口实例方式如下：

```java
public static void main(String[] args){
	List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8);
    list.forEach(integer -> System.out.println(integer));
}
```

此时该lambda表达式integer -> System.out.println(integer)接收了一个参数，不返回值，符合accept()方法的定义，编译通过。**此时可知使用Lambda表达式创建函数式接口实例时，Lambda表达式的入参和返回必须符合这个函数式接口中唯一抽象方法的定义。**

###### 5.2 方法引用

方法引用有很多种，它们的语法如下：

- 静态方法引用：`ClassName::methodName`

  定义一个静态方法，如下：

  ```java
  public class Test {
      public static void myStaticPrintln(Object object){
          System.out.println(String.valueOf(object));
      }
  }
  ```
  则上面代码可改为：
  
  ```java
  public static void main(String[] args){
  	List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8);
        list.forEach(Test::myStaticPrintln);
  }
  ```
  
- 实例上的实例方法引用：`instanceReference::methodName`

  定义一个实例方法

  ```java
  public class Test {
      public static void myPrintln(Object object){
          System.out.println(String.valueOf(object));
      }
  }
  ```

  则上面代码可改为：

  ```java
  public static void main(String[] args){
  	List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8);
      list.forEach(new Test()::myPrintln);
  }
  ```

- 超类上的实例方法引用：`super::methodName`

  （暂未找到合适的Demo,后续补上。）

- 类型上的实例方法引用：`ClassName::methodName`

  由于Integer是不可变类，所以先定义一个类如下：

  ```java
  public class Test {
  
      private Object object;
  
      public Test( Integer integer) {
          this.object = object;
      }
      
      public void myNewPrintln(){
          System.out.println(String.valueOf(this.object));
      }
  ｝
  ```

  实例方法引用则为如下写法：

  ```java
  public static void main(String[] args){
  	List<Test> tests = Arrays.asList(new Test(1),new Test(2),new Test(3));
      list.forEach(Test::myNewPrintln);
  }
  ```

- 构造方法引用：`Class::new`

  定义一个构造方法：

  ```java
  public Test(final Object object) {
      System.out.println(String.valueOf(object));
  }
  ```

  则上面代码可改为：

  ```java
  public static void main(String[] args){
  	List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8);
        list.forEach(Test::new);
  }
  ```

- 数组构造方法引用：`TypeName[]::new`

  数组的构造方法引用的语法则比较特殊，为了便于理解，你可以假想存在一个接收`int`参数的数组构造方法。参考下面的代码：

  ```java
  IntFunction <int[]> arrayMaker = int[]::new;
  int[] array = arrayMaker.apply(10)
  ```

**使用方法引用实现函数式接口实例时，被引用的方法的入参和返回也必须符合这个函数式接口中唯一抽象方法的定义。**