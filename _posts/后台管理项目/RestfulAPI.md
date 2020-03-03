---
title: RestfulAPI
date: 2019-03-04 00:18:03
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---

 


# RESTful
REST（Representational State Transfer）,中文翻译叫"表述性状态转移".REST指的是一组架构约束条件和原则。"如果一个架构符合REST的约束条件和原则，我们就称它为RESTful架构，REST其实并没有创造新的技术、组件或服务，在我的理解中，它更应该是一种理念、一种思想，利用Web的现有特征和能力，更好地诠释和体现现有Web标准中的一些准则和约束。  
RESTful API是面向资源的架构，因此其URL就应该是一个资源.
举个例子~~
<!-- more -->

```
http://xxx.com/article/delect/1
```
按照链接的意思来看，这是一个删除一篇文章的api地址。那这RESTful API 吗？不是根据上面的解释，地址链接应该只是指向一个资源，而不能有任何实际性的操作(增删改查)，所有的具体操作应该由HTTP动词表示。正确的 删除链接应该是
```
[DELETE] http://xxx.com/article/1
```
## 良好RESTful API的设计原则

### 基本原则一：URI
### 基本原则二：HTTP动词
对于资源的具体操作类型，由HTTP动词表示，常用的HTTP动词有下面五个：
1. GET:从服务取出资源。
2. POST：在服务器新建一个资源。
3. PUT:在服务器更新资源（客户端提供改变后的完整资源）。
4. PATCH:在服务器更新资源（客户端提供改变的属性）。
5. DELETE:从服务器删除资源。
#### 还有两个不常用的HTTP动词。
```
HEAD：获取资源的元数据

OPTIONS：获取信息，关于资源的哪些属性四客户端可以改变的。

```


**例子**
```
[POST] http://xxx.com/articles //新增

[GET] http://xxx.com/articles/1 // 获取资源

[PUT] http://xxx.com/articles/1 // 修改

[DELETE] http://xxx.com/article/1 //删除
```
### 基本原则三：状态码（Status Codes）
常见状态码(状态码可自行设计，只需开发者约定好规范即可)：

200:SUCCESS,请求成功;

401:Unauthorized,无权限;

403:Forbidden,禁止访问;

410:Gone,无此资源;

500:INTERNAL SERVER ERROR,服务器发生错误。

### 基本原则 四
规范的API应该包含版本信息，在RESTful API中，最简单的包含版本的方法是将版本信息放到url中，如：
```
[GET] http://xxx.com/v1/article/1 
```
另一种做法是，使用HTTP header中的accept来传递版本信息。

## 其他
1. API 的身份认证应该使用OAuth 2.0框架。

2. 服务器返回的数据格式，应该尽量使用JSON，避免使用XML。

## 安全问题
### 遗漏了对资源从属关系的检查
一个典型的RESTful的URL会用资源名加上资源的id编号来标识其唯一性，就像这样:/users/{userid}，例如：/users/1

一般而言用户只能查看自己的用户信息，而不允许查看其它用户的信息。在这种情况下，攻击者很可能会尝试把这个URL里面的USER ID从1修改为其他数值，以期望应用返回指定用户的信息。  
不过由于这个安全风险太显而易见，绝大多数应用都会对当前请求者的身份进行校验，看其是否是编号为1的用户，校验成功才返回URL中指定的用户信息，否则会拒绝当前请求。

### HTTP响应中缺失必要的 Security Headers




### 不经意间泄露的业务信息
以查看用户信息的RESTful URL为例：/users/100。由于用户ID是一个按序递增的数字，因此攻击者既可以通过ID知道目前应用中的用户规模，也可以分别在月初和月末的时候注册一个用户，并对比两个用户的ID即可知道当前这个月有多少新增用户。同理，如果订单号也是按序自增的数字，攻击者可以了解到一定时间范围内的订单量。
**解决办法：** 使用一个具有随机性，唯一性，不可预测性的值作为ID，最常见的实例是UUID


### API缺乏速率限制的保护