---
title: "@RequestParam"
category: spring
layout: post
---

> `@RequestParam` annotation

***

1. 정의
2. 사용

***

# 1. 정의
일반적인 웹 서버 애플리케이션의 GEt 방식 쿼리문의 전달 파라미터를 매핑하기 위해 사용된다.

# 2. 사용
`AnyController.java`
```java
// request url: /sample?attribute1=attr1&attribute2=attr2
@RequestMapping("/sample")
public String pathVariableSampleMethod(
    @RequestParam String attribute1,
    @RequestParam(name="attribute2") String attribute2
) {}
```