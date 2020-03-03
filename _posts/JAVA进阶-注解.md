---
title: JAVA进阶-注解
date: 2019-03-03 21:37:13
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---


# JAVA 注解的基本原理

注解作为解决 XML配置内容复杂，维护成本高的一种标记式配置方法，各种框架中的比重越来越高，特别是在SpringBoot 中。 但是这也不能说明注解就会代替 xml，注解简便，清晰，易于维护，但是也伴随这高耦合。而 xml 相对于注解则是相反的。  
本文意不再辨析两者谁优谁劣，而在于以最简单的语言介绍注解相关的基本内容。
<!-- more -->

## 注解的本质
所有的注解类型都继承自这个普通的接口（Annotation）。因为本质上，Annotion是一种特殊的接口，程序可以通过反射来获取指定程序元素的Annotion对象，然后通过Annotion对象来获取注解里面的元数据。（元数据从metadata一词译来，就是“关于数据的数据”的意思）。可能文字有点抽象。下面用JDK 内置注解代码看看：
```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {

}

```
这是注解 @Override 的定义，其实它本质上就是：
```
public interface Override extends Annotation{
    
}
```
现在代码可以清晰的看到，注解的本质就是继承了 Annotation 接口的接口。

## 注解的分类
### 根据 注解的作用大致可以分为三类：  
- 编写文档：通过代码里面标识的元数据生成文档。
- 代码分析：通过代码里标识的元数据对代码进行分析；
-  编译检查：通过代码里标识的元数据让编译器能实现基本的编译检查；

综上所述可知，Annotation主要用于提升软件的质量和提高软件的生产效率。
### 根据成员个数分类
1. 标记注解：没有定义成员的Annotation类型，自身代表某类信息，如：@Override
2. 单成员注解：定义了多个成员，使用时以name=value对分别提供数据

### 根据注解使用的功能和用途分类
1. 系统内置注解：系统自带的注解类型，如@Override

2. 元注解：注解的注解，负责注解其他注解，如@Target

3. 自定义注解：用户根据自己的需求自定义的注解类型

## 元注解
在分类那部分，提到了对于注解的注解-元注解。那注解还能用在注解上？是的。对于一个注解也是需要去描述该注解的对象，范围，生命周期的。是不是很绕。在 Java5.0 定义了4个标准 meta-annotation 类型，它们被用来描述 其他 annotation 类型 做说明的。
### 1. @Target ：
描述 annotation 修饰的范围，可被用于 packages、types（类、接口、枚举、Annotation类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数）。

### 2. @Retention:
该注解有一个属性value，是RetentionPolicy类型的，Enum RetentionPolicy是一个枚举类型，
表示该注解的生命周期，它应该存在哪个地方。 
1. RetentionPolicy.SOURCE：注解只保留在源文件，当Java文件编译成class文件的时候，注解被遗弃；
2. RetentionPolicy.CLASS：注解被保留到class文件，但jvm加载class文件时候被遗弃，这是默认的生命周期；
3. RetentionPolicy.RUNTIME：注解不仅被保存到class文件中，jvm加载class文件之后，仍然存在；

### 3. @Documented
Documented 注解表明这个注解应该被 javadoc工具记录. 默认情况下,javadoc是不包括注解的. 但如果声明注解时指定了 @Documented,则它会被 javadoc 之类的工具处理, 所以注解类型信息也会被包括在生成的文档中.（个人观点：不是重点，了解即可。勿喷


### 4. @Inherited
这是一个稍微复杂的注解类型. 它指明被注解的类会自动继承. 更具体地说,如果定义注解时使用了 @Inherited 标记,然后用定义的注解来标注另一个父类, 父类又有一个子类(subclass),则父类的所有属性将被继承到它的子类中.