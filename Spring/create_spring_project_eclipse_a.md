# Eclipse에서 Spring 프로젝트 생성하기-A

### Marketplace에서 Spring Tools 3 (Standalone Edition) or Spring Tools 4 (aka Spring Tool Suite 4) 를 설치하면 더 쉽게 구성 가능합니다.

이클립스에서 New > Dynamic Web Project 를 클릭합니다.   
Target runtime : tomcat 8.n.n   
Dynamic web module version : 3.1   
Configuration : Default Configuration for apache-tomcat-8.n.n   

Next > Next 클릭후 Generate web.xml deployment descriptor를 선택하고 Finish를 클릭합니다.

- - - - -

만약 프로젝트 생성시 Generate web.xml deployment descriptor 선택을 못했다면 프로젝트 우클릭 > Java EE Tools > Generate Deployment Descriptor Stub 을 선택하면 됩니다.

- - - - -

만약 web.xml 에서 \<web-app 줄에 빨간 경고가 출력된다면 네임스페이스가 다른 버전으로 되어 있는지 확인합니다.   

```
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
```

- - - - -

프로젝트 우클릭 > Configure > Convert to Maven Project를 클릭합니다.

- - - - -

프로젝트 우클릭 > Properties를 클릭 후 (shortcut Alt+Enter)   

Resource 에서는 Text file encoding 을 UTF-8로 설정합니다.   
Java Build Path 에서는 JRE와 Server Runtime을 맞춰줍니다.   

이 예시에서는 Project Facets 항목을 Dynamic Web Module 3.1, Java 1.8 을 선택하며 우측에 Runtimes 에 apache-tomcat 이 선택되어 있는지 확인합니다.

수정을 완료했다면 Apply and Close를 클릭합니다.

- - - - -

만약 클래스를 찾을 수 없다는 에외가 발생하면 프로젝트 우클릭 > Maven > Update Project 을 먼저 하고   

프로젝트 우클릭 > Properties를 클릭 후 Deployment Assembly에 Maven Dependencies 가 있는지 확인하며 없을 경우 Add > Java Build Path Entries > Maven Dependencies를 추가합니다.

- - - - -

src/main/webapp/index.jsp 파일을 생성 후   
Tomcat Server에 프로젝트를 추가 후 실행합니다.

루트(/) 혹은 /프로젝트명 으로 접근했을때 404가 아닌 빈 페이지가 출력되는지 확인합니다.   

만약 루트(localhost:8080)로 프로젝트를 열고 싶다면 톰캣서버의 server.xml 하단에 추가되어 있는 프로젝트의 path 를 "/" 으로 수정합니다.   

- - - - -

pom.xml에 properties를 추가합니다
```
<properties>
    <spring.version>4.2.6.RELEASE</spring.version>
</properties>
```

pom.xml에 dependencyManagement를 추가합니다
```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-framework-bom</artifactId>
            <version>${spring.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

pom.xml에 dependencies를 추가합니다.
```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
    </dependency>
</dependencies>
```
- - - - -

src/main/webapp/HTTP404.jsp, src/main/webapp/HTTP500.jsp 파일을 생성하고 web.xml 에 에러 페이지로 매핑합니다.   

*** src/main/webapp/WEB-INF/web.xml ***
```
<error-page>
    <error-code>404</error-code>
    <location>/HTTP404.jsp</location>
</error-page>

<error-page>
    <error-code>500</error-code>
    <location>/HTTP500.jsp</location>
</error-page>
```
- - - - -

//TODO//   

*** src/main/webapp/WEB-INF/web.xml ***
```
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/spring/root-context.xml</param-value>
</context-param>
```

//TODO//   

*** src/main/webapp/WEB-INF/spring/root-context.xml ***
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

//TODO//   

*** src/main/webapp/WEB-INF/web.xml ***
```
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

//TODO//   

*** src/main/webapp/WEB-INF/web.xml ***
```
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
```

//TODO//   

*** src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml ***
```
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

	<annotation-driven />
	<context:component-scan base-package="com.qrry12b.spring" />

	<resources mapping="/resources/**" location="/resources/" />

	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
</beans:beans>
```

- - - - -

*** src/main/java/com/qrry12b/spring/HomeController.java ***
```
package com.qrry12b.spring;

import import org.springframework.stereotype.Controller;
import javax.servlet.http.HttpServletRequest;
import org.springframework.web.bind.annotation.*;

@Controller
public class HomeController {
    @RequestMapping(value = "/", method = RequestMethod.GET)
    public String main() {
        return "main";
    }
}
```

- - - - -

*** src/main/webapp/WEB-INF/views/main.jsp ***
```
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Main Page</title>
</head>
<body>
	Main Page
</body>
</html>
```

- - - - -