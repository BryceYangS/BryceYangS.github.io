---
layout: post
title: "[Spring] HandlerMethodArgumentResolver?"
subtitle: "HandlerMethodArgumentResolver"
categories: study
tags: springboot
---
> HandlerMethodArgumentResolver

![김영한님의 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술](/assets/img/springboot/argument_resolver.png)
*이미지 참조 : 김영한님의 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술*

## ArgumentResolver
HttpServletRequest, Mode, @RequestParam, @ModelAttribute, @RequestBody, HttpEntity 모두 처리 가능하도록 ArgumentResolver 인터페이스를 구현한 구체 클래스들이 존재.

### 동작방식
ArgumentResolver의 `supportsParameter()`를 호출해서 해당 파라미터를 지원하는지 체크, 지원한다면 `resolveArgument()`를 호출해서 실제 객체를 생성한다. 그리고 이렇게 생성된 객체가 컨트롤러 호출 시 넘어가는 것.  
→ 직접 인터페이스를 확장해 커스터마이징한 ArgumentResolver를 만들 수 있음.  

ArgumentResolver가 HttpMessageConverter를 사용해 요청과 응답시 필요한 객체 혹은 데이터 변환을 처리함.


### Customize HandlerMethodArgumentResolver

#### 1. WebMvcConfigurer의 addArgumentResolvers 오버라이드
```java
@Configuration
public class WebMvcConfigure implements WebMvcConfigurer {

	@Override
	public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
		argumentResolvers.add(Search());
	}

	public HandlerMethodArgumentResolver Search() {
		return new SearchArgumentResolver();
	}
}
```

#### 2. HandlerMethodArgumentResolver 구현

```java
public class SearchArgumentResolver implements HandlerMethodArgumentResolver {

   /* 지원 여부 */
   @Override
   public boolean supportsParameter(MethodParameter methodParameter) {
      return methodParameter.getParameterType().equals(Search.class);
   }

   /* 인자 할당 */
   @Override
   public Object resolveArgument(MethodParameter methodParameter, ModelAndViewContainer modelAndViewContainer,
      NativeWebRequest nativeWebRequest, WebDataBinderFactory webDataBinderFactory) throws Exception {
      return new Search(
         LocalDate.parse(nativeWebRequest.getParameter("date"), DateTimeFormatter.ISO_DATE),
         nativeWebRequest.getParameter("test1"),
         nativeWebRequest.getParameter("test2")
      );
   }
}
```

#### 3. 사용

```java
@RestController
public class TestController{
	@GetMapping("/test")
	public String retrieve(Search search) throws Exception {
		return testService.retrieve(search);
	}
}
```


### References
- 인프런, 김영한님의 \<스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술\> : [https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)