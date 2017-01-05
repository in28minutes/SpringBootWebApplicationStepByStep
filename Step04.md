##What You Will Learn during this Step:
- Your First JSP
 - There is a bit of setup before we get there!
- Introduction to View Resolver

## Useful Snippets and References
First Snippet - /src/main/webapp/WEB-INF/jsp/login.jsp
```
<html>
<head>
<title>Yahoo!!</title>
</head>
<body>
My First JSP!!!
</body>
</html>
```

Second Snippet - /src/main/resources/application.properties
```
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp
logging.level.: DEBUG
```

Third Snippet : To enable jsp support in embedded tomcat server!
```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>provided</scope>
        </dependency>

```
Second Snippet
```
```

## Exercises
- Create a new jsp and a new controller method to redirect to it!
- Play around!

## Files List
### /pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.in28minutes.springboot.web</groupId>
    <artifactId>Spring-Boot-First-Web-Application</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>war</packaging>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.0.RELEASE</version>
    </parent>

    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>provided</scope>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>


    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```
### /src/main/java/com/in28minutes/springboot/web/controller/LoginController.java
```
package com.in28minutes.springboot.web.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/*
 * Browser sends Http Request to Web Server
 *
 * Code in Web Server => Input:HttpRequest, Output: HttpResponse
 * JEE with Servlets
 *
 * Web Server responds with Http Response
 */

@Controller
public class LoginController {

    @RequestMapping(value = "/login")
    public String sayHello() {
        return "login";
    }

}
```
### /src/main/java/com/in28minutes/springboot/web/WebApplication.java
```
package com.in28minutes.springboot.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class WebApplication {

    public static void main(String[] args) {
        ApplicationContext ctx = SpringApplication.run(WebApplication.class, args);
    }
}
```
### /src/main/resources/application.properties
```
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp
logging.level.: DEBUG
```
### /src/main/webapp/WEB-INF/jsp/login.jsp
```
<html>
<head>
<title>Yahoo!!</title>
</head>
<body>
My First JSP!!!
</body>
</html>
```
