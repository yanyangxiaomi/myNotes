## 枚举

枚举类型是Java 5中新增的一个特性，它是一种特殊的数据类型，既是一种类(class)类型,但又有一些特殊的约束。
1. 它是解决静态常量再某些使用场景下的不足，如下：

```
public static final int SEASON_SPRING = 1;
public static final int SEASON_SUMMER = 2;
public static final int SEASON_FALL = 3;
public static final int SEASON_WINTER = 4;
```
​		定义这四个常量表示季节，当某一方法需要传入季节参数时，形参就是int类型，调用者再调用时可以传入任意int型数值，需要开发者做额外的措施保证参数的安全性，若是使用枚举时，传入的参数只能是枚举类中包含的对象，开发者不用进行参数范围校验。此外当常量的数量发生变化时，需要修改方法中校验参数安全的代码，而使用枚举时不需要更改。
2. 枚举类在定义的时使用enum关键字，默认是继承了Enum类。
```
public enum SeasonEnum {
    SPRING,SUMMER,FALL,WINTER;
}
```
它的简单定义如上，枚举类的所有实例都必须放在第一行展示，不需使用new 关键字，不需显式调用构造器。自动添加public static final修饰。枚举类的构造器只能是私有的。
3. 枚举类也可以定义属性和方法(静态和非静态都可以)，还能实现接口。如下(此处为演示，故实现的接口方法是一致的)：
```
public enum SeasonEnum {
        SPRING("春天") {
            @Override
            public String say() {
                return "It's " + this.name + " now!";
            }
        },SUMMER("夏天") {
            @Override
            public String say() {
                return "It's " + this.name + " now!";
            }
        },FALL("秋天") {
            @Override
            public String say() {
                return "It's " + this.name + " now!";
            }
        },WINTER("冬天") {
            @Override
            public String say() {
                return "It's " + this.name + " now!";
            }
        };

        private String  name;

        private SeasonEnum(String name) {
            this.name = name;
        }

        public String getName(){
            return this.name;
        }

        public abstract String say();
    }
```
4. Java 5中的对switch进行了扩展，使其也可以支持enum类型。示例如下：
```
public static void printName(SeasonEnum seasonEnum){
        switch (seasonEnum){
            case SPRING: //无需使用SeasonEnum进行引用
                System.out.println("春天");
                break;
            case SUMMER:
                System.out.println("夏天");
                break;
            case FALL:
                System.out.println("秋天");
                break; 
            case WINTER:
                System.out.println("冬天");
                break;
        }
    }
```
5. 枚举还可以用于实现单例模式。

   ​		单例的实现方式有：懒汉式(线程安全，效率低)、饿汉式(线程安全、某些情况下效率低)、双重检查锁、静态内部类。以上四种序列化和反射都会破坏单例。枚举序列化是由jvm保证的，每一个枚举类型和定义的枚举变量在JVM中都是唯一的，在枚举类型的序列化和反序列化上，Java做了特殊的规定：在序列化时Java仅仅是将枚举对象的name属性输出到结果中，反序列化的时候则是通过java.lang.Enum的valueOf方法来根据名字查找枚举对象。同时反射类的源码对枚举在进行反射操作是直接抛异常的。

6. 使用枚举时占用的内存常常是静态变量的两倍还多，在内存不充足时应该尽量避免使用枚举。