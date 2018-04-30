### Map相关接口API源码理解

> Map 是一个键值对的接口,它代表了字典的抽象接口。
> HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体,
初始化HashMap会初始化一个Entry<K,V>类型的数组,而Entry又是一个链表结构，指向下一个Entry,构成了一个单向链表

根据源码中的描述,HashMap实现了Map的所有方法,并且允许null作为value,也允许null作为key
一个HashMap的实例,有两个参数影响它的性能,一个是initial capacity(初始容量),load factor(加载因子)
初始容量是哈希表在创建时的容量，加载因子 是哈希表在其容量自动增加之前可以达到多满的一种尺度

![Map](https://github.com/TakeaHeader/BlogNote/blob/master/map.png)

**clear()**

HashMap清空Map的方法是直接将数组全部填充为null,size重置为0;

**containsKey**

HashMap中通过参数key来获取entry对象,获取到entry则为true;否则是false,然后循环整个链表,通过hash值来检索key,检索到则返回

**containsValue**

和containKey类似，循环链表判断通过equals方法判断,相同返回true,否则false


**entrySet**

HashMap内部有一个EntrySet类继承自AbstractSet<Map.Entry<K,V>>,entrySet会返回一个EntrySet的对象,通过EntrySet返回一个entry的迭代器EntryIterator
这样用来循环所有的key,Value键值对,而迭代器则是根据链表的特性,不停的迭代下一个Entry,由于HashMap不是线程安全的,
在获取下一个entry对象时,首先会判断已经修改的次数(HashMap内部会记录修改次数),当修改次数和期待的次数不一致时,则可能是并发造成的，
EntryIterator则直接抛出ConcurrentModificationException异常,这就是HashMap中的fast-fail机制(快速失败机制)

**get**

通过key获取value分为两种情况，一种是null 为key,一种是key值不是null，
原理是一样的，循环链表,为null则返回value,不是null则根据hash值类获取,
所以HashMap中有且只有一个key值可以为null

**put**
添加key ,value键值对时,首先会先判断容量是否足够，是否需要扩容,扩容之后,先是判断key是否为null
为null,则寻找null为key的entry进行覆盖,不是null,根据Hash循环链表,去覆盖，如果没有覆盖，则添加新的entry到数组中


**isEmpty**

判断内部计数器是否等于0,也就是size==0

**size**

HashMap内部有一个计数器,直接返回计数器


