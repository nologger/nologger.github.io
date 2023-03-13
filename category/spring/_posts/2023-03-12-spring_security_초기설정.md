---
title: spring security 초기설정
category: spring
layout: post
---

> spring security

***

1. 정의
2. 초기설정

***

# 1. 정의
스프링 기반 애플리케이션의 보안 프레임워크

# 2. 초기설정

## 2.1 의존성 주입
`build.gradle`
```gradle
...

dependencies {
    implementation 'org.springframework.security:spring-security-web:5.3.13.RELEASE'
    implementation 'org.springframework.security:spring-security-config:5.3.13.RELEASE'
}

...
```

## 2.2 DelegatingFilterProxy 설정
`web.xml`
```xml
    <!-- ... -->

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            /WEB-INF/spring/root-context.xml
            /WEB-INF/spring/security-context.xml
        </param-value>
    </context-param>

    <!-- ... -->

    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!-- ... -->
```

## 2.3 security-context.xml
`security-context.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans
        xmlns="http://www.springframework.org/schema/security"
        xmlns:beans="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/security
 http://www.springframework.org/schema/security/spring-security.xsd">
</beans:beans>
```