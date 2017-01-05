##What we will do:
In this short step:
- Format Add Todo Page
- Add Html5 Form Validations

## Useful Snippets
```     
	<fieldset class="form-group">
		<label>Description</label>
		<input name="desc" type="text" class="form-control" required="required"/>
	</fieldset>

```

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
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
        </dependency>

        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>3.3.6</version>
        </dependency>

        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>1.9.1</version>
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
import org.springframework.web.bind.annotation.SessionAttributes;

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
@SessionAttributes("name")
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
### /src/main/java/com/in28minutes/springboot/web/controller/TodoController.java
```
package com.in28minutes.springboot.web.controller;

import java.util.Date;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.SessionAttributes;

import com.in28minutes.springboot.web.service.TodoService;

@Controller
@SessionAttributes("name")
public class TodoController {

    @Autowired
    private TodoService service;

    @RequestMapping(value = "/list-todos", method = RequestMethod.GET)
    public String showTodoPage(ModelMap model) {
        String user = (String) model.get("name");
        model.addAttribute("todos", service.retrieveTodos(user));
        return "list-todos";
    }

    @RequestMapping(value = "/add-todo", method = RequestMethod.GET)
    public String showTodoPage() {
        return "todo";
    }

    @RequestMapping(value = "/add-todo", method = RequestMethod.POST)
    public String addTodo(ModelMap model, @RequestParam String desc) {
        service.addTodo((String) model.get("name"), desc, new Date(), false);
        model.clear();// to prevent request parameter "name" to be passed
        return "redirect:/list-todos";
    }

    @RequestMapping(value = "/delete-todo", method = RequestMethod.GET)
    public String deleteTodo(@RequestParam int id) {
        service.deleteTodo(id);

        return "redirect:/list-todos";
    }
}
```
### /src/main/java/com/in28minutes/springboot/web/model/Todo.java
```
package com.in28minutes.springboot.web.model;

import java.util.Date;

public class Todo {
    private int id;
    private String user;
    private String desc;
    private Date targetDate;
    private boolean isDone;

    public Todo(int id, String user, String desc, Date targetDate,
            boolean isDone) {
        super();
        this.id = id;
        this.user = user;
        this.desc = desc;
        this.targetDate = targetDate;
        this.isDone = isDone;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public Date getTargetDate() {
        return targetDate;
    }

    public void setTargetDate(Date targetDate) {
        this.targetDate = targetDate;
    }

    public boolean isDone() {
        return isDone;
    }

    public void setDone(boolean isDone) {
        this.isDone = isDone;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + id;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        Todo other = (Todo) obj;
        if (id != other.id) {
            return false;
        }
        return true;
    }

    @Override
    public String toString() {
        return String.format(
                "Todo [id=%s, user=%s, desc=%s, targetDate=%s, isDone=%s]", id,
                user, desc, targetDate, isDone);
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
### /src/main/java/com/in28minutes/springboot/web/service/TodoService.java
```
package com.in28minutes.springboot.web.service;

import java.util.ArrayList;
import java.util.Date;
import java.util.Iterator;
import java.util.List;

import org.springframework.stereotype.Service;

import com.in28minutes.springboot.web.model.Todo;

@Service
public class TodoService {
    private static List<Todo> todos = new ArrayList<Todo>();
    private static int todoCount = 3;

    static {
        todos.add(new Todo(1, "in28Minutes", "Learn Spring MVC", new Date(),
                false));
        todos.add(new Todo(2, "in28Minutes", "Learn Struts", new Date(), false));
        todos.add(new Todo(3, "in28Minutes", "Learn Hibernate", new Date(),
                false));
    }

    public List<Todo> retrieveTodos(String user) {
        List<Todo> filteredTodos = new ArrayList<Todo>();
        for (Todo todo : todos) {
            if (todo.getUser().equalsIgnoreCase(user)) {
                filteredTodos.add(todo);
            }
        }
        return filteredTodos;
    }

    public void addTodo(String name, String desc, Date targetDate,
            boolean isDone) {
        todos.add(new Todo(++todoCount, name, desc, targetDate, isDone));
    }

    public void deleteTodo(int id) {
        Iterator<Todo> iterator = todos.iterator();
        while (iterator.hasNext()) {
            Todo todo = iterator.next();
            if (todo.getId() == id) {
                iterator.remove();
            }
        }
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
logging.level.: INFO
```
### /src/main/webapp/WEB-INF/jsp/list-todos.jsp
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
<head>
<title>Todos for ${name}</title>
<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
    rel="stylesheet">
</head>
<body>
    <div class="container">
        <table class="table table-striped">
            <caption>Your Todos are</caption>

            <thead>
                <tr>
                    <th>Description</th>
                    <th>Date</th>
                    <th>Completed</th>
                    <th></th>
                </tr>
            </thead>

            <tbody>
                <c:forEach items="${todos}" var="todo">
                    <tr>
                        <td>${todo.desc}</td>
                        <td>${todo.targetDate}</td>
                        <td>${todo.done}</td>
                        <td>
                            <a type="button" class="btn btn-warning" 
                                href="/delete-todo?id=${todo.id}">Delete</a>
                        </td>
                    </tr>
                </c:forEach>
            </tbody>
        </table>
        <div>
            <a class="button" href="/add-todo">Add</a>
        </div>
    </div>

    <script src="webjars/jquery/1.9.1/jquery.min.js"></script>
    <script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
</body>
</html>
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
### /src/main/webapp/WEB-INF/jsp/todo.jsp
```
<html>
<head>
<title>Your Todo</title>
<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
    rel="stylesheet">
</head>
<body>

    <div class="container">
        <form action="/add-todo" method="post">
            <fieldset class="form-group">
                <label>Description</label>
                <input name="desc" type="text" class="form-control" required="required"/>
            </fieldset>
            <button type="submit" class="btn btn-success">Add</button>
        </form>
    </div>

    <script src="webjars/jquery/1.9.1/jquery.min.js"></script>
    <script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>

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
Welcome ${name}. You are now authenticated. <a href="/list-todos">Click here</a> to start maintaining your todo's.
</body>
</html>
```
