---
title: SpringBoot学习系列文章四
date: 2020-02-17 16:25:02
author: fuf
notebook: blog
evernote-version: 0
source: 原创
thumbnail: 
tags:
    - SpringBoot
blogexcerpt:
---


# Spring Boot中使用Swagger2构建强大的RESTful API文档
由于Spring Boot能够快速开发、便捷部署等特性，相信有很大一部分Spring Boot的用户会用来构建RESTful API。而我们构建RESTful API的目的通常都是由于多终端的原因，这些终端会共用很多底层业务逻辑，因此我们会抽象出这样一层来同时服务于多个移动端或者Web前端。
<!-- more -->
这样一来，我们的RESTful API就有可能要面对多个开发人员或多个开发团队：IOS开发、Android开发或是Web开发等。为了减少与其他团队平时开发期间的频繁沟通成本，传统做法我们会创建一份RESTful API文档来记录所有接口细节，然而这样的做法有以下几个问题：

- 由于接口众多，并且细节复杂（需要考虑不同的HTTP请求类型、HTTP头部信息、HTTP请求内容等），高质量地创建这份文档本身就是件非常吃力的事，下游的抱怨声不绝于耳。
- 随着时间推移，不断修改接口实现的时候都必须同步修改接口文档，而文档与代码又处于两个不同的媒介，除非有严格的管理机制，不然很容易导致不一致现象。


为了解决上面这样的问题，本文将介绍RESTful API的重磅好伙伴Swagger2，它可以轻松的整合到Spring Boot中，并与Spring MVC程序配合组织出强大RESTful API文档。它既可以减少我们创建文档的工作量，同时说明内容又整合入实现代码中，让维护文档和修改代码整合为一体，可以让我们在修改代码逻辑的同时方便的修改文档说明。另外Swagger2也提供了强大的页面测试功能来调试每个RESTful API。具体效果如下图所示：

下面来具体介绍，如果在Spring Boot中使用Swagger2。首先，我们需要一个Spring Boot实现的RESTful API工程，若您没有做过这类内容，建议先阅读
Spring Boot构建一个较为复杂的RESTful APIs和单元测试。
# 创建一个 SpringBoot 项目
将这个项目命名为 restService。 Idea 会自动为你自动构建项目结构。如果使用其他IDE，可以看本系列的第一篇。

# 添加Swagger2依赖

在项目的根目录下面的pom.xml中加入Swagger的依赖
```

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>${swagger2.version}</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>${swagger2.version}</version>
</dependency>

```
# 创建Swagger2配置类
在RestServiceApplication (Idea 项目自动生成的启动类，在根项目)，的同级目录创建Swagger2 的配置类Swagger2
```
@Configuration
@EnableSwagger2
public class Swagger2 {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.cto.edu"))  # 扫描基础包地址
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("德鲁伊的成长之路RESTful APIs")
                .description("官网  http://123.207.108.233/ )")
                .termsOfServiceUrl("http://localhost:80/")
                .contact("ctoDev")
                .version("1.0")
                .build();
    }
}

```

# 添加文档内容
通过@ApiOperation注解来给API增加说明、通过@ApiImplicitParams、@ApiImplicitParam注解来给参数增加说明
```
@RestController
@RequestMapping("/api/blog/topic")
public class BlogTopicController {

    private static final Logger LOG = LoggerFactory.getLogger(BlogTopicController.class);

    @Resource
    private BlogTopicFacade blogTopicFacade;

    @ApiOperation(value = "分页获取主题列表", notes = "根据类别获取类别下的主题列表")
    @ApiImplicitParam(name = "obj", value = "{\"categoryIds\":[],\"number\":0,\"size\":1}", required = true, dataType = "JSONObject")
    @SuppressWarnings("unchecked")
    @PostMapping(value = "/findForPage")
    public ViewerResult findForPage(@RequestBody JSONObject obj) {

        ViewerResult result = new ViewerResult();
        try {
            List<String> categoryIds = (List<String>) obj.get("categoryIds");
            int number = obj.getInteger("number");
            int size = obj.getInteger("size");
            Pageable page = PageRequest.of(number, size);
            Searchable searchable = Searchable.newSearchable();
            searchable.setPage(page);

            if (categoryIds == null || categoryIds.isEmpty()) {
                Page<BlogTopic> topicPage = blogTopicFacade.listPage(searchable);
                result.setData(topicPage);
            } else {
                Page<BlogTopic> topicList = blogTopicFacade.findTopicByCategoryIds(categoryIds, page);
                result.setData(topicList);
            }

            result.setSuccess(true);
        } catch (Exception e) {
            result.setSuccess(false);
            result.setErrMessage("查找主题失败");
            LOG.error(e.getMessage(), e);
        }
        return result;
    }

    @ApiOperation(value = "获取主题信息", notes = "根据Id获取主题信息")
    @ApiImplicitParam(name = "obj", value = "{\"id\":\"1\"}", required = true, dataType = "JSONObject")
    @PostMapping(value = "/findById")
    public ViewerResult findById(@RequestBody JSONObject obj) {
        ViewerResult result = new ViewerResult();
        BlogTopic topic = null;
        try {
            String id = obj.getString("id");
            topic = blogTopicFacade.getById(id);
            result.setSuccess(true);
            result.setData(topic);
        } catch (Exception e) {
            result.setSuccess(false);
            result.setErrMessage(e.getMessage());
            LOG.error(e.getMessage(), e);
        }
        return result;
    }

}
```
# 访问
启动Spring Boot程序，访问：http://localhost:8081/swagger-ui.html



