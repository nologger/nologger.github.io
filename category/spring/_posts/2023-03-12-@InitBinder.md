---
title: "@InitBinder"
category: spring
layout: post
---

> `@InitBinder` annotation

***

1. 정의
2. 사용

***

# 1. 정의
`@InitBinder` 애노테이션은 커맨드 객체 파라미터가 객체 프로퍼티에 매핑되기 전에 사용자가 정의한 형태의 데이터 바인딩을 지원하는 애너테이션이다.

# 2. 사용
일반적으로 커맨드 객체 파라미터가 객체 프로퍼티에 매핑될 때, 전송을 요청한 폼 페이지의 모든 파라미터에 대해 바인딩을 시도한다. `@InitBinder` 애너테이션을 이용하여 명시된 파라미터만을 매핑하거나(`setAllowedFields`) 또는 명시된 파라미터만을 매핑하지 않을 수 있다(`setDisallowedFields`).

```java
import org.springframework.web.bind;
import org.springframework.web.bind.annotation;

@Controller
public class AnyController {
    @RequestMapping
    public String sample(
        @ModelAttribute Sample sample
    ) {}

    @InitBinder
    public void sampleForSetAllowedFileds(WebDataBinder binder) {
        binder.setAllowedFileds("attr1", "attr2");
    }

    @InitBinder
    public void sampleForSetDisallowedFields(WebDataBinder binder) {
        binder.setDisallowedFields("attr3");
    }
}
```