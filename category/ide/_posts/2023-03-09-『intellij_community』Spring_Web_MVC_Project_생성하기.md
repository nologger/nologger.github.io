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
│ ├ java/groupId/artifactId
│ │ ├ main
│ │ │ ├ sample
│ │ │ │ └ SampleController.java
│ │ │ └ MainController
│ │ └ SpringWebMvcApplication.java
│ ├ resources
│ └ webapp/WEB-INF
│   ├ spring
│   │ └ spring-context.xml
│   ├ views
│   │ ├ sample
│   │ │ └ sample.jsp
│   │ └ index.jsp
│   └ web.xml
└ test
  ├ java
  └ resources
```

# 4. 관련 파일 정보
`web.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://jva.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
    <servlet>
        <servlet-name>spring</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring/spring-context.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>spring</servlet-name>
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
</web-app>
```

`spring-context.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">


    <mvc:annotation-driven />
    <mvc:default-servlet-handler/>

    <mvc:resources mapping="/resources/**" location="/resources/" />

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <bean id="jsonView" class="org.springframework.web.servlet.view.json.MappingJackson2JsonView">
        <property name="contentType" value="text/html; charset=UTF-8"/>
    </bean>

    <bean id="beanNameViewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver">
        <property name="order" value="0"/>
    </bean>

    <context:component-scan base-package="org.example.main"/>
</beans>
```

`SpringWebMvcApplication.java`
```java
package org.example;

import org.apache.catalina.LifecycleException;
import org.apache.catalina.core.StandardContext;
import org.apache.catalina.startup.Tomcat;

import javax.servlet.ServletException;
import java.io.File;

public class SpringMvcApplication {
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

`SampleController.java`
```java
package org.example.main.sample;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class SampleController {
    @RequestMapping("/sample")
    public String sample() {
        System.out.println("sample controller test");
        return "sample/sample";
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

`sample.jsp`
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
    <title>index</title>
</head>
<body>
    intellij spring web mvc project sample test !
</body>
</html>
```

# 5. 실행 및 추가사항
`SpringMvcApplication.java`의 `main` 메서드를 실행시킴으로써 톰캣을 실행시킨다.

추가적으로, 코드 작성시 반영이 안되거나 새로운 컨트롤러 작성시 `request path`를 제대로 캐치하지 못하는 문제가 발생하는데, 현재 관련 문제는 파악중이다. 임시방편으로 `gradle`의 `clean`을 이용하여 새로운 빌드를 통해 처리중이다.