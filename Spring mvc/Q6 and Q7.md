# Q6
Registration.jsp 
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
   pageEncoding="UTF-8"%>
<%@taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Spring MVC Form Validation</title>
      <style>
         .error {
         color: red
         }
      </style>
   </head>
   <body>
   		<form action = "/mvc/processForm" method = "post">
      <form:form action="processForm" modelAttribute="reg">
         <div align="center">
            <h2>Register Here</h2>
            <table>
               <tr>
                  <td>UserName</td>
                  <td>
                  	 <form:errors path = "register.*/" />
                     <input type="text" name="username"/>
                  </td>
                  <td>
                  	  
                     <form:errors path="username" cssClass="error" />
                  </td>
               </tr>
               <tr>
                  <td>Password</td>
                  <td>
                     <form:errors path = "register.*/" />
                     <input type="text" name="password"/>
                  </td>
                  <td>
                     <form:errors path="password" cssClass="error" />
                  </td>
               </tr>
               <tr>
                  <td>Email</td>
                  <td>
                     <form:errors path = "register.*/" />
                     <input type="text" name="email"/>
                  </td>
                  <td>
                     <form:errors path="email" cssClass="error" />
                  </td>
               </tr>
               <tr>
                  <td>Contact</td>
                  <td>
                     <form:errors path = "register.*/" />
                     <input type="text" name="contact"/>
                  </td>
                  <td>
                     <form:errors path="contact" cssClass="error" />
                  </td>
               </tr>
               <tr>
                  <td></td>
                  <td><input type="submit" value="Submit" /></td>
               </tr>
            </table>
         </div>
      </form:form>
   </body>
</html>
```
RegistrationBean.java
```java
package com.validation;

import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Pattern;
import javax.validation.constraints.Size;

public class RegistrationBean {

  @NotNull
  @Size(min = 8, max = 20, message = "You can't leave this empty.")
  @Pattern(regexp = "^[A-Za-z0-9]", message = "Invalid username")
  private String username;

  @NotNull
  @Size(min = 8, max = 20, message = "You can't leave this empty.")
  @Pattern(regexp = "^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,6}$", message = "Invalid password")
  private String password;

  @NotNull
  @Pattern(regexp = "^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,6}$", message = "Invalid email")
  private String email;

  @NotNull
  @Min(18)
  @Pattern(regexp = "^[0-9]", message = "Invalid contact")
  private String contact;

public String getUsername() {
	return username;
}

public void setUsername(String username) {
	this.username = username;
}

public String getPassword() {
	return password;
}

public void setPassword(String password) {
	this.password = password;
}

public String getEmail() {
	return email;
}

public void setEmail(String email) {
	this.email = email;
}

public String getContact() {
	return contact;
}

public void setContact(String contact) {
	this.contact = contact;
} 
}
```
RegistrationController.java
```java

package com.validation;
import javax.validation.Valid;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
@Controller
public class RegistrationController {
  @RequestMapping(value = "/register", method = RequestMethod.GET)
  public String showForm(Model model) {
    model.addAttribute("registration", new RegistrationBean());
    return "register";
  }
  @RequestMapping(value = "/processForm", method = RequestMethod.POST)
  public String processForm(@Valid @ModelAttribute("registration") RegistrationBean register,
      BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
      return "register";
    } else {
      return "success";
    }
  }
}
package com.validation;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
@Controller
public class RegistrationController {
	
	@RequestMapping("/register" )
	public ModelAndView getForm()
	{
		ModelAndView model = new ModelAndView("register");
		return model;
	}
	
	@RequestMapping("/success")
	public ModelAndView acceptForm(@ModelAttribute("register") RegistrationBean reg)
	{
		ModelAndView model = new ModelAndView("success");
		return model;
		
	}
}
package com.validation;

import javax.validation.Valid;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class RegistrationController {

  @RequestMapping("/register")
  public String showForm(Model model) {
    model.addAttribute("registration", new RegistrationBean());
    return "register";
  }

  @RequestMapping("/processForm")
  public String processForm(@Valid @ModelAttribute("registration") RegistrationBean register,
      BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
      return "register";
    } 
    else 
    {
      return "success";
    }

  }
}
```
Spring-dispatcher-servlet.java
```
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:mvc="http://www.springframework.org/schema/mvc" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="
    http://www.springframework.org/schema/beans     
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context 
    http://www.springframework.org/schema/context/spring-context-3.0.xsd
    http://www.springframework.org/schema/mvc
    http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

	
	<context:component-scan base-package="org.springmvc"/>
	 
	<context:component-scan base-package="com.validation"/>
	  
	 <context:component-scan base-package="com.collection4"/>
	 <mvc:annotation-driven />
	
	 
	
	<bean id="viewResolver"
			class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix">
				<value>/WEB-INF/</value>
			</property>
			<property name="suffix">
				<value>.jsp</value>
			</property>
	</bean>
	
</beans>
```
Confirmation.jsp
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
   pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
   <head>
      <title>Registration Confirmation</title>
   </head>
   <body>
      Hello <font color="green">${registration.username} ${registration.password} </font> you have successfully registered with us.
   </body>
</html>
```
# Q7 Add custom validations in the above example.
Contact should be only numeric and exactly 10-digits.
```java
//Isnumerialc.java

package com.controller;

import javax.validation.Constraint;
import javax.validation.Payload;

import net.sourceforge.retroweaver.runtime.java.lang.annotation.Documented;
import net.sourceforge.retroweaver.runtime.java.lang.annotation.ElementType;
import net.sourceforge.retroweaver.runtime.java.lang.annotation.Retention;
import net.sourceforge.retroweaver.runtime.java.lang.annotation.RetentionPolicy;
import net.sourceforge.retroweaver.runtime.java.lang.annotation.Target;

@Documented
@Constraint(validatedBy = Numeric.class)
@Target( { ElementType.FIELD } )
@Retention( RetentionPolicy.RUNTIME)
public @interface Isnumerical {
	
	String message() default "Not Numeric" + 
			"Please enter a numeric value";

	Class<?>[] groups() default {};
	
	Class<? extends Payload>[] payload() default {};
}
```
Numeric.java
```java
package com.controller;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;


public class Numeric implements ConstraintValidator<IsNumeric, String> {

	private static final String CONTACT_PATTERN = "((?=.*\\d).{10,10})";
	private Pattern pattern;
	private Matcher matcher;
	
	public Numeric()
	{
		pattern = Pattern.compile(CONTACT_PATTERN);
	}
	
	@Override
	public void initialize(IsNumeric isNumeric)
	{
		isNumeric.message();
	}
	
	@Override
	public boolean isValid(String contact, ConstraintValidatorContext arg1) {
		
		if(contact == null)
		{
			return false;
		}
		
		matcher = pattern.matcher(contact);
		return matcher.matches();
	}

}
```
Customer.java
```java
package com.controller;

import java.util.ArrayList;

import javax.validation.constraints.NotEmpty; import javax.validation.constraints.NotNull; import javax.validation.constraints.Pattern; import javax.validation.constraints.Size;

public class Customer 
{
  @NotNull
@NotEmpty
@Pattern(regexp = "^[\\p{Alnum}]{8,20}$")
private String username;

@NotNull
@NotEmpty
@Pattern(regexp = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*[.@#;$]).{8,20}$")
private String password;

@NotNull
@NotEmpty
private String email;

//using custom annotation
@IsNumeric
private String contact;

@NotNull
@NotEmpty
@Size(min = 6, max = 6)
private Long zip;

@NotEmpty
private ArrayList<String> city;

public String getUsername() {
	return username;
}
public void setUsername(String username) {
	this.username = username;
}
public String getPassword() {
	return password;
}
public void setPassword(String password) {
	this.password = password;
}
public String getEmail() {
	return email;
}
public void setEmail(String email) {
	this.email = email;
}
public String getContact() {
	return contact;
}
public void setContact(String contact) {
	this.contact = contact;
}
public Long getZip() {
	return zip;
}
public void setZip(Long zip) {
	this.zip = zip;
}
public ArrayList<String> getCity() {
	return city;
}
public void setCity(ArrayList<String> city) {
	this.city = city;
}
}
```






