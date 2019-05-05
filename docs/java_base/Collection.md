## Collection

Collection是一个高度抽象出来的集合，包含了集合的基本操作和属性，包含了List和Set两大分支。

#### 1、List是一个有序的队列。实现类有LinkedList、ArrayList、Vector、Stack。

###### 1.1、ArrayList 

ArrayList 是一个数组队列，相当于动态数组。与Java中的数组相比，它的容量能动态增长(默认容量大小是10、扩容是“(原始容量x3)/2 + 1”)。

- ArrayList继承于AbstractList，实现了List。它是一个数组队列，提供了相关的添加、遍历、删除、修改等操作。
- 实现了RandomAccess接口提供了快速随机访问功能，即可以通过元素的序号快速获取元素对象；这就是快速随机访问。
- 实现了Cloneable接口，重写了clone()函数，可以被克隆。
- 实现了Serializable接口，支持序列化。
- ArrayList可以通过迭代器、for循环、增强for循环来遍历。***遍历ArrayList时，使用随机访问(即，通过索引序号访问)效率最高，而使用迭代器的效率最低。***

***ArrayList中操作是线程不安全的，不适合用于多线程环境下***。

###### 1.2、LinkedList

LinkedList是一个是继承于AbstractSequentialList的双向链表，还可以被当作堆栈、队列、双端队列使用。LinkedList的数据结构包含当前节点的值、上一节点、下一节点，这个实现方式可以看出LinkedList不存在容量不足问题。

- LinkedList实现了List接口，能尽心队列的相关操作。
- LinkedList实现了Deque接口，Deque接口定义了在双端队列两端访问元素的方法，能够当作双端队列、FIFO队列和LIFO队列使用。
- LinkedList实现了Cloneable接口，既重写了clone()方法，能够进行克隆，克隆时将全部元素克隆到一个新的LinkedList对象中。。
- LinkedLis实现了Serializable接口，能够进行序列化。
- 除了迭代器、for循环、增强for循环方式遍历LinkedList外，还可以通过pollFirst()、pollLast()、removeFirst()、removeLast()方式遍历，后两种速度最快但会删除数据，for循环遍历方式最慢。

***LinkedList是非同步的，适用于单线程。***

###### 1.3、Vector

Vector是矢量队列，它是JDK1.0版本添加的类。继承于AbstractList，实现了List, RandomAccess, Cloneable这些接口。Vector通过数组保存数据，初始容量为10，容量不足时，若容量增加系数 >0，则将容量的值增加“容量增加系数”；否则，将容量大小增加一倍。

- Vector 继承了AbstractList，实现了List；所以，它是一个队列，支持相关的添加、删除、修改、遍历等功能。
- Vector 实现了RandmoAccess接口，即提供了随机访问功能。RandmoAccess是java中用来被List实现，为List提供快速访问功能的。在Vector中，我们即可以通过元素的序号快速获取元素对象；这就是快速随机访问。
- Vector 实现了Cloneable接口，即实现clone()函数。它能被克隆。
- Vector可以通过迭代器、for循环、增强for循环、Enumeration方式遍历。

***Vector和ArrayList不同，Vector中的操作是线程安全的。***

###### 1.4、Stack

Stack是栈。它的特性是：先进后出(FILO, First In Last Out)。Stack是继承于Vector(矢量队列)，由于Vector是通过数组实现的，这就意味着，**Stack也是通过数组实现的，而非链表**。

- Stack是通过数组去实现的，所以
  ***执行push时(即，将元素推入栈中)，是通过将元素追加的数组的末尾中。
  执行peek时(即，取出栈顶元素，不执行删除)，是返回数组末尾的元素。
  执行pop时(即，取出栈顶元素，并将该元素从栈中删除)，是取出数组末尾的元素，然后将该元素从数组中删除。***
- Stack继承于Vector，意味着Vector拥有的属性和功能，Stack都拥有。

#### 2、Set

Set是一个不允许有重复元素的集合。实现类有HashSet和TreeSet，HashSet的实现依赖于HashMap；TreeSet的实现依赖于TreeMap。

###### 2.1、HashSet

HashSet是一个没有重复元素的集合，继承于AbstractSet类，并实现了Set接口。底层是由HashMap实现，不保证元素的顺序，但允许有NULL值。

- 遍历方式有迭代器和增强for循环。HashSet通过iterator()返回的迭代器是fail-fast的。

***HashSet是不同步的，操作是线程不安全的。***

###### 2.2、TreeSet

TreeSet是一个没有重复元素的有序集合，继承于AbstractSet抽象类，实现了NavigableSet<E>、Cloneable、Serializable接口。

- TreeSet 继承于AbstractSet，所以它是一个Set集合，具有Set的属性和方法。
- TreeSet 实现了NavigableSet接口，意味着它支持一系列的导航方法。比如查找与指定目标最匹配项。
- TreeSet 实现了Cloneable接口，意味着它能被克隆。
- TreeSet 实现了java.io.Serializable接口，意味着它支持序列化。
- TreeSet 底层基于TreeMap实现，所以TreeSet支持两种排序方式：自然排序 或者 根据创建提供的 Comparator 进行排序。这取决于使用的构造方法。
- TreeSet为基本操作（add、remove 和 contains）提供受保证的 log(n) 时间开销。
- TreeSet支持迭代器和增强for循环遍历集合。

***TreeSet是非线程安全的。***