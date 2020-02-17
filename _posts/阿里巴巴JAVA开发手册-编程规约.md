
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