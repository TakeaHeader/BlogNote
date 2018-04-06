### Map相关接口API源码理解

> Map 是一个键值对的接口,它代表了字典的抽象接口。

> HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体,
初始化HashMap会初始化一个Entry<K,V>类型的数组,而Entry又是一个链表结构，指向下一个Entry,构成了一个单向链表

根据源码中的描述,HashMap实现了Map的所有方法,并且允许null作为value,也允许null作为key
一个HashMap的实例,有两个参数影响它的性能,一个是initial capacity(初始容量),load factor(加载因子)
初始容量是哈希表在创建时的容量，加载因子 是哈希表在其容量自动增加之前可以达到多满的一种尺度

[Map](https://github.com/TakeaHeader/BlogNote/blob/master/map.png)
