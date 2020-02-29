
# Nacos



# 什么是Nacos？

Nacos是一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。  

Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。  
Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。
Nacos官方地址：https://nacos.io/zh-cn/index.html 

# Nacos 的关键特性
1. 服务发现和服务健康监测
    1. Nacos 支持基于 DNS 和基于 RPC 的服务发现。服务提供者使用 原生SDK、OpenAPI、或一个独立的Agent TODO注册 Service 后，服务消费者可以使用DNS TODO 或HTTP&API查找和发现服务
    2. Nacos 提供对服务的实时的健康检查，阻止向不健康的主机或服务实例发送请求。Nacos 支持传输层 (PING 或 TCP)和应用层 (如 HTTP、MySQL、用户自定义）的健康检查。 对于复杂的云环境和网络拓扑环境中（如 VPC、边缘网络等）服务的健康检查，Nacos 提供了 agent 上报模式和服务端主动检测2种健康检查模式。Nacos 还提供了统一的健康检查仪表盘，帮助您根据健康状态管理服务的可用性及流量。

2. 动态配置服务
动态配置服务让您能够以中心化，外部化和动态化的方式管理所有环境的配置。动态配置消除了配置变更时重新部署应用和服务的需要。配置中心化管理让实现无状态服务更简单，也让按需弹性扩展服务更容易。

3. 动态 DNS 服务
动态 DNS 服务支持权重路由，让您更容易地实现中间层负载均衡、更灵活的路由策略、流量控制以及数据中心内网的简单DNS解析服务。动态DNS服务还能让您更容易地实现以 DNS 协议为基础的服务发现，以帮助您消除耦合到厂商私有服务发现 API 上的风险。

Nacos 提供了一些简单的 DNS APIs TODO 帮助您管理服务的关联域名和可用的 IP:PORT 列表.

# 安装单机版 Nacos
Nacos 下载地址：https://github.com/alibaba/nacos/releases  
截止目前最新版本为：1.1.4
[!Nacos](../../img/Nacos.png)

下载之后解压进入到bin目录执行启动命令：sh startup.sh -m standalone  

命令参数中-m表示模式mode，standalone表示启动的是单机版模式，windows用户可以使用该命令启动Nacos： cmd startup.cmd -m standalone  
执行启动命令之后可以查看到启动日志：tail -fn 200 /home/server/nacos/logs/start.out


Nacos是一个用Java语言编写的web项目，Tomcat默认端口是8848，访问8848端口可以打开Nacos管理台。  默认账号和密码都是 nacos   
下载Nacos 并启动 Nacos server。之后就可以编写相关代码实践了。下面通过官方的一个demo，来熟悉 Nacos 和 Spring Cloud 的配合。

# 使用Nacos注册中心功能



1. 添加依赖：

    ```
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```
  > 注意一：如果你想要的是Spring Cloud Alibaba，记得使用 dependencyManagement 管理版本。

  > 注意二：版本兼容，可以在[官网](https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E) 查看版本管理规范。  


2. 在 bootstrap.properties 中配置 Nacos server 的地址和应用名  
    ```
    server.port = 8000
    spring.cloud.nacos.discover.server-addr = 127.0.0.1:8848
    # Nacos 服务器地址。
    spring.application.name = nacos-provider
    ```


3. 编写入口类NacosProvider ，并添加一个EchoController 提供HTTP接口服务。

```
@SpringBootApplication
@EnableDiscoveryClient
public class NacosProvider {

    public static void main(String[] args) {
        SpringApplication.run(NacosProvider.class,args);
    }

    @RestController
    class EchoController {

        @GetMapping("/echo/{string}")
        public String echo(@PathVariable String string) {
            return "Hello Nacos Discovery " + string;
        }
    }

}

```
4. 启动nacos-provider。可以在启动日志看到register。
在 nacos 服务器 127.0.0.1:8848 里面的服务列表，发现
nacos-provider 已经注册成功。  
[!nacos](../../img/nacos-2.png)

# nacos-consumer

1. nacos-consumer 模块项目的依赖加入alibaba-nacos-discovery依赖。
2. 编写入口 NacosConsumer ，并添加一个TestController提供HTTP接口服务，通过RestTemplate远程调用NacosProvider。
```
@SpringBootApplication
@EnableDiscoveryClient
public class NacosConsumer {

    @LoadBalanced
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }

    public static void main(String[] args) {
        SpringApplication.run(NacosConsumer.class,args);
    }

    @RestController
    public class TestController {

        private final RestTemplate restTemplate;

        @Autowired
        public TestController(RestTemplate restTemplate) {this.restTemplate = restTemplate;}

        @GetMapping("/echo/{str}")
        public String echo(@PathVariable String str) {
            return restTemplate.getForObject("http://nacos-provider/echo/" + str, String.class);
        }

    }

}
```
3. 在bootstrap.yml文件配置nacos配置中心相关参数。
```
server.port =8001
spring.application.name = nacos-cinsumer
spring.cloud.nacos.discovery.server-addr:127.0.0.1:8848 # Nacos注册中心地址



4.调用接口服务
启动 nacos-consumer 项目，在地址栏访问。

> nacos 默认使用了 ribbon 做客户端负载均衡。