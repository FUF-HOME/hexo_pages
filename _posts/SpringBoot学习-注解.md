# 注解

SpringBoot帮助开发者快速搭建Spring框架SpringBoot帮助开发者快速启动一个Web容器；

SpringBoot继承了原有Spring框架的优秀基因；

SpringBoot简化了使用Spring的过程。


Spring由于其繁琐的配置，一度被人认为“配置地狱”，各种XML、Annotation配置，让人眼花缭乱，而且如果出错了也很难找出原因。在此背景下， Spring Boot应用而生。它使用习惯优于配置的理念，让项目快速运行起来，使用Spring Boot可以很轻松的创建一个独立运行(运行jar，内嵌servlet容器)、准生产级别的项目，可以大大减少我们的Spring配置。而配置中需要用到大量的注解。

# 主要的注解
## 1、@SpringBootApplication

这是 Spring Boot 最最最核心的注解，用在 Spring Boot 主类上，标识这是一个 Spring Boot 应用，用来开启 Spring Boot 的各项能力。  
其实这个注解就是 @SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan 这三个注解的组合，也可以用这三个注解来代替 @SpringBootApplication 注解。
##  2、@EnableAutoConfiguration

允许 Spring Boot 自动配置注解，开启这个注解之后，Spring Boot 就能根据当前类路径下的包或者类来配置 Spring Bean。

如：当前类路径下有 Mybatis 这个 JAR 包，MybatisAutoConfiguration 注解就能根据相关参数来配置 Mybatis 的各个 Spring Bean。
## 3、@Configuration

这是 Spring 3.0 添加的一个注解，用来代替 applicationContext.xml 配置文件，所有这个配置文件里面能做到的事情都可以通过这个注解所在类来进行注册。
## @Bean.
Spring帮助我们管理Bean分为两个部分，一个是注册Bean，一个装配Bean。  

完成这两个动作有三种方式，一种是使用自动配置的方式、一种是使用JavaConfig的方式，一种就是使用XML配置的方式。

@Compent 作用就相当于 XML配置
组装bean
```
@Component
public class Student{
    privae String name = "fuf";

    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    } 


}

```
@Component就是跟<bean>一样，可以托管到Spring容器进行管理。

@Service, @Controller , @Repository = {@Component + 一些特定的功能}。这个就意味着这些注解在部分功能上是一样的。
1. @Component	最普通的组件，可以被注入到spring容器进行管理
2. @Repository	作用于持久层
3. @Service	作用于业务逻辑层
4. @Controller	作用于表现层（spring-mvc的注解）

@Bean 需要在配置类中使用，即类上需要加上@Configuration注解
```

@Configuration
public class WebSocketConfig{
    public Student student(){
        return new Student();
    }
}

```
两者都可以通过@Autowired和@Resource装配

@Autowired
Student student;

Bean注解的作用之一就是能够管理第三方jar包内的类到容器中。 现在我们引入一个第三方的jar包，这其中的某个类，StringUtil需要注入到我们的IndexService类中，因为我们没有源码，不能再StringUtil中增加@Component或者@Service注解。这时候我们可以通过使用@Bean的方式，把这个类交到Spring容器进行管理，最终就能够被注入到IndexService实例中。
@Configuration等价于xml中的beans，@Bean注解方法等价于xml中的bean


# @EnableAutoConfiguration
主要是更好的使用start-POMS。建议将@Configuration类加@EnableAutoConfiguration注解，可以更好的决定使用哪些jar，也可以排除不需要的配置。

# @Configuration
spring-boot提倡使用java的配置。尽管可以使用一个xml源调用SpringApplication.run()。一般定义main入口方法的类也可以使用@Configuration加自己需要的配置。建议将各自的配置放在不同的类中，可以借助@Import来引入其他的配置类，也可以使用@ComponentScan注解引入所有的spring组件（包括@Configuration类）。如果你必须基于xml的配置，仍然建议将@Configuration的类加一个@ImportResource注解加载xml配置文件。@Configuration等价于xml中的beans，@Bean注解方法等价于xml中的bean。（config组件）

# @ComponentScan
用@ComponentScan可以搜索使用Spring框架定义的beans。一般在目录的根目录下定义一个@ComponentScan类扫描所有的应用程序组件。（比如@Component @Service @Repository @Controller等）将会被自动注册为Spring Beans。

# @SpringBootApplication
相信很多开发者都是使用@Configuration @EnableAutoConfiguration @ComponentScan去注解main类，其实与使用@SprngBootApplication一个作用，但还是建议使用这三个注解，看上去更让人理解。

# @ConfigurationProperties
该注解可以将一些属性值properties绑定到@ConfigurationProperties bean中，例如:

@Configuration @ConfigurationProperties(locations = "classpath:some.properties", prefix = "test", ignoreUnknownFields = false)

public class TestConfiguration {

@NotBlank private String name;

...

}

然后在test.properties=testName

这样TestConfiguration中就可以读取属性。那如何使用这些配置的bean呢？

@Configuration @EnableConfigurationProperties(TestConfiguration.class)

public class TestGetConfigurationProperties {

@Autowired private TestConfiguration testConfiguration;

}

这样就注入的bean就带有@ConfigurationProperties中的属性了，使用起来还是方便，不过不建议太多的hardCode去设置太多的属性。

@Component @Bean
@Component等价于@Controller @Service @Repository。@Component中使用字段或者方法不会使用CGLIB增强，调用的都是原始对象上的方法字段。@Bean注解方法上，表明方法返回的一个需要注入的bean，@Configuration注解类上，相当于是xml中的beans，@Bean相当于是xml中的bean。@Bean是@Configuration的子元素。

# @Profiles
Spring Profiles提供了一个隔离应用配置的方式，任何@Component或者@Configuration都可以被@Profiles("dev")注解，然后根据当前的开发环境指定一个spring.profiles.active纯文件来指定哪个环境变量生效。

# @PostContruct
servlet生命周期：web容器加载sevlet -> servlet构造函数 -> @PostContruct注解方法 -> init() -> Service -> destory() -> @PreDestory()注解方法 -> 结束。

@PostContruct使用注意:

1.注解方法不能有任何参数

2.注解方法返回值必须是null

3.注解方法不得抛出已检查异常

4.注解方法需要是非静态方法

5.方法只会被执行一次

# @Async
该注解方法可以允许方法被异步调用，调用者立即返回，而被调用的方法交给Spring的TaskExecutor完成。返回值是一个Future<>，通过方法内的return new AsyncResult<>()。