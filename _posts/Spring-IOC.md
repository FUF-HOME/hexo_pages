## 3.2 ApplicationContext
ApplicationContext 容器建立BeanFactory之上，拥有BeanFactory的所有功能，但在实现上会有所差别。我认为差别主要体现在两个方面：1.bean的生成方式；2.扩展了BeanFactory的功能，提供了更多企业级功能的支持。
1. bean 的加载方式，Bean BeanFactory提供BeanReader来从配置文件中读取bean配置。相应的ApplicationContext也提供几个读取配置文件的方式：
   - FileSystemXmlApplicationContext：从XML 文件中，加载已被定义的bean，需要你提供给构造器 XML 文件的完整路径。
   - ClassPathXmlApplicationContext：从 XML 文件中加载已被定义的bean，但是不需要提供文件路径，只要正确配置CLASSPATH 环境变量即可，容器会从CLASSPATH中搜索bean 配置文件。
   - AnnotationConfigApplicationContext
   - ConfigurableWebApplicationContext
2. ApplicationContext 会在启动阶段完成所有的初始化，并不会等到 getBean()才执行。
3. ApplicationContext：还额外还增加了三个功能：ApplicationEventPublisher,ResourceLoader,MessageResource

### ApplicationEventPublisher

### ResourceLoader
ResourceLoader并不能将其看成是Spring独有的功能，spring Ioc只是借助于ResourceLoader来实现资源加载。也提供了各种各样的资源加载方式： 
 - DefaultResourceLoader 先检查资源路径是否以classpath:前缀打头，如果是，则尝试构造ClassPathResource类 型资源并返回。否则， 尝试通过URL，根据资源路径来定位资源

### MessageSource

https://juejin.im/post/593386ca2f301e00584f8036