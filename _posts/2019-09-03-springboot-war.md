---
layout: inner
position: right
title: 'Spring Boot打war启动(EasyReport)'
date: 2019-09-02 11:15:00
categories: development
tags: SpringBoot War Thymeleaf
featured_image: '/img/posts/01_bloc-jams-angular-1130x864-2x.png'
project_link: ''
button_icon: 'github'
button_text: 'View'
lead_text: '为了方便部署和统一管理，同时节约服务器端口资源，故将jar方式更改为采取war方式打包'
---
## Spring Boot打war启动(EasyReport)

前言：为了方便部署和统一管理，同时节约服务器端口资源，故将jar方式更改为采取war方式打包

### POM.xml

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>${spring-boot.version}</version>
            <!-- 这里指定打包的时候不再需要tomcat相关的包 -->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
          <groupId>org.apache.tomcat.embed</groupId>
          <artifactId>tomcat-embed-core</artifactId>
          <scope>provided</scope>
        </dependency>
        <!-- servlet api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>${javax.servlet.api.version}</version>
            <scope>provided</scope>
        </dependency>
```

### 启动类

```java
@SpringBootApplication
public class WebApplication extends SpringBootServletInitializer {
    public static void main(final String[] args) {
        SpringApplication.run(WebApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(WebApplication.class);
    }
}
```

### 遇到的问题

1. 碰到一个外观模式注入的Service，使用`@Resource` 注入一直是失败的，用jar方式没问题，后改用`@Autowired`注入启动成功

2. 由于之前采取jar方式启动，URL只到端口层，故项目中请求正常，换成war后，由于带上了工程名称，ajax请求默认不包含工程名，导致部分请求404

传统的jsp页面js获取项目路径的写法：
```javascript
var contextPath = '${pageContext.request.getContextPath()}';
```
springboot thymeleaf js获取项目路径：
```javascript
<script th:inline="javascript">
        /*<![CDATA[*/
        var context = /*[[@{/}]]*/;
        /*]]>*/
</script>
```
