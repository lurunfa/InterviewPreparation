# Collection 专题讲解
## Collection架构
>![架构图](./res/collection架构.png)
一共有三大接口
1. List
2. Set
3. Queue/Deque
## 问题1 对比 Vector、ArrayList、LinkedList有何区别
- Vector 是 Java 早期提供的线程安全的动态数组，如果不需要线程安全，则尽量不要使用。因为同步会造成额外的开销（基本所有操作的方法加入synchronized）内部使用一个数组来存储，扩容如果没有指定扩容大小则是原来的两倍，若依然小于扩容量则 扩容至扩容量，如果扩容后大小超出最大扩容值，则扩容后大小为最大扩容量。
- ArrayList 是应用更广泛的动态数组。本身不是线程安全，因此性能比vector好，内部也是用数组来存储数据。默认的大小为10，扩容机制：先扩容为原来的50%,int newCapacity = oldCapacity + (oldCapacity >> 1);然后比较，若还不够，则扩容值需求值，最后判断是否比最大值大，若大，则只能扩容置最大值,最后将老数组复制到新数组当中（为什么不使用System.arraycopy()?)。由于使用数组来存储数据，因此，在数组在中间插入删除的效率比较慢，因为插入或者删除后，都需要对数组进行移动。但是在随机访问数组中的元素的效率比较高。（尾部的插入和删除效率也是很高）
- LinkList 是java提供的双向链表，不需要调整容量，也不是线程安全。双向链表中，每个节点都有指向前和后的指针。整个链表中保存这头和尾部的指针。查询是是通过序号或者通过下标取获取，这个下标即元素加入链表的顺序，由于是双向链表所以通过比较下标和size的一半，分别在前或者后遍历。因此查询效率较慢，但是在链表中添加和删除元素的效率很高，不需要移动后面的元素。

## 问题2 对比TreeSet,HashSet,LinkHashSet有何区别
- TreeSet内部使用TreeMap来保存数据，键key为需要保存的值，value为一个虚假（Dummy）的值，直接new OBJECT出来。而TreeMap内部是一颗可爱的红黑书。因此支持自然顺序的访问。所谓的自然顺序表示用存储对象本身的比较方法的排序来访问。当然也支持比较顺序，在构造方法中定义比较器。缺点是添加，删除，包含等操作很慢时间复杂度为O(log(n))因为每次都是在查找二叉树中找到相应的节点。因此TreeSet来保存用于排序的数组。
- HashSet，内部用hashmap来实现，原理和上述类似，但是hashmap时桶+链表+红黑书的结构。这个的具体讲解在后面会说到，因此在不考虑冲突的情况下，能做到常数时间的添加，删除，包含等操作。但不保证有序。
- LinkedHashSet 继承HashSet，但是在HashSet有专门用于LinkedHashSet的构造方法，该构造方法用protected字段保护。该构造方法专门在输入参中增加虚假信息的字段。因此为调用该构造方法，并且底层new了一个LinkHashMap.内部用一个entry来保存数据，这个entry中继承了Hashmap里的node并且增加了前后指针。