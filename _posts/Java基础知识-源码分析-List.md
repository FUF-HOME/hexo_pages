---
title: Java基础知识-源码分析-List
date: 2019-03-03 21:35:53
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---
 
# 源码分析-List
List是**有序**的**Collection**，使用此接口能够精确的控制每个元素插入的位置。用户能够**使用索引**（元素在List中的位置，类似于数组下标）来访问List中的元素。

# 1. ArrayList
ArrayList是Collection框架的一部分，出现在java.util包。它为我们提供了Java中的动态数组。虽然，它可能比标准数组慢，但在需要大量操作数组的程序中很有用。
- ArrayList继承AbstractList类并实现List接口。

- ArrayList是通过大小初始化的，但是如果集合增加，则大小会增加，如果从集合中删除对象，则大小会缩小。

- Java ArrayList允许我们随机访问列表。

- ArrayList不能用于基本类型，例如int，char等。在这种情况下，我们需要一个包装器类。
<!-- more -->
## ArrayList 中的方法
    forEach（Consumer <？super E>操作）：对Iterable的每个元素执行给定的操作，直到所有元素都已处理或该操作引发异常为止。

    keepAll（Collection <？> c）：仅保留此列表中包含在指定集合中的元素。

    removeIf（Predicate <？super E>过滤器）：移除此集合中满足给定谓词的所有元素。

    contains（Object o）：如果此列表包含指定的元素，则返回true。

    remove（int index）：删除此列表中指定位置的元素。

    remove（Object o）：从列表中删除第一次出现的指定元素（如果存在）。

    get（int index）：返回此列表中指定位置的元素。

    subList（int fromIndex，int toIndex）：返回此列表中指定的fromIndex（包括在内）和toIndex（不包括在内）之间的部分视图。

    spliterator（）：在此列表中的元素上创建后绑定和故障快速的Spliterator。

    set（int index，E element）：用指定的元素替换此列表中指定位置的元素。

    size（）：返回此列表中的元素数。

    removeAll（Collection <？> c）：从此列表中删除指定集合中包含的所有元素。

    sureCapacity（int minCapacity）：如有必要，增加此ArrayList实例的容量，以确保它至少可以容纳最小容量参数指定的元素数量。

    listIterator（）：返回此列表中元素的列表迭代器（按适当顺序）。

    listIterator（int index）：从列表中的指定位置开始以适当的顺序返回列表中元素的列表迭代器。

    isEmpty（）：如果此列表不包含任何元素，则返回true。

    removeRange（int fromIndex，int toIndex）：从列表中删除所有索引介于fromIndex（包括）和toIndex（不包括）之间的所有元素。

    void clear（）：此方法用于从任何列表中删除所有元素。

    void add（int index，Object element）：此方法用于在列表中特定位置的索引处插入特定元素。

    void trimToSize（）：此方法用于将ArrayLis实例的容量调整为列表的当前大小。

    int indexOf（Object O）：

    index 返回特定元素的第一次出现，或者返回-1（如果该元素不在列表中）。

    int lastIndexOf（Object O）：返回最后一次出现的特定元素的索引；如果元素不在列表中，则返回-1。

    Object clone（）：此方法用于返回ArrayList的浅表副本。

    Object [] toArray（）：此方法用于按正确顺序返回包含列表中所有元素的数组。

    Object [] toArray（Object [] O）：还用于以与先前方法相同的正确顺序返回包含此列表中所有元素的数组。

    boolean addAll（Collection C）：此方法用于将特定集合中的所有元素附加到上述列表的末尾，其顺序是由指定集合的​​迭代器返回值。

    boolean add（Object o）：此方法用于将特定元素附加到列表的末尾。

    boolean addAll（int index，Collection C）：用于将特定集合中从指定位置开始的所有元素插入提到的列表中。


## Array vs ArrayList in Java
1. Array是Java提供的基本功能。ArrayList是Java中集合框架的一部分。因此，使用[]访问数组成员，而ArrayList有一组方法来访问和修改元素。
2. Array是固定大小的数据结构，而ArrayList不是。在创建Arraylist对象时，不需要提到它的大小。即使我们指定了一些初始容量，我们也可以添加更多的元素。
```
import java.util.ArrayList; 
import java.util.Arrays; 
class Test 
{ 
    public static void main(String args[]) 
    { 
        /* ........... Normal Array............. */
        // Need to specify the size for array  
        int[] arr = new int[3]; 
        arr[0] = 1; 
        arr[1] = 2; 
        arr[2] = 3; 
        // We cannot add more elements to array arr[] 
  
        /*............ArrayList..............*/
        // Need not to specify size  
        ArrayList<Integer> arrL = new ArrayList<Integer>(); 
        arrL.add(1); 
        arrL.add(2); 
        arrL.add(3); 
        arrL.add(4); 
        // We can add more elements to arrL 
  
        System.out.println(arrL); 
        System.out.println(Arrays.toString(arr)); 
    } 
} 

```

3. 根据数组的定义，数组既可以包含基本数据类型，也可以包含类的对象。然而，ArrayList只支持对象，而不支持基本数据类型。
4. 由于ArrayList不能为原始数据类型创建，所以ArrayList的成员总是引用不同内存位置的对象(有关详细信息，请参阅这一点)。因此，在ArrayList中，实际对象从不存储在连续的位置。实际对象的引用存储在连续的位置。

# 2. LinkedList
LinkedList是**线性数据结构**，其中元素存储在**不连续的空间**，并且每个元素都是具有数据部分和地址部分的单独对象。 元素使用指针和地址链接。 每个元素称为一个**节点**。 它有着**动态性**以及**插入和删除的简便性**的优点。 它还有一些缺点，例如**无法直接访问节点**，而我们需要从头开始，然后通过链接到达要访问的节点。

为了将元素存储在链表中，我们使用双链表，它提供了线性数据结构，还用于继承抽象类并实现链表和双端队列。

在Java中，LinkedList类实现List接口。 LinkedList类还包括其他Java集合一样的各种构造函数和方法。

## LinkedList 中的方法
    add（int index，E element）：此方法将指定的元素插入此列表中的指定位置。

    add（E e）：此方法将指定的元素追加到此列表的末尾。

    addAll（int index，Collection c）：此方法从指定位置开始将指定集合中的所有元素插入此列表。

    addAll（Collection c）：此方法将指定集合中的所有元素按指定集合的​​迭代器返回的顺序追加到此列表的末尾。

    addFirst（E e）：此方法将指定的元素插入此列表的开头。

    addLast（E e）：此方法将指定的元素追加到此列表的末尾。

    clear（）：此方法从此列表中删除所有元素。

    clone（）：此方法返回此LinkedList的浅表副本。

    contains（对象o）：如果此列表包含指定的元素，则此方法返回true。

    DescendIterator（）：此方法以相反的顺序在此双端队列中的元素上返回迭代器。

    element（）：此方法检索但不删除此列表的头（第一个元素）。

    get（int index）：此方法返回此列表中指定位置的元素。

    getFirst（）：此方法返回此列表中的第一个元素。

    getLast（）：此方法返回此列表中的最后一个元素。

    indexOf（Object o）：此方法返回此列表中指定元素的首次出现的索引;如果此列表不包含该元素，则返回-1。

    lastIndexOf（Object o）：此方法返回此列表中指定元素的最后一次出现的索引;如果此列表不包含该元素，则返回-1。

    listIterator（int index）：此方法返回列表中元素的列表迭代器（按适当顺序），从列表中的指定位置开始。

    offer（E e）：此方法将指定元素添加到此列表的末尾（最后一个元素）。

    offerFirst（E e）：此方法将指定的元素插入此列表的开头。

    offerLast（E e）：此方法将指定的元素插入此列表的末尾。

    peek（）：此方法检索但不删除此列表的头（第一个元素）。

    peekFirst（）：此方法检索但不删除此列表的第一个元素，如果此列表为空，则返回null。

    peekLast（）：此方法检索但不删除此列表的最后一个元素，如果此列表为空，则返回null。

    poll（）：此方法检索并删除此列表的头（第一个元素）。

    pollFirst（）：此方法检索并删除此列表的第一个元素，如果此列表为空，则返回null。

    pollLast（）：此方法检索并删除此列表的最后一个元素，如果此列表为空，则返回null。

    pop（）：此方法从此列表表示的堆栈中弹出一个元素。

    push（E e）：此方法将元素压入此列表表示的堆栈。

    remove（）：此方法检索并删除此列表的头（第一个元素）。

    remove（int index）：此方法删除此列表中指定位置的元素。

    remove（对象o）：此方法从列表中删除指定元素的第一次出现（如果存在）。

    removeFirst（）：此方法从列表中删除并返回第一个元素。

    removeFirstOccurrence（对象o）：此方法删除列表中指定元素的首次出现（当从头到尾遍历列表时）。

    removeLast（）：此方法从列表中删除并返回最后一个元素。

    removeLastOccurrence（对象o）：此方法删除此列表中最后一次出现的指定元素（从头到尾遍历列表时）。

    set（int index，E element）：此方法用指定的元素替换此列表中指定位置的元素。

    size（）：此方法返回此列表中的元素数。

    spliterator（）：此方法在此列表中的元素上创建后绑定和故障快速的Spliterator。

    toArray（）：此方法以正确的顺序（从第一个到最后一个元素）返回一个包含此列表中所有元素的数组。

    toArray（T [] a）：此方法返回一个包含此列表中所有元素的数组，该数组按适当顺序排列（从第一个元素到最后一个元素）; 返回数组的运行时类型是指定数组的运行时类型。 



# 3. CopyOnWriteArrayList
CopyOnWriteArrayList: CopyOnWriteArrayList类是在JDK 1.5中引入的，它实现了List接口。它是ArrayList的增强版，其中所有修改(add, set, remove等)都是通过创建一个新的副本来实现的。

## 以下是关于CopyOnWriteArrayList的几点注意事项
- 顾名思义，CopyOnWriteArrayList会创建基础ArrayList的克隆副本，对于每个更新操作，在某些时候都将自动同步，这由JVM负责。 因此，对于正在执行读取操作的线程没有影响。

- 使用它的成本很高，因为将为每个更新操作创建一个克隆副本。 因此，如果我们的频繁操作是读取操作，则CopyOnWriteArrayList是最佳选择。

- 它扩展了对象并实现了Serializable，Cloneable，Iterable <E>，Collection <E>，List <E>和RandomAccess

- 带下划线的数据结构是可增长数组。

- 它是ArrayList的线程安全版本。

- 保留插入，允许重复，并允许异构对象。

- 关于CopyOnWriteArrayList的主要要点是CopyOnWriteArrayList的迭代器无法执行删除操作，否则我们会得到运行时异常，表示UnsupportedOperationException。

https://www.geeksforgeeks.org/copyonwritearraylist-in-java/