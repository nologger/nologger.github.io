---
title: 『intellij_community』tomcat 연결
category: ide
layout: post
---

> Intellij community에서 외장 Tomcat을 내장처럼 이용하여 프로젝트 실행하는 과정을 나타낸다.

***

1. 새 프로젝트
2. 의존성 주입
3. 프로젝트 구조 생성
4. 관련 파일 정보
5. 실행 및 추가사항

***

# 1. 새 프로젝트
intellij > 새프로젝트
- 시스템 빌드: Gradle

# 2. 의존성 주입
**build.gradle**
```gradle
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework:spring-webmvc:5.3.24'
    implementation 'javax.servlet:jstl:1.2'
    implementation 'com.fasterxml.jackson.core:jackson-databind:2.14.2'
    implementation 'org.apache.tomcat.embed:tomcat-embed-core:8.5.23'
    implementation 'org.apache.tomcat.embed:tomcat-embed-jasper:8.5.23'
    implementation 'org.apache.tomcat:tomcat-jasper:8.5.23'
    implementation 'org.apache.tomcat:tomcat-jasper-el:8.5.23'
    implementation 'org.apache.tomcat:tomcat-jsp-api:8.5.23'

    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'

    compileOnly 'javax.servlet.jsp:javax.servlet.jsp-api:2.3.3'
    compileOnly 'org.projectlombok:lombok:1.18.26'
    compileOnly 'javax.servlet:javax.servlet-api:4.0.1'
}

test {
    useJUnitPlatform()
}
```

# 3. 프로젝트 구조 생성
```
src
├ main
│ ├ java/kim
│ │ ├ ma
│ │ │ └ MainController
│ │ └ SpringWebMvcApplication.java
│ ├ resources
│ └ webapp/WEB-INF
│   ├ spring
│   │ ├ root-context.xml
│   │ └ servlet-context.xml
│   ├ views
│   │ └ index.jsp
│   ├ resources
│   │ ├ image
│   │ ├ css
│   │ └ js
│   └ web.xml
└ test
  ├ java
  └ resources
```

# 4. 관련 파일 정보
`web.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 네임 스페이스와 스키마 선언 ########################################################################-->
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
    <!-- //네임 스페이스와 스키마 선언 ######################################################################-->


    <!-- 루트 컨텍스트 설정 ################################################################################-->
    <!-- The definition of the Root Spring container shared by all Servelts and Filters -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            /WEB-INF/spring/root-context.xml
        </param-value>
    </context-param>
    <!-- Creates the Spring Container shared by all Servlets and Filters -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- //루트 컨텍스트 설정 ###############################################################################-->

    
    <!-- 서블릿 컨텍스트 설정 ###############################################################################-->
    <!-- Processes application requests -->
    <servlet>
        <servlet-name>applicationServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring/servlet-context.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>applicationServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- //서블릿 컨텍스트 설정 ############################################################################# -->
</web-app>
```

`root-context.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Root Context: defines shared resources visible to all other web components -->

</beans>
```

`servlet-context-xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:beans="http://www.springframework.org/schema/beans"
             xmlns:context="http://www.springframework.org/schema/context"
             xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->

    <!-- Enables the Spring MVC @Controller programming model -->
    <annotation-driven enable-matrix-variables="true"/>

    <!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
    <resources mapping="/resources/**" location="/resources/" />

    <!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <beans:property name="prefix" value="/WEB-INF/views/" />
        <beans:property name="suffix" value=".jsp" />
    </beans:bean>

    <context:component-scan base-package="kim" />
</beans:beans>
```

`SpringWebMvcApplication.java`
```java
package org.example;

import org.apache.catalina.LifecycleException;
import org.apache.catalina.core.StandardContext;
import org.apache.catalina.startup.Tomcat;

import javax.servlet.ServletException;
import java.io.File;

public class SpringWebMvcApplication {
    public static void main(String[] args) throws ServletException, LifecycleException {

        Tomcat tomcat = new Tomcat();        
        StandardContext ctx = (StandardContext)tomcat.addWebapp("/", (new File("src/main/webapp/")).getAbsolutePath());

        tomcat.setPort(8080);
        tomcat.start();
        tomcat.getServer().await();
    }
}
```

`MainController.java`
```java
package org.example.main;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class MainController {
    @RequestMapping("/")
    public String index() {
        System.out.println("MainController test");
        return "index";
    }
}
```

`index.jsp`
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <title>index</title>
</head>
<body>
    intellij spring web mvc project index test !
</body>
</html>
```

# 5. 실행 및 추가사항
`SpringMvcApplication.java`의 `main` 메서드를 실행시킴으로써 톰캣을 실행시킨다.

추가적으로, 코드 작성시 반영이 안되거나 새로운 컨트롤러 작성시 `request path`를 제대로 캐치하지 못하는 문제가 발생하는데, 현재 관련 문제는 파악중이다. 임시방편으로 `gradle`의 `clean`을 이용하여 새로운 빌드를 통해 처리중이다.