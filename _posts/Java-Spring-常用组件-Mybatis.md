---
title: Java-Spring-常用组件-Mybatis
date: 2019-03-03 21:34:28
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---



# Mybatis
Mybatis是一个半**ORM**（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。程序员直接编写原生态sql，可以严格控制sql执行性能，灵活度高。   
MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
3.通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。（从执行sql到返回result的过程）。
<!-- more -->

## 使用Mybatis





## 操作
1. 模糊查询like语句该怎么写?

```
<selectlike>

select * from bar  where  name like #{value};

</selectlike>
```

针对MySQL数据库的语句，采用concat（）函数，它可以将多个字符串连接成一个字符
```
<selectlike>

select name from bar where   concat('%',#{productName},'%')

</selectlike>
```

适用于所有数据库的则采用MyBatis的bind元素
```
<select id="selectByLike">
    <bind name="user_name" value="'%' + _name + '%'"/>
    select * from table where name like #{user_name}
</select>
```

## 分页
1. 逻辑分页：即虽然看起来实现了分页的功能，但实际上是将查询的所有结果放置在内存中，每次都从内存获取。这种情况适用于数据量较少的情况
使用RowBounds

2. 物理分页：这种分页方法从底层上就是每次只查询对应条目数量的数据，从而实现了真正意义上的分页。

在UserInfoMapper.xml文件中增加对应查询语句，如下：
```
<select id="selectUserInfoByMap" parameterType="Map" resultMap="UserInfoResult">
		select * from userinfo
		<if test="start!=null and pagesize!=null">
			limit #{start},#{pagesize}
		</if>
</select>

```