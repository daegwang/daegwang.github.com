---
layout: post
title: "Spring MVC Framework"
description: "스프링 MVC 프레임워크 설명"
date: 2017-01-27
tags: [java, spring, mvc]
comments: true
share: true
---

## DispatcherServlet
클라이언트의 요청을 받아 컨트롤러에게 전달하는 역할을 수행합니다. web.xml에 설정이 되어 있으며 관련 설정은 servlet-context.xml에서 추가할 수 있습니다.  

##### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

</web-app>
```

또한 인코딩을 위해 해당 부분을 추가해 줍니다.

```xml
<filter>
  <filter-name>encoding</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
   <param-name>encoding</param-name>
   <param-value>UTF-8</param-value>
  </init-param>
</filter>

<filter-mapping>
  <filter-name>encoding</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

##### appServlet/servlet-context.xml
스프링 컨테이너 설정 파일입니다.  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />

	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />

	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<context:component-scan base-package="com.jdg.controller" />
	
</beans:beans>
```


## 1. HandlerMapping
### DefaultAnnotationHandlerMapping
- @RequestMapping어노테이션을 이용한 매핑 방법입니다. 컨트롤러 내의 메소드별로 매핑이 가능합니다.
- URL, GET/POST, Parameter 등을 매핑에 활용할 수 있습니다.

### @PathVariable

```java
@RequestMapping(value = "/user/{username}", method = RequestMethod.GET)
public String user(@PathVariable String username, Model model) {
	model.addAttribute("username", username );

	return "user";
}
```
@PathVariable을 사용하여 url에 사용되는 변수를 메소드 내의 Parameter로 사용할 수 있습니다.

### @RequestParam

```java
@RequestMapping(value = "/add", method = RequestMethod.GET)
public String add(@RequestParam("name") String name
		, @RequestParam("age") String age
		, Model model) {
	model.addAttribute("name", name );
	model.addAttribute("age", age );
	
	return "add";
}
```

`/add?name=lucas&age=20` 을 호출하게 되면 add메소드와 url매핑이 이루어지게 되며, name에는 `lucas`가, age에는 `20`이 할당됩니다.

### Object 사용

```java
public class User{
	String name;
	String age;
	//getter&setter
	...
}
```
다음과 같은 `getter&setter`메소드가 구현된 `User`객체가 존재한다고 하면 더 편리하게 데이터를 가져올 수 있습니다.

```java
@RequestMapping(value = "/add", method = RequestMethod.GET)
public String add(User user) {
	return "add";
}
```

jsp파일내에서 객체 속성을 접근하기 위해서는  `${user.name}`과 같이 사용하시면 됩니다.

### @ModelAttribute

```java
@RequestMapping(value = "/add", method = RequestMethod.GET)
public String add(@ModelAttribute("myuser") User user) {
	return "add";
}
```
위와 같이 객체 앞에 @ModelAttribute를 사용하시면  jsp에서 `${myuser.name}`과 같이 접근 가능합니다.

### redirect:
메소드 내에서 다른 url로 이동하고 싶을 때 사용 가능합니다.

```java
@RequestMapping(value = "/add", method = RequestMethod.GET)
public String add(HttpServletRequest request, Model model) {
	return "redirect:result";
}

@RequestMapping(value = "/result", method = RequestMethod.GET)
public String result(Model model) {
	return "result";
}
```


## 2. Controller

##### myController.java  

```java
package com.jdg.example;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Handles requests for the application home page.
 */
@Controller
public class HomeController {
	
	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);
	
	/**
	 * Simply selects the home view to render by returning its name.
	 */
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		logger.info("Welcome home! The client locale is {}.", locale);
		
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		
		String formattedDate = dateFormat.format(date);
		
		model.addAttribute("serverTime", formattedDate );
		
		return "home";
	}
	
}
```

컨트롤러 클래스에서는 매핑된 메소드를 실행하고 결과를 출력할 뷰와 뷰에 전달할 ModelAndView객체를 리턴합니다. 위 소스에서는 home.jsp를 뷰로 보여주며 model이 뷰에 전달됩니다.

## 3. ViewResolver
컨트롤러에서 보여쥴 뷰의 이름으로 `home`을 지정하였습니다. DispatcherServlet은 이와 매칭되는 뷰를 찾기 위해 ViewResolver를 사용하게 됩니다. 

```xml
<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<beans:property name="prefix" value="/WEB-INF/views/" />
	<beans:property name="suffix" value=".jsp" />
</beans:bean>
```

스프링 컨테이너 설정 파일 안에서 ViewResoler를 설정할 수 있습니다.  
이 값은 prefix + 리턴값 + suffix 이 되며 이는 `/WEB-INF/views/` + `home` + `.jsp` = `/WEB-INF/views/home.jsp` 를 나타냅니다.  
예제에서 사용된 ViewResolver는 InternalResourceViewResolver이며, 이외에도 다양한 Resolver들이 존재합니다. (XmlViewResolver, XstlViewResolver, VelocityViewResolver 등)

## 4. View(JSP)
##### home.jsp

```xml
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  The time on the server is ${serverTime}. </P>
</body>
</html>
```

실제로 사용자에게 보여질 뷰 화면입니다. 컨트롤러에서 전달해주는 객체의 값을 사용할 수 있습니다. 