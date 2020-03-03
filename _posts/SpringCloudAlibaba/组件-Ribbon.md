---
title: 组件-Ribbon
date: 2020-02-04 00:26:50
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---




# Ribbon
Ribbon is a Inter Process Communication (remote procedure calls) library with built in software load balancers. The primary usage model involves REST calls with various serialization scheme support.

Ribbo是一个基于HTTP和TCP的客户端负载均衡器，当我们将Ribbon和注册中心(Eureka,Nacos)一起使用时，Ribbon会从注册中心去获取服务端列表，然后进行轮询访问以到达负载均衡的作用，客户端负载均衡中也需要心跳机制去维护服务端清单的有效性，当然这个过程需要配合服务注册中心一起完成。

<!-- more -->
# 负载均衡
**负载平衡**（Load balancing）是一种计算机技术，用来在多个计算机（计算机集群）、网络连接、CPU、磁碟驱动器或其他资源中分配负载，以达到最佳化资源使用、最大化吞吐率、最小化响应时间、同时避免过载的目的。一般负载均衡软件会部署在服务器，和客户端两部分。
1. 服务端：服务端软件负载均衡则主要是在服务器上安装一些具有负载均衡功能的软件来完成请求分发进 比较常见的有nginx...
2. 客户端：在客户端负载均衡中，所有的客户端节点都有一份自己要访问的服务端清单，这些清单统统都是从服务注册中心获取的。比较常见的有 ribbon...


# 使用Ribbon

1. **添加依赖**
spring-cloud-starter-alibaba-nacos-discovery 已经集成了 robbon。

2. **写注解**

```
    @Bean
    @LoadBalanced # 为RestTemplate 整合 Ribbon
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
```



3. **写配置**
   1. 代码配置：基于代码，更加灵活，缺点 父子上下文，线上修改得重新打包，发布

    ```
    @Configuration
    @RibbonClients(defaultConfiguration = RibbonConfiguration.class)
    public class AuthCenterRibbonConfiguration {
    }
    ```


   2. 属性配置：易上手，配置更加直观，线上秀嘎无需重新打包，发布优先级更高，缺点：极端场景下没有代码配置方式灵活。

    ```
    auth-center:
    ribbon:
        # 负载规则
        NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
    # 饥饿加载
    ribbon:
    eager-load:
        clients: '*'
        enabled: true
    ```



4. 实现

`UserDTO UserDTO = restTemplate.getForObject("http://auth-center/users/{userId}", UserDTO.class, userId);`

当RestTemplate 去请求的时候 会自动调用 Ribbon 将 auth-center 转换成用户中心地址，并且使用负载均衡算法返回一个实例给我们使用。

# Ribbon 组成

|  接口           | 作用      | 默认值|
|  ----           |  ----    | ----- |
| IClientConfig   | 读取配置  | DefaultClientConfigImpl     |
| IRule          | 负载均衡规则，选则实例 | ZoneAvoidanceRule     |
|IPing           |筛选ping不通的实例	|DummyPing|
|ServerList      |交给ribbon的实例列表|ConfigurationBasedServerList(ribbon)/NacosServerList(springcloudalibaba)|
|ServerListFilter|过滤不合规实例	|ZonePreferenceServerListFilter|
|ILoadBalancer| ribbon的入口   |ZoneAwareLoadBalancer|
|ServerListUpdater| 更新交给ribbon的List的策略 |PollingServerListUpdater

# 内置负债均衡算法
1. RoundRobinRule: 轮询


2. RandomRule: 随机


3. AvailabilityFilteringRule :会优先过滤由于多次访问故障而处于断路器跳闸状态的服务,还有并发的连接数量超过阀值的服务，然后对剩余的服务轮询


4. WeigthtedResponseTimeRule
根据平均响应时间计算所有服务的权重，响应时间越快权重越大被访问概率越高，刚开始统计信息不够使用轮询，之后切换


5. RetryRule
先按照轮询获取服务，如果失败则在指定的时间内重试，获取可用服务


6. BestAvailableRule
会过滤由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务


7. ZoneAvoidanceRule
默认规则，复合判断server所在区域的性能和server可用性选择服务器，没有Zone的环境下类似轮询


- 自己写Rule: 继承 AbstractLoadBalancerRule 实现choose ，继承 Balancer。