##What You Will Learn during this Step:
- Add validation for userid and password
 - Hard coded validation!!

## Useful Snippets and References
First Snippet
```
package com.in28minutes.springboot.web.service;

import org.springframework.stereotype.Component;

@Component
public class LoginService {
    public boolean validateUser(String user, String password) {
        return user.equalsIgnoreCase("in28Minutes") && password.equals("dummy");
    }
}

```
Second Snippet
```
    @Autowired
    private LoginService service;

    @RequestMapping(value = "/login", method = RequestMethod.POST)
    public String handleLogin(ModelMap model, @RequestParam String name,
            @RequestParam String password) {

        boolean isValidUser = service.validateUser(name, password);

        if (isValidUser) {
            model.put("name", name);
            return "welcome";
        } else {
            model.put("errorMessage", "Invalid Credentials!!");
            return "login";
        }
    }
```

## Exercises

## Files List
### /pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.in28minutes.springboot.web</groupId>
    <artifactId>Spring-Boot-First-Web-Application</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

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

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;

import com.in28minutes.springboot.web.service.LoginService;

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

    @Autowired
    private LoginService service;

    @RequestMapping(value = "/login", method = RequestMethod.GET)
    public String showLoginPage(ModelMap model) {
        return "login";
    }

    @RequestMapping(value = "/login", method = RequestMethod.POST)
    public String handleLogin(ModelMap model, @RequestParam String name,
            @RequestParam String password) {

        boolean isValidUser = service.validateUser(name, password);

        if (isValidUser) {
            model.put("name", name);
            return "welcome";
        } else {
            model.put("errorMessage", "Invalid Credentials!!");
            return "login";
        }
    }

}
```
### /src/main/java/com/in28minutes/springboot/web/service/LoginService.java
```
package com.in28minutes.springboot.web.service;

import org.springframework.stereotype.Component;

@Component
public class LoginService {
    public boolean validateUser(String user, String password) {
        return user.equalsIgnoreCase("in28Minutes") && password.equals("dummy");
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
<form action="/login" method="POST">
		<p><font color="red">${errorMessage}</font></p>
        Name : <input name="name" type="text" /> Password : <input name="password" type="password" /> <input type="submit" />
</form>
</body>
</html>
```
### /src/main/webapp/WEB-INF/jsp/welcome.jsp
```
<html>
<head>
<title>Yahoo!!</title>
</head>
<body>
Welcome!!! My name is ${name}
</body>
</html>
```
