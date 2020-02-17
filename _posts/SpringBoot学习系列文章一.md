---
title: SpringBoot学习系列文章一
date: 2018-06-17 12:04:36
author: fuf
notebook: blog
evernote-version: 0
source: 原创
thumbnail: 
tags:
    - SpringBoot
blogexcerpt:
---


# SpringBoot
SpringBoot是伴随着Spring4.0诞生的； SpringBoot的目标是简化Spring的开发过程、让开发者快速搭建框架和web容器。并为微服务提供更好的支持，提供服务监控能力。Spring为开发者带来了简单和能力：
1. Spring Boot使编码变简单

2. Spring Boot使配置变简单

3. Spring Boot使监控变简单

4. Spring Boot使部署变简单

![undefined](https://ws1.sinaimg.cn/large/006wG1mNgy1gbzbduygosj3089089aam.jpg)
 

# 一 工具准备
"工欲善其事必先利其器", 有一套趁手的开发工具和环境能够让你事半功倍。
现在我们选择一款开发工具并且配置号开发环境。
 1. IDE：IIntelliJ IDEA 强烈推荐这款ide。功能齐全，好看(只是重点)。
 2. jdk1.8

开发工具，开发环境都配置好了，可以建立Java project，能正常运行就可以了

# 二 SpringBoot 知识入门
入门一个新的框架或者知识点，我建议理论联系实例，在接触一个新的框架，知识点，最开始是会被一些知识术语所迷惑，不明白这些技术术语词汇的定义、概念、含义，没有这些做根基，就很难做到掌握和学习这个技术，并达到融汇贯通的程度。  
所以，**在学习SpringBoot ，首先要从宏观的层面上**，去了解这个技术的发展，背景，使用场景，演进历史，这一部分，可以在官方网站上去了解。对比其他知识的学习也是一样。先在官网上读懂文档，再去下手。一般文档里面包含了掌握该技术需要了解基本知识。Spring 官网  https://spring.io/ 去获取最权威的介绍和定义

   
# 三 Spring 技术基础知识
SpringBoot 并不是一个 构建 服务的基础框架，它是基于Spring开发，目的在于简化Spring的开发，让开发者快速搭建框架和web容器。所以如果你想要深入理解SpringBoot那就必须掌握好Spring。
Spring 主要 有两个重要的知识点
  1. IOC：控制反转
  2. AOP

## IOC
　Ioc—Inversion of Control，即“控制反转”，不是什么技术，而是一种设计思想。在Java开发中，Ioc意味着将你设计好的对象交给容器控制，而不是传统的在你的对象内部直接控制。如何理解好Ioc呢？理解好Ioc的关键是要明确“谁控制谁，控制什么，为何是反转（有反转就应该有正转了），哪些方面反转了”，那我们来深入分析一下：

1. 谁控制谁，控制什么：
   传统Java SE程序设计，我们直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而IoC是有专门一个容器来创建这些对象，即由Ioc容器来控制对 象的创建；谁控制谁？当然是IoC 容器控制了对象；控制什么？那就是主要控制了外部资源获取（不只是对象包括比如文件等）。

2. 为何是反转，哪些方面反转了：
   有反转就有正转，传统应用程序是由我们自己在对象中主动控制去直接获取依赖对象，也就是正转；而反转则是由容器来帮忙创建及注入依赖对象；为何是反转？因为由容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象，所以是反转；哪些方面反转了？依赖对象的获取被反转了。

**这里需要补充一个知识点：** Spring中，存在一个 Bean工厂。我们把每一个java类当做是一个 bean，Spring就可以当做是一个factory（工厂），bean factory（工厂）的功能就是专门生产bean的。也就是说：Spring 可以去生产类的对象，也即 实例化类对象（new 类名();）。


## Spring AOP
面向切面编程，且解决横切性的问题。  

什么是横切性的问题？比如开发中，有很多的类、很多的方法，类与类之间存在调用的依赖的关系，我们称之为“从上而下”的线性调用。在这些代码中，经常需要在很多位置，添加“打印日志”的代码。而这些，“打印日志”的代码，基本都是一样的，和“从上而下”的线性调用，没有什么直接的业务逻辑关系。我们可以称之为：横切到这个“从上而下”的线性中。就像一个“十字”、“垂直”、“正交”这样。  

除了“日志”属于横切性问题，“事务”也属于。  

AOP就是为了解决这种横切性的问题，通过配置，不让这些相同的代码，充斥在项目代码的各处。而是通过，很少的配置，把这些相似的横切性代码，配置到它们应该出现的位置。

AOP也需要了解一些，专门的术语，我们这里只是简单的介绍一下，AOP需要说清楚，还要写专门的文章，去举例和描述。

# SpingBoot 的 Hello World
在Idea 中。创建一个SpringBoot 的Hello Word程序，去初步的体验，和接触Spring Boot ，有个直观的感受和印象。有助于后面慢慢的去深入了解和学习掌握这个技术。  

1. 通过SPRING INITIALIZR工具产生基础项目这里使用
访问：http://start.spring.io/
选择构建工具Maven Project、Spring Boot版本以及一些工程基本信息，点击Generate Project下载项目压缩包 或者在Idea 里面选择Spring init 选项自动生成一个基础项目
2. 解压项目包，并用idea以Maven项目导入(如果你是使用idea自动生成的，可以省略这一步)


通过上面步骤完成了基础项目的创建，如上图所示，Spring Boot的基础结构共三个文件（具体路径根据用户生成项目时填写的Group所有差异）：
 
```
src/main/java下的程序入口：Chapter1Application
src/main/resources下的配置文件：application.properties
src/test/下的测试入口：Chapter1ApplicationTests
```

引入Web模块，需添加spring-boot-starter-web模块：

```
<dependency>
  <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### 编写HelloWord服务
1. 创建package命名为com.fufhome.web
   1. com 代表所属，可以选择com：，org：组织,cn:中国，有多种标识)
   2. fuf：一般是个人身份标识
   3. web：功能包名称
2. 上面创建了一个web package接下来在web包里面创建一个类 HelloController。内容如下：

```
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String index() {
        return "Hello World";
    }
}
```
3. 启动主程序，打开浏览器访问http://localhost:8080/hello，可以看到页面输出   Hello World

至此已完成目标，通过Maven构建了一个空白Spring Boot项目，再通过引入web模块实现了一个简单的请求处理。
