---
title: Java基础知识-源码分析-Map
date: 2019-03-03 21:36:09
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---

# 源码分析-Map
java.util.Map接口表示键和值之间的映射。 Map接口不是Collection接口的子类型。 因此，它的行为与其余集合类型略有不同
<!-- more -->
## Map接口的特征

映射不能包含重复的键，并且每个键最多可以映射到一个值。键值是否为空，HashMap 都允许null，LinkedHashMap 允许

映射的顺序取决于特定的实现，例如TreeMap和LinkedHashMap具有可预测的顺序，而HashMap没有。

有两个用于在Java中实现Map的接口：Map和SortedMap，以及三个类：HashMap，TreeMap和LinkedHashMap。

## Map 中的一些方法
    public Object put（Object key，Object value）：此方法用于在此映射中插入一个条目。
    public void putAll（Map map）：此方法用于在此地图中插入指定的地图。
    public Object remove（Object key）：此方法用于删除指定键的条目。
    public Object get（Object key）：此方法用于返回指定键的值。
    public boolean containsKey（Object key）：此方法用于从该映射图中搜索指定的键。
    public Set keySet（）：此方法用于返回包含所有键的Set视图。
    public Set entrySet（）：此方法用于返回包含所有键和值的Set视图。


# 1. HashMap
HashMap是Java Collection 的一部分。 它提供了Java Map接口的基本实现。 它将数据存储在（key，value）对中。 要访问一个value，必须知道其key。 HashMap被称为HashMap，因为它使用了一种称为Hashing的技术。 hash是一种将大字符串转换为代表相同字符串的小字符串的技术。 较短的值有助于索引编制和更快的搜索。
## HashMap的几个重要功能是：

1. HashMap是java.util包的一部分。

2. HashMap extends了抽象类AbstractMap，该类也提供了Map接口的不完整实现。

3. 它还实现了Cloneable和Serializable接口。 上述定义中的K和V分别表示Key和Value。

4. HashMap不允许重复的key，但允许重复的value。 这意味着单个value不能包含多个value，但是多个key可以包含单个value。

5. HashMap也允许空key，但只允许一次和多个值。

6. 此类无法保证 Map 的顺序。 特别是，它不能保证顺序会随着时间的推移保持恒定。 它与HashTable大致相似，但不同步。




# 2. LinkedHashMap

LinkedHashMap就像HashMap一样，具有保持插入元素顺序的功能。 HashMap提供了快速插入，搜索和删除的优势，但从未保持LinkedHashMap提供的插入轨迹和顺序，而LinkedHashMap提供了按**插入顺序访问元素**的位置。 

## LinkedHashMap的几个重要功能如下

1. LinkedHashMap包含基于键的值。 它 implements 了Map接口并 extends 了HashMap类。

2. 它仅包含唯一元素。
  
3. 它可能具有一个null键和多个null值。

4. 它与HashMap相同，具有保持插入顺序的附加功能。 例如，当我们使用HashMap运行代码时，我们得到了不同顺序的元素。


## LinkedHashMap 中的方法
    void clear（）：此方法用于从地图中删除所有映射。

    boolean containsKey（Object key）：如果一个指定元素由一个或多个键映射，则此方法用于返回true。

    Object get（Object key）：该方法用于检索或获取由指定键映射的值。

    protected boolean removeEldestEntry（Map.Entry eldest）：当地图从地图上删除其最旧的条目时，该方法用于返回true。

    entrySet？（）：此方法返回此映射中包含的映射的Set视图。

    forEach？（BiConsumer <K，V>action）：此方法对该映射中的每个条目执行给定的操作，直到已处理完所有条目或该操作引发异常。

    getOrDefault？（Object key，V defaultValue）：此方法返回指定键所映射到的值；如果此映射不包含该键的映射，则返回defaultValue。

    keySet？（）：此方法返回此映射中包含的键的Set视图。

    removeEldestEntry？（Map.Entry <K，V> eldest）：如果此地图应删除其最旧的条目，则此方法返回true。

    replaceAll？（BiFunction <K，V>function）：该方法将在该条目上调用给定函数的结果替换每个条目的值，直到处理完所有条目或该函数引发异常为止。

    values？（）：此方法返回此映射中包含的值的Collection视图。

# 3，TreeMap

Java中的TreeMap用于实现Map接口和NavigableMap以及Abstract类。 根据映射键的自然顺序或在映射创建时提供的Comparator对映射进行排序，具体取决于所使用的构造函数。 事实证明，这是排序和存储键值对的有效方法。 不论显式比较器如何，树图所维护的存储顺序都必须与equals保持一致，就像任何其他排序图一样。 树形图实现在某种意义上是不同步的，即如果一个图同时被多个线程访问，并且至少一个线程在结构上修改了该图，则必须在外部进行同步。 
TreeMap的一些重要功能包括：

1. 该类实现Map接口，包括NavigableMap，SortedMap并扩展AbstractMap

2. Java中的TreeMap不允许使用空key，因此会引发NullPointerException。 但是，多个空值可以与不同的键关联。

3. 此类中的方法返回的所有Map.Entry对及其视图均表示生成映射时的快照。 它们不支持Entry.setValue方法。


# 4. ConcurrentHashMap
