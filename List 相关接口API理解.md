### List 相关接口API理解

> ArrayList 可变长数组的实现, 默认初始化一个空数组, 每次添加当容量不足（数组长度不够）,会重新产生一个加原长度二分之一的数组,

int newlength = oldlength + (oldlength >> 1); 

 移除一个元素在数组中相对麻烦,所以ArrayList为了保持速度,采用以空间换效率的方式来快速移位,移除的位置赋值为null,让GC自动回收

 ArrayList 可以添加重复的数据,同时是线程不安全的

 > Vector 和ArrayList类似

 相同点:
 1. 都是可变长数组
 2. 都可以添加重复数据
 3. 都是List的实现类

 不同点:

1. Vector 是线程安全的,ArrayList则不是;这也就导致了Vector的效率会相对差一些

2. Vector默认会初始化一个数组长度为10 的空数组,ArrayList则就是空数组

3. Vector当空间不足时会自动增长一倍,也可以指定,ArrayList则是二分之一,相对更省空间

> LinkedList 也是List的实现类,是双向链表的实现,所有删除,插入元素特别快,

LinkedList 中实现了尾插法,和头插法,以及指定位置插入,仅仅是修改前一个位置的下一个节点指向自己,下一个位置节点的头结点指向自己

由于是通过指向性的方式的数据结构,所以内存空间不是必须连续的,

ArrayList和Vector是基于数组的， 所以内存是连续的。

频繁的插入修改移除使用LinkedList速度较快