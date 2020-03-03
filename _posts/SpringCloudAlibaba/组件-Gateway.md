---
title: 组件-Gateway
date: 2020-01-04 00:26:21
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---


# Spring Cloud Gateway


是 Spring Cloud的网关(第二代)，未来会取代第一代。

## 核心概念
## 1，路由
## 2，谓词：是路由的判断条件
## 3，过滤器

## 架构
## 编写 GateWay
<!-- more -->

## 路由谓词工厂

## 自定义谓词工厂

## 内置过滤器工厂

## 自定义过滤器工厂
###  过滤器生命周期
### 
## 全局过滤器
## SpringCloud Gateway 整合sentinel

## 排错，调试技术总结 

### 第一： Actuator 监控端点

### 第二：  日志
### 第三 Wiretap 
## 过滤器执行顺序

## SpringCloud Gateway 限流
SpringCloud Gateway  内置

# 总结
1. 注册 注册到Nacos 自动发现微服务
2. 集成 Ribbon 
3. 容错(默认 Hystrix，也可以Sentinel)