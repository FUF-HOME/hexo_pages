# Java基础知识-反射


# Java 反射机制
Java 反射机制在**程序运行时**，对于**任意一个类**，都能够知道这个类的**所有属性和方法**；对于任意一个对象，都能够调用它的任意一个方法和属性。这种 **动态的获取信息 以及 动态调用对象的方法** 的功能称为 java 的反射机制。  

反射机制很重要的一点就是“运行时”，其使得我们可以在程序运行时加载、探索以及使用编译期间完全未知的 .class 文件。换句话说，Java 程序可以加载一个运行时才得知名称的 .class 文件，然后获悉其完整构造，并生成其对象实体、或对其 fields（变量）设值、或调用其 methods（方法）。

https://juejin.im/post/598ea9116fb9a03c335a99a4#heading-2

# 1. 提供api方法取得类的结构；
1. Class<NameClass> NameClass = 类名.class
2. NameClass.getFields() // 得到所有变量

NameClass 里面存储了类的信息。

# 2. 调用类的方法
```
private static void getPrivateMethod() throws Exception{
    //1. 获取 Class 类实例
    TestClass testClass = new TestClass();
    Class mClass = testClass.getClass();
    
    //2. 获取私有方法
    //第一个参数为要获取的私有方法的名称
    //第二个为要获取方法的参数的类型，参数为 Class...，没有参数就是null
    //方法参数也可这么写 ：new Class[]{String.class , int.class}
    Method privateMethod =
            mClass.getDeclaredMethod("privateMethod", String.class, int.class);
            
    //3. 开始操作方法
    if (privateMethod != null) {
        //获取私有方法的访问权
        //只是获取访问权，并不是修改实际权限
        privateMethod.setAccessible(true);
        
        //使用 invoke 反射调用私有方法
        //privateMethod 是获取到的私有方法
        //testClass 要操作的对象
        //后面两个参数传实参
        privateMethod.invoke(testClass, "Java Reflect ", 666);
    }
}
```
需要注意的是，第3步中的 setAccessible(true) 方法，是获取私有方法的访问权限，如果不加会报异常 IllegalAccessException，因为当前方法访问权限是“private”的.

# 代理
https://juejin.im/post/5ad3e6b36fb9a028ba1fee6a

# 3. 动态代理

# 什么是动态代理
代理类在程序运行时创建的代理方式被成为 动态代理。 也就是说，这种情况下，代理类并不是在Java代码中定义的，而是在运行时根据我们在Java代码中的“指示”动态生成的。相比于静态代理， 动态代理的优势在于可以很方便的对代理类的函数进行统一的处理，而不用修改每个代理类的函数。 这么说比较抽象，下面我们结合一个实例来介绍一下动态代理的这个优势是怎么体现的。
现在，假设我们要实现这样一个需求：在执行委托类中的方法之前输出“before”，在执行完毕后输出“after”。我们还是以上面例子中的Vendor类作为委托类，BusinessAgent类作为代理类来进行介绍。首先我们来使用静态代理来实现这一需求，相关代码如下：
```


```