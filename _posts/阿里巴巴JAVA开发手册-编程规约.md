---
title: 阿里巴巴JAVA开发手册-编程规约
date: 2019-08-17 20:21:12
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - JAVA,Alibaba
blogexcerpt:
---


# 编程规约
本节中基本是一些对于编程风格，对于程序员之间的代码编码编写进行统一要求，使得代码的阅读变得更为清晰，易懂。具体可以查看 1.5.0 版本的 JAVA 开发手册，这里主要挑出一些平时可能会忽略，易错的部分。

#  (一) 命名风格
(6)  抽象类名使用 **Abstract 或 Base** 开头，异常类命名使用 **Exception** 结尾，测试类命名以它要测试的类的名称开始，以 **Test** 结尾。  

(8)  POJO 类中 Boolean 类型变量**都不要加 is 前缀**。否则部分框架解析会引起序列化错误。

> 反例：定义为基本数据类型 Boolean isDeleted 的属性，它的方法也是 isDeleted()，RPC 框架在反向解析的时候，“误以为”对应的属性名称是 deleted，导致属性获取不到，进而抛出异常。

(13) 在常量与变量的命名时，表示类型的名称放在词尾，以提升辨识度。  
> 正例：startTime，workQueue    
反例：startedAt,QueueOfWork  

(15)  接口类中的方法和属性不要加任何修饰符号（public 也不要加），保持代码的简洁性，并加上有效的 Javadoc注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是与接口方法相关，并且是整个应用的基础常量。

> 正例：接口方法签名 void commit();
接口基础常量 String COMPANY = "alibaba";  
反例：接口方法定义 public abstract void f();
> 说明：JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的默认实现。

(17)  枚举类名带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开。  

> 说明：枚举其实就是特殊的类，域成员均为常量，且构造方法被默认强制是私有。

> 正例：枚举名字为 ProcessStatusEnum 的成员名称：SUCCESS / UNKNOWN_REASON。

(18).各层命名规约：  

1. Service / DAO 层方法命名规约  
    - 获取单个对象的方法用 get 做前缀。  
    - 获取多个对象的方法用 list 做前缀，复数形式结尾。  
    - 获取统计值的方法用 count 做前缀。  
    - 插入的方法用 save / insert 做前缀。  
    - 删除的方法用 remove / delete 做前缀。  
    - 修改的方法用 update 做前缀。  
2. 领域模型命名规约   
    - 数据对象：xxxDO，xxx为数据表名。  
    - 数据传输对象：xxxDTO，xxx为业务领域相关的名称。  
    - 展示对象：xxxVO，xxx一般为网页名称。  
> - POJO 是 DO / DTO / BO / VO 的统称，禁止命名成 xxxPOJO。  

# （二）常量定义
(3) 不要使用一个常量类维护所有变量，要按照功能进行归类，分开维护
> 正例：缓存相关常量放在类 CacheConsts 下；系统配置相关常量放在类 ConfigConsts 下。

(4).常量的复用层次有五层：**跨应用共享常量、应用内共享常量、子工程内共享常量、包内共享常量、类内共享常量。**

- 跨应用共享常量：放置二方库中，通常是 client.jar 中的 constant 目录下。

- 应用内共享常量：放置一方库中，通常是子模块中的 constant 目录下。

- 子工程内共享常量：即在当前子工程的 constant 目录下。

- 包内共享常量：即在当前包下单独的 constant 目录下。

- 类内共享常量：直接在类内部 private static final 定义。

(5).如果变量值仅在一个固定范围内变化用 enum 类型来定义。

> 说明：如果存在名称之外的延伸属性应使用 enum 类型，下面正例中的数字就是延伸信息，表示一年中的第几个季节。                                     

# (三) 代码格式


# (四) OOP 规约
- 3 尽量避免使用可变参数，若要使用，必须放参数列表的**最后**。

    > 正例：public List listUsers(String type, Long... ids) {.,..}


- 6 Object 的 equals 方法容易抛空指针异常，应使用常量或确定有值的对象来调用 equals 方法，推荐使用 java.util.Objects 中的 equals 方法。
    > 正例："test".equals(object);
- 7 所有的整数包装类对象之间的值的比较，全部使用 equals 方法比较
    > 说明：对于 Integer 包装类，在 -128 至 127 的赋值，Integer 对象是在 IntegerCache.cache 产生的，会复用已有对象，这个区间内的 Integer 值用 == 进行判断也可以，但区间以外的值，都会在堆上产生，不会复用对象，只能用 equals 方法判断。

- 8 浮点数值，无论是基本类型还是包装类型，都不能直接判断比较，即不能使用 == 和 equals 判断。

    > 说明：浮点数采用“尾数+阶码”的编码格式，二进制无法精确表示大部分的十进制小数，精度不准确。  
    1. 正例：指定一个误差范围，两个浮点数的差值在此范围类，就认为是相等的。
    ```
    float a = 1.0f-0.9f;
    float a = 0.9f-0.8f;
    float diff = 1e-6f;
    
    if(Math.abs(a-b) < diff ){
        System.out.printIn("TURE");
    }
    ```
  
- 10.为了防止精度缺失，使用 BigDecimal 时，不能使用 BigDecimal(double) 的构造方法，应使用 BigDecimal(String) 或 valueOf 方法。
- 11.基本数据类型和包装数据类型使用标准如下：

    - 所有的 POJO 类属性必须使用包装数据类型。

    - RPC 方法的返回值和参数必须使用包装数据类型。

    - 所有的局部变量使用基本数据类型。

    > 说明：POJO 类属性在没有初值是提醒使用者在需要使用时，必须自己显示的进行赋值，任何 NPE 问题，或者入库检查，都由使用者来保证。

- 12 定义 DO/DTO/VO 等 POJO 类时，不要设定任何属性默认值。
    > 反例：POJO 类的 createTime 默认值为 new Date(),但是这个属性在数据提取时并没有置入具体值，在更新其他字段有附带了此字段，导致创建时间被修改成当前时间。
- 15 POJO 类必须写 toString() 方法。

- 16 POJO 类不能有同一属性 xxx 的 isXxx 方法和 getXxx() 方法
- 17 使用索引访问用 String 的 split 方法得到的数组时，需做最后一个分隔符后有无内容的检查，否则会有抛 IndexOutOfBoundsException 的风险。
- 21.循坏体内，字符串的连接方式，使用 StringBuilder 的 append 方法进行扩展。

    > 说明：下例中，反编译出的字节码文件显示每次循环都会 new 出一个 StringBuilder 对象，然后进行 append 操作，最后通过 toString 方法返回 String 对象，造成内存资源浪费。
    ```
    String str = "start";
    for(int i = 0; i < 100; i++ ){
        str = str + "hello";
    }

    ```
- 23.慎用 Object 的 clone 方法 来拷贝对象。

    > 说明：对象 clone 方法默认是浅拷贝，若想实现深拷贝需重写 clone 方法。

- 24.类成员和方法访问控制从严：

    - 如果不允许外部直接通过 new 来创建对象，那么构造方法必须是 private。

    - 工具类不允许有 public 和 default 构造方法。

    - 类非 static 成员变量并且与子类共享，必须是 protected。

    - 类非 static 成员变量仅在本类使用，必须是 private。

    - 类 static 成员变量仅在本类使用，必须是 private。

    - 若是 static 成员变量，考虑是否为 final。
    - 类成员方法只供类内部调用，必须是 private。
    - 类成员方法只对继承类公开，限制为 protected。

    > 说明：任何类、方法、参数、变量，严控访问范围。过于宽泛的访问范围，不利于模块解耦。

# (五 ) 集合处理
- 1.关于 hashCode 和 equals 的处理，遵循以下规则：

    - 只要重写 equals，就必须重写 hashCode。
    - 因为 Set 存储的是不重复的对象，依据 hashCode 和 equals 进行判断，所以 Set 存储的对象必须重写这两个方法。
    - 作为 Map 的 key 的对象也必须重写这两个方法。
    > String 已经重写了这两个方法，所以可以使用 String 对象作为 key 来使用
- 2. ArrayList 的 subList 结果不可强转成 ArrayList，否则会抛出 ClassCastException 异常，即 java.util.RandomAccessSubList cannot be cast to java.util.ArrayList。

    > 说明：subList 返回的是 ArrayList 的内部类 SubList，并不是 ArrayList 而是 ArrayList 的一个视图，对于 SubList 子列表的所有操作最终会反映到原列表上。

- 3.使用 Map 的方法 keySet() / values() / entrySet() 返回集合对象时，不可以对其进行添加元素操作，否则会抛出UnsupportedOperationException 异常。
- 5.在 subList 场景中，高度注意对原集合元素的增加或删除，均会导致子列表的遍历、增加、删除产生 ConcurrentModificationException 异常。
- 6.使用集合转数组的方法，必须使用集合的 toArray(T[] array)，传入的是类型完全一致、长度为 0 的空数组。
     > 说明：使用 toArray 带参方法，数组空间大小的 length：

    - 1） 等于 0，动态创建与 size 相同的数组，性能最好。

    - 2） 大于 0 但小于 size，重新创建大小等于 size 的数组，增加 GC 负担。

    - 3） 等于 size，在高并发情况下，数组创建完成之后，size 正在变大的情况下，负面影响与上相同。

    - 4） 大于 size，空间浪费，且在 size 处插入 null 值，存在 NPE 隐患。
- 7.在使用 Collection 接口任何实现类的 addAll()方法时，都要对输入的集合参数进行 NPE 判断。

    > 说明：在 ArrayList#addAll 方法的第一行代码即 Object[] a = c.toArray(); 其中 c 为输入集合参数，如果为 null，则直接抛出异常。
- 11.不要在 foreach 循环中进行元素的 remove / add 操作。 remove 元素请使用 Iterator 方式。如果是并发操作，需要对 Iterator 对象加锁。

- 13.集合泛型定义时，在 JDK7 及以上，使用 diamond 语法或全省略。
     ```
     // diamond 方式，即<> 
     HashMap<String, String> userCache = new HashMap<>(16); 
     // 全省略方式 
     ArrayList<User> users = new ArrayList(10); 
     ```

- 15.遍历 Map 类集合应使用 entrySet，而不是 keySet。
    > 说明：keySet 其实是遍历了 2 次，一次是转为 Iterator 对象，另一次是从 hashMap 中取出 key 所对应的 value。而 entrySet 只是遍历了一次就把 key 和 value 都放到了 entry 中，效率更高。如果是 JDK8，使用 Map.forEach 方法。
- 16.高度注意 Map 类集合 K/V 能不能存储 null 值的情况。
- 18.对集合进行去重操作应使用 Set 的特性，而不是使用 List 的 contains 方法。

# （六）并发控制
- 1.获取单例对象需要保证线程安全，其中的方法也要保证线程安全。
    > 说明：资源驱动类，工具类，单例工厂类都需要注意。
- 3.线程资源必须通过线程池提供，不允许在应用中自行显式创建线程。

    - 说明：线程池的好处是减少在创建和销毁线程上所消耗的时间以及系统资源的开销，解决资源不足的问题。如果不使用线程池，有可能造成系统创建大量同类线程而导致消耗完内存或者“过度切换”的问题。

- 4.线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。
- 6.必须回收自定义的 ThreadLocal 变量，尤其在线程池场景下，线程经常会被复用，如果不清理自定义的 ThreadLocal 变量，可能会影响后续业务逻辑和造成内存泄露等问题。尽量在代理中使用 try-finally 块进行回收。
- 8.对多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则可能会造成死锁。
    > 说明：线程一需要对表 A,B,C 依次全部加锁后才可以进行更新操作，那么线程二的加锁顺序也必须时 A,B,C，否则可能出现死锁。
- 9.在使用阻塞等待获取锁的方式中，必须在 try 代码块之外，并且在加锁方法与 try 代码块之间没有任何可能抛出异常的方法调用，避免加锁成功后，在 finally 中无法解锁。
    ```
    // 正例：
    Lock lock = new XxxLock();
    // ...
    lock.lock();
    try {
        doSomething();
        doOthers();
    } finally {
        lock.unlock();
    }
    // 反例：
    Lock lock = new XxxLock();
    // ...
    try {
        // 如果此处抛出异常，则直接执行 finally 代码块
        doSomething();
        // 无论加锁是否成功，finally 代码块都会执行
        lock.lock();
        doOthers();
    } finally {
        lock.unlock();
    }

     ```
- 10.在使用尝试机制来获取锁的方式中，进入业务代码块之前，必须先判断当前线程是否持有锁。锁的释放规则与锁的阻塞等待方式相同。
    ```
        // 正例：
    Lock lock = new XxxLock();
    // ...
    boolean isLocked = lock.tryLock();
    if (isLocked) {
        try {
            doSomething();
            doOthers();
        } finally {
            lock.unlock();
        } }
    ```
- 15.避免 Random 实例被多线程使用，虽然共享该实例是线程安全的，但会因竞争同一 seed 导致的性能下降。
    > 说明：Random 实例包括 java.util.Random 的实例或者 Math.random() 的方式。

    > 正例：在 JDK7 之后，可以直接使用 API ThreadLocalRandom，而在 JDK7 之前，需要编码保证每个线程持有一个实例。

# （七）控制语句
- 1.在一个 switch 块内，每个 case 要么通过 continue / break / return 等来终止，要么注释说明程序将继续执行到哪一个 case 为止；在一个 switch 块内，都必须包含一个 default 语句并且放在最后，即使它什么代码也没有。
     > 说明 break 是推出 switch 语句块，而 return 是推出方法体

- 2 当 switch 括号内的变量类型为 String 并且此变量为外部参数，必须先进行 null 判断
- 5.在表达特殊情况的分支时，少用 if - else 方式。
    ```
    if(condition){
        return obj;
    }

    ```

    > 说明：如果非使用 if()...else if()...else...方式表达逻辑，请勿超过 3 层。

    > 正例：超过 3 层的 if-else 的逻辑判断代码可以使用卫语句、策略模式、状态模式等来实现，其中卫语句即代码逻辑先考虑失败、异常、中断、退出等直接返回的情况，以方法多个出口的方式，解决代码中判断分支嵌套的问题，这是逆向思维的体现。
# (八）注释规约
- 1.类、类属性、类方法的注释必须使用 Javadoc 规范，使用/**内容*/格式，不得使用 // xxx 方式。
- 2.所有的抽象方法（接口中的方法）必须要用 Javadoc 注释、除了返回值、参数、异常说明外，还必须指出该方法做什么事，实现什么功能。
- 3.所有的类都必须添加创建者和创建日期。
- 4.方法内部单行注释，在被注释语句上方另起一行，使用//注释。方法内部多行注释使用/* */注释，注意与代码对齐。
- 5.所有的枚举类型字段必须要有注释，说明每个数据项的用途。

# （九）其它
- 1.在使用正则表达式时，利用好其预编译功能，可以有效加快正则匹配速度。
  - 说明：不要在方法体内定义：Pattern patterm = Pattern.compile("规则");
- 3.后台输送给页面的变量必须加$!{var}——中间的感叹号。
  - 如果 var 为 null 或者为空，那么会直接将$(var)显示在页面上。
- 6.日期格式化时，注意大小写。
  - 表示日期和时间的格式：new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"); 小写 h 为 12 小时制。
- 7.不要在视图模板加入任何复杂的逻辑，视图的职责是展示，不要抢模型和控制器的活。