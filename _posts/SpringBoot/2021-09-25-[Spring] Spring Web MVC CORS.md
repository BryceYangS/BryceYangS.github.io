---
layout: post
title: "[Spring] Spring Web MVC의 CORS"
subtitle: "Spring CORS"
categories: study
tags: springboot
---
> Spring Web MVC의 CORS

백기선님의 `스프링 부트 개념과 활용` 인프런 강의를 정리한 내용입니다.


- Single-Origin Policy
    - 서로 다른 resource는 원칙적으로 호출/공유할 수 없음
- Cross-Origin Resource Sharing
    - 서로 다른 resource에 대해 호출/공유할 수 있도록 하는 기술

## Origin?
- URI 스키마 (http, https)
- hostname (whiteship.me, localhost)
- port (8080, 18080)
위 세 기준으로 origin이 생성됨.

## 스프링 MVC @CrossOrigin
- @Controller나 @RequestMapping에 추가하거나
- WebMvcConfigurer 사용해서 글로벌 설정


```java
@RestController
public class SampleController {
    
    // 18080 서버에서 호출이 가능해짐
    @CrossOrigin(origins = "http://localhost:18080")
    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```


```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
            .allowedOrigins("http://localhost:18080");
    }
} 
```

## References
- [https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/]https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/)