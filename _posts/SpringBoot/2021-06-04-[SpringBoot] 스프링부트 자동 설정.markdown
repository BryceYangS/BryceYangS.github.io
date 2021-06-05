---
layout: post
title: "[SpringBoot] 스프링부트 자동 설정"
subtitle: "스프링부트 자동 설정"
categories: study
tags: springboot
---
> 스프링부트의 자동 설정


## SpringBoot Auto Configuration 작동 방식

@SpringBootApplication 내부 @SpringBootConfiguration, @EnableAutoConfiguration 등 있음. `@EnableAutoConfiguration`이 스프링 부트에서 자동설정 관련 기능을 담당.  

@EnableAutoConfiguration 내부에 `@Import(AutoConfigurationImportSelector.class)` 

```java
public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware,
		ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {
	@Override
	public String[] selectImports(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return NO_IMPORTS;
		}
		AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(annotationMetadata);
		return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
	}
}
```

getAutoConfigurationEntry() → `getCandidateConfigurations()` : **META-INF/spring.factories**에 정의된 자동 설정 클래스를 불러옴.  
- sping.factories 목록에 있는 모든 것들이 bean으로 등록되는 것은 아님
- Configuration 클래스를 들어가보면 **Conditional**로 시작하는 애노테이션의 조건을 만족하는 클래스만 bean으로 등록된다.


Classpath에 라이브러리 jar 파일이 등록되면 spring.factories에 있는 관련 설정이 실행된다.  

자동 설정 후보 클래스의 @Conditional* 조건에 따라 빈으로 등록된다.  

spring-configuration-metadata는 자동 설정에 사용할 프로퍼티 정의 파일로, application.yml에 작성한 값으로 프로퍼티를 세팅한 후, 구현되어 있는 자동 설정에 값을 주입시켜준다.  

