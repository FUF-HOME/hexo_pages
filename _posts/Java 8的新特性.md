# Java 8的新特性
# 简介
毫无疑问，Java 8是Java自Java 5（发布于2004年）之后的最重要的版本。这个版本包含语言、编译器、库、工具和JVM等方面的十多个新特性。在本文中我们将学习这些新特性，并用实际的例子说明在什么场景下适合使用。

# 2. Java语言的新特性
Java 8 是Java的一个重大版本，在这部分，我们将介绍 Java 8的大部分新特性。
## 2.1 Lambda 表达式和函数式接口
Lambda表达式 是Java 8 中最大和最令人期待的语言改变，它允许我们将函数当成参数传递给某个方法，或者把代码本身当作数据处理。  
最简单的Lambda表达式可由逗号分隔的参数列表， -> 符号和语句块组成，列如：
` Arrays.asList("a","b","c").forEach(e -> System.out.printIn(e)); `
在上面这个代码中的参数e的类型是由编译器推理得出的，你也可以显示指定该参数的类型，例如：  
` Arrays.asList("a","b","c").forEach((String e) -> System.out.printIn(e));`
如果Lambda表达式需要更复杂的语句块，则可以使用花括号将该语句块括起来，类似于Java中的函数体，例如：  
```
Arrays.asList("a","b","c").forEach((String e) ->{ 
    System.out.printIn(e);
    System.out.printIn(e);
});
```
Lambda表达式可以引用类成员和局部变量（会将这些变量隐式得转换成final的）
```
String separator = ",";
Arrays.asList("a","b","c").forEach(
    (String e ) -> System.out.printIn(e+ separator);
)
```
Lambda表达式有返回值，返回值的类型也由编译器推理得出。如果Lambda表达式中的语句块只有一行，则可以不用使用return语句。
```

Array.asList("a","b","c").sort((e1,e2) -> e1.compareTo(e2));
```
```
Array.asList("a","b","c").sort((e1,e2) -> {
    int result = e1.compareTo(e2);
    return reuslt;
});
```
Lambda的设计者们为了让现有的功能与Lambda表达式良好兼容，考虑了很多方法，于是产生了函数接口这个概念。函数接口指的是只有一个函数的接口，这样的接口可以隐式转换为Lambda表达式。java.lang.Runnable和java.util.concurrent.Callable是函数式接口的最佳例子。在实践中，函数式接口非常脆弱：只要某个开发者在该接口中添加一个函数，则该接口就不再是函数式接口进而导致编译失败。为了克服这种代码层面的脆弱性，并显式说明某个接口是函数式接口，Java 8 提供了一个特殊的注解@FunctionalInterface（Java 库中的所有相关接口都已经带有这个注解了），举个简单的函数式接口的定义：
```
@FunctionalInterface
public interface Functional{
    void method();
}
```
不过有一点需要注意，默认方法和静态方法不会破坏函数式接口的定义，因此如下的代码是合法的。
```
@FunctionalInterface
public interface FunctionalDefaultMethods{
    void method();
    default void defaultMehtod();
}
```
Lambda表达式作为Java 8的最大卖点，它有潜力吸引更多的开发者加入到JVM平台，并在纯Java编程中使用函数式编程的概念。如果你需要了解更多Lambda表达式的细节，可以参考官方文档。

## 2.2 接口的默认方法和静态方法

Java 8使用两个新概念扩展了接口的含义：默认方法和静态方法。默认方法使得接口有点类似traits，不过要实现的目标不一样。默认方法使得开发者可以在 不破坏二进制兼容性的前提下，往现存接口中添加新的方法，即不强制那些实现了该接口的类也同时实现这个新加的方法。  
默认方法和抽象方法之间的区别在于抽象方法需要实现，而默认方法不需要。接口提供的默认方法会被接口的实现类继承或者覆写，例子代码如下
```
private inteface Defaulabe{
    default String notReqired(){
        return "Default implementation";
    }
}

private statuc class DefaultbleImpl implements Defaulable{}

private static class OverridableImpl implements Defaulable{
    public String notRequired(){
        return "Overridden implementation";
    }
}
```
Defaulable接口使用关键字default定义了一个默认方法notRequired()。DefaultableImpl类实现了这个接口，同时默认继承了这个接口中的默认方法；OverridableImpl类也实现了这个接口，但覆写了该接口的默认方法，并提供了一个不同的实现。
Java 8带来的另一个有趣的特性是在接口中可以定义静态方法，例子代码如下：
```
private interface DefaulableFactory{
    static Defaulable create(Supplier<Defaulable> supplier){
        return supplier.get();
    }
}
```
下面的代码片段整合了默认方法和静态方法的使用场景：
```
public static void main(String[] args){
    Defaulable defaulable = DefaulableFactory.create(DefaultableImpl::new);
    System.out.printIn(defaulable.notRequired());
    defaulable=DefaulableFactory.create(OverridableImpl::new);
     System.out.println(defaulable.notRequired());
}

```
这段代码的输出结果如下：
```
Default implementation

Overridden implementation
```
由于JVM上的默认方法的实现在字节码层面提供了支持，因此效率非常高。默认方法允许在不打破现有继承体系的基础上改进接口。该特性在官方库中的应用是：给java.util.Collection接口添加新方法，如stream()、parallelStream()、forEach()和removeIf()等等。

尽管默认方法有这么多好处，但在实际开发中应该谨慎使用：在复杂的继承体系中，默认方法可能引起歧义和编译错误。如果你想了解更多细节，可以参考官方文档。

## 2.3 方法引用
方法引用使得开发者可以直接引用现存的方法、Java类的构造方法或者实例对象。方法引用和Lambda表达式配合使用，使得java类的构造方法看起来紧凑而简洁，没有很多复杂的模板代码。

## 2.4 重复注解

# 3. Java编译器的新特性
## 3.1 参数名称
# 4. Java官方库的新特性
## 4.1 Optional
## Streams
新增的Stream API（java.util.stream）将生成环境的函数式编程引入了Java库中。这是目前为止最大的一次对Java库的完善，以便开发者能够写出更加有效、更加简洁和紧凑的代码。
## 4.3 Date/Time API(JSR 310)

## 4.4 Nashorn JavaScript引擎
## 4.5 Base64

## 4.6 并行数组

## 4.7 并发性

# 5. 新的Java工具

# 6. JVM的新特性
使用Metaspace（JEP 122）代替持久代（PermGen space）。在JVM参数方面，使用-XX:MetaSpaceSize和-XX:MaxMetaspaceSize代替原来的-XX:PermSize和-XX:MaxPermSize。
# 7. 结论