## Map

Map是一个映射接口，即key-value键值对。实现类有HashMap、TreeMap、WeakHashMap、Hashtable、LinkedHashMap。

#### 1、HashMap

HashMap是一个存储键值对映射(key-value)的散列表，有两个影响性能的参数：初始容量(默认值16)、加载因子(默认值0.75)。容量是哈希表中桶的数量，加载因子是哈希表在其容量自动增加之前可以达到多少容量的占比。当哈希表中的条目数超出了加载因子与当前容量的乘积时，则要对该哈希表进行扩容操作(容量扩大一倍)。

- HashMap继承了AbstractMap类，实现了Map接口。Map是"key-value键值对"接口，AbstractMap实现了"键值对"的通用函数接口。 
- HashMap在JDK1.8之前是通过“拉链法”实现的哈希表，通过数组加链表的形式实现；在JDK1.8中，初始时也是通过“拉链法”，当链表的深度到达8时，将链表转换为红河树，以提高查找效率。
- HashMap实现了Cloneable接口，即实现了clone()方法可以对HashMap对象进行克隆。
- HashMap实现了Serializable接口，可以对HashMap进行序列化操作。
- 遍历方式：entrySet()获取HashMap的“键值对”的集合；keySet()获取HashMap的“键”的集合；value()获取HashMap的“值”的集合。

***HashMap是不同步的，线程不安全，不适合在多线程中使用。HashMap中的映射不是有序的。它的key、value都可以为null。***

#### 2、TreeMap

TreeMap是一个有序存储键值对映射(key-value)的散列表，基于红黑树实现，根据键的自然顺序进行排序或者自定义的Comparator进行排序。

- TreeMap继承了AbstractMap类，并实现了NavigableMap接口。能都对TreeMap进行导航操作。
- 实现了Cloneable接口，重写了cone()方法，能都对TreeMap对象进行克隆。
- 实现了Serializable接口，能都进行序列化操作。
- 遍历方式：entrySet()获取TreeMap的“键值对”的集合；keySet()获取TreeMap的“键”的集合；value()获取TreeMap的“值”的集合。

***TreeMap是不同步的，线程不安全，不适合在多线程中使用。它的value可以为null，key在使用默认比较器时若为空则会报空指针异常，可自定义比较器处理null。***

#### 3、WeakHashMap

WeakHashMap也继承域AbstractMap，实现了Map接口。和HashMap一样，是存储键值对映射(key-value)的散列表，且键和值都可以为空。但WeakHashMap的键是“弱键”。

- 跟HashMap一样，是通过“拉链法”实现的，底层通过数组加链表的形式实现，另外多了一个ReferenceQueue队列，这个队列会保存被GC回收的“弱键”。当向WeakHashMap中添加键值对时与HashMap一样，根据hash值保存在数组中，若数组中存在就放到键值对链的末尾；当某个弱键不再被引用时就会被GC回收，也会被添加到ReferenceQueue队列中；当再次操作WeakHashMap时，会先同步队列与数组链表，在数组链表中键值与队列中键值一致的键值对。

***和HashMap一样，WeakHashMap是不同步的。可以使用 Collections.synchronizedMap 方法来构造同步的 WeakHashMap。***

#### 4、Hashtable

和HashMap一样，是存储键值对映射(key-value)的散列表。但Hashtable继承于Dicitionary类，与HashMap一样加载因子为0.75，但初始容量为11。

- Hashtable继承于Dictionary类，实现了Map接口。Map是"key-value键值对"接口，Dictionary是声明了操作"键值对"函数接口的抽象类。
- Hashtable也实现了Cloneable、java.io.Serializable接口，可以克隆和序列化。

***Hashtable 的函数都是同步的，所以它是线程安全的，但效率较低。它的key、value都不可以为null。此外，Hashtable中的映射不是有序的。***

#### 5、LinkedHashMap

LinkedHashMap是一个继承了HashMap的散列表，它是有序的HashMap，可以认为是HashMap+LinkedList，即它既使用HashMap操作数据结构，又使用LinkedList维护插入元素的先后顺序。

- LinkedHashMap充分的运用了多态思想，没有全部重写HashMap的数据操作函数，而是重写了部分操作函数及数据结构Entry，而重写的Entry也是继承与HashMap的Entry并新定义了两个变量before、after来表示前后顺序(变量accessOrder表示排列顺序，false为按照插入，true为按照访问，默认是按照插入排序)。此时Entry中有三个Entry变量：next、before、after。***next是用于维护HashMap指定table位置上连接的Entry的顺序的，before、After是用于维护Entry插入的先后顺序的。***

