---
title: "@MatrixVariable"
category: spring
layout: post
---

> `@MatrixVariable` annotation

***

1. 정의
2. 사용

***

# 1. 정의
request URL에 파라미터를 포함하여 전송하는 경우 사용되는 애노테이션이다. `@PathVariable`이 하나에 경로에 대한 단일 파라미터만을 처리하였다면, `@MatrixVariable` 애노테이션은 하나에 경로에 대해 다중 파라미터를 처리할 수 있다. 

`@RequestParam` 애노테이션으로 처리를 할 수 있는 부분이지만, 키값을 기준으로 파라미터를 나누어 사용할 수 있다는 차이점이 있다.

# 2. 사용
`@MatrixVariable` 애노테이션을 사용하기 위해서는 서블릿 컨텍스트내에 설정이 필요하다.
`servlet-context.xml`
```xml
<annotation-driven enable-matrix-variables="true"/>
```

`AnyController.java` : request url: /sample/matrix;attribute=attr
```java
@RequestMapping("/sample/matrix")
public String matrixVariableSampleMethod (
    @MatrixVariable String attribute
) {}
```

`AnyController.java` : request url: /sample/{pathVarialbe}/matrix1;attribute=attr/matrix2;attribute1=attr1;attribute2=attr2;
```java
@RequestMapping("/sample/{path}/matrix1/matrix2")
public String matrixVariableSampleMethod (
    @PathVariable String path,
    @MatrixVariable(pathVar="matrix1", value="attribute") String attribute,
    @MatrixVariable(pathVar="matrix2", value="attribute1") String attribute1,
    @MatrixVariable(pathVar="matrix2", value="attribute2") String attribute2  
) {}
```

`AnyController.java` : request url: /sample/{pathVarialbe}/matrix1;attribute=attr/matrix2;attribute1=attr1;attribute2=attr2;
```java
@RequestMapping("/sample/{path}/matrix1/matrix2")
public String matrixVariableSampleMethod (
    @PathVariable String path,
    @MatrixVariable(pathVar="matrix1", value="attribute") String attribute,
    @MatrixVariable(pathVar="matrix2") MultiValueMap<String, String> matrix2  
) {}
```