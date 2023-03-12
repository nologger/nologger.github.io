---
title: "@ModelAttribute"
category: spring
layout: post
---

> `@ModelAttribute` annotation

***

1. 정의
2. 사용
    1. 파라미터 사용 예시
    2. 메서드 사용 예시

***

# 1. 정의
request URL에 커맨드 객체를 전달하여 바인딩하기 위해 사용되는 애노테이션이다.

# 2. 사용
## 2.1. 파라미터 사용 예시
`Sample.java`
```java
@Getter
@Setter
public class Sample {
    String attr1;
    String attr2;
}
```

`SampleController.java`
```java
@Controller
public class SampleController {
    @RequestMapping("/sample1")
    public String sample1(
        @ModelAttribute Sample sample
    ) {
        System.out.println("sample.attr1=" + sample.getAttr1());
        System.out.println("sample.attr2=" + sample.getAttr2());
        return "sample1";    
    }

    // Spring MVC는 @ModelAttirbute와 관계없이 데이터 바인딩 작업 진행
    @RequestMapping("/sample2")
    public String sample2(
        Sample sample
    ) {
        System.out.println("sample.attr1=" + sample.getAttr1());
        System.out.println("sample.attr2=" + sample.getAttr2());
        return "sample2";
    }
}
```
## 2.2. 메서드 사용 예시
`Sample.java`
```java
@Getter
@Setter
public class Sample {
    String attr1;
    String attr2;
}
```

`SampleController.java`
```java
@Controller
public class SampleController {
    @ModelAttribute("attribute1")
    public String setAttribute1() {
        return "attr1";
    }

    @ModelAttribute
    public void setAttribute2(Model model) {
        model.addAttribute("attribute2", "attr2");
    }

    @RequestMapping("/sample1")
    public String sample1(
        @ModelAttribute Sample sample
    ) {
        System.out.println("sample.attr1=" + sample.getAttr1());
        System.out.println("sample.attr2=" + sample.getAttr2());
        return "sample";    
    }
}
```

`sample.jsp`
```jsp
<!-- ... -->
<div>
    <span>${ attribute1 }</span>
    <span>${ attribute2 }</span>
</div>
<!-- ... -->
```