# FreeMaker 介绍

FreeMarker是一款免费的Java模板引擎，是一种基于模板和数据生成文本（HMLT、电子邮件、配置文件、源代码等）的工具，它不是面向最终用户的，而是一款程序员使用的组件。
FreeMarker最初设计是用来在MVC模式的Web开发中生成HTML页面的，所以没有绑定Servlet或任意Web相关的东西上，所以它可以运行在非Web应用环境中。

# 发展史
FreeMarker第一版在1999年未就发布了，2002年初使用JavaCC（Java Compiler Compiler是一个用Java开发的语法分析生成器）重写了FreeMarker的核心代码，2015年FreeMarker代码迁移到了Apache下。

# 工作原理
FreeMarker模板存储在服务器上，当有用户访问的时候，FreeMarker会查询出相应的数据，替换模板中的标签，生成最终的HTML返回给用户，如下图

二、FreeMarker基础使用

对于SpringBoot集成其他组件，一般直接三板斧弄起
1. 添加引用
2. 添加注解
3. 写配置

按照三板斧，首先在 项目的根目录下pom.xml 中添加FreeMarke 的依赖
```
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
            <version>{lastVersion}</version>
        </dependency>

```

