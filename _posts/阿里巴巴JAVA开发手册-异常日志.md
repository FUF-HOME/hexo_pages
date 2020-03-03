---
title: 阿里巴巴JAVA开发手册-异常日志
date: 2020-01-22 21:00:43
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - JAVA,Alibaba
blogexcerpt:
---

# 异常日志

# (一) 异常处理
- 1 可以通过预检查方式规避的 RuntimeException 异常不应该通过 catch 的方式处理，如：NullPointerException，IndexOutOfBoundsException 等等。
- 5.有 try 块放到了事务代码中，catch 异常后，如果需要回滚事务，一定要注意手动回滚事务。
- 7.不要在 finally 块中使用 return。
<!-- more -->
    > 说明：try 块中的 return 语句执行成功后，并不马上返回，而是继续执行 finally 块中的语句，如果此处存在 return 语句，则在此直接返回，无情丢弃掉 try 块中的返回点。
# （二）日志规约
- 1.应用中不可直接使用日志系统（Log4j、Logback）中的 API，而应依赖使用日志框架 SLF4J 中的 API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。
- 3.应用中的扩展日志（如打点、临时监控、访问日志等）命名方式：appName_logType_logName.log

    - logType:日志类型，如 stats / monitor / access 等；logName:日志描述。

    - 这种命名的好处：通过文件名就可知道日志文件属于什么应用，什么类型，什么目的，也有利于归类查找。
- 4.在日志输出时，字符串变量之间的拼接使用占位符的方式。

    - 说明：因为 String 字符串的拼接会使用 StringBuilder 的 append() 方式，有一定的性能损耗。使用占位符仅是替换动作，可以有效提升性能。

    - 正例：logger.debug("Processing trade with id: {} and symbol: {}", id, symbol);
- 5.对于 trace / debug / info 级别的日志输出，必须进行日志级别的开关判断。
- 6.避免重复打印日志，浪费磁盘空间，务必在 log4j.xml 中设置 additivity=false。
  - 正例：<logger name="com.fuf.rest.config" additivity="false">


# (三) 单元测试

# (四) 安全规约
- 2.用户敏感数据禁止直接展示，必须对展示数据进行脱敏。

    - 说明：中国大陆个人手机号码显示为:1370969，隐藏中间 4 位，防止隐私泄露。

- 3.用户输入的 SQL 参数严格使用参数绑定或者 METADATA 字段值限定，防止 SQL 注入，禁止字符串拼接 SQL 访问数据库。

- 4.用户请求传入的任何参数必须做有效性验证。

