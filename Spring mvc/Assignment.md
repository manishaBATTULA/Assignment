# Q1
Design and develop a Spring MVC web application as follows: Create index.jsp page having one hyperlink. When user dicks on that hyperlink, it should call HelloWorldController. Design HelloWorldController class that returns a view helloWorld.jsp Design a hello World jsp page that displays "Hello World" message.
            
HelloController.java
```java
          package hellocontroller;
          import org.springframework.stereotype.Controller;
          import org.springframework.ui.Model;
          import org.springframework.web.bind.annotation.RequestMapping;
          import org.springframework.web.bind.annotation.RequestParam;
           public class HelloController {
	  @Controller
	  public class HelloWorldController 
	  {
 
	  @RequestMapping("/hello")
	  public String hello(
	        @RequestParam(value = "name", required = false, defaultValue = "World") String name,
	        Model model)
		{
	    model.addAttribute("name", name);
	    return "helloworld";
	   }
	   }
	   }


```
HelloPage.jsp
```
<html>
 <body> <h1>First Spring MVC Application Demo</h1>
<h2>${Welcome Message}</h2>   
</body>
</html>
```
spring-dispatcher-servlet.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
   xmlns:context = "http://www.springframework.org/schema/context"
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation = "http://www.springframework.org/schema/beans     
   http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">  

   <!-- http://www.springframework.org/schema/context 
   http://www.springframework.org/schema/context/spring-context-5.3.8.xsd -->

   <bean id="HandlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />

   <bean name="/welcome.html" class="com.hellocontroller.HelloController" />
 
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name = "prefix">
            <value>/WEB-INF/</value>
        </property>
          <property name = "suffix">
              <value>.jsp</value>
          </property>
    </bean> 
</beans>
```
web.xml
```
<display-name>Hellowebpage</display-name>
   <servlet>
    <servlet-name>spring-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
 <servlet-mapping>
    <servlet-name>Spring-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
</web-app>
```
