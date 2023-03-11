---
title: "@PathVariable"
category: spring
layout: post
---

> `@ParhVariable` annotation

***

1. 정의
2. 사용

***

# 1. 정의
request URL에 파라미터를 포함하여 전송하는 경우 사용되는 애노테이션이다.

# 2. 사용
`AnyController.java`
```java
// request url: /sample/sample1/sample2/sample3/sample4
@RequestMapping("/sample/{param1}/{param2}/{param3}/{param4}")
public String pathVariableSampleMethod(
    @PathVariable String param1,
    @PathVariable("param2") String param2,
    @PathVariable(name="param3") String param3,
    @PathVariable(value="param4") String param4
) {}
```