---
layout: post
title: "[Interview] 면접 공부"
subtitle: "interview"
categories: study
tags: etc
---
> 면접 공부  

## 1. Java
### HashMap & TreeMap
[[Java] HashMap vs TreeMap](/study/2021/05/24/Java-HashMap-vs-TreeMap)

### primitive & reference / Boxing & Unboxing
[[Java] Primitive & Reference & Boxing & Unboxing](/study/2021/05/24/Java-Primitive-&-Reference-&-Boxing-&-Unboxing)

### JVM 메모리
[[Java] JVM Stack & Heap](/study/2021/05/24/Java-JVM-Stack-&-Heap)

### Generic
[[Java]Generics](/study/2020/10/26/Java-Generics)

### GC
[[Java] Garbage Collection](/study/2021/05/14/Java-Garbage-Collection/)

### Java 6-8-11 차이.

### JSR 310?.


## 2. HTTP
### POST & PUT 동일 요청 반복 경우 어떠한 결과가 나오는가
[[HTTP] POST & PUT 반복요청 차이점](/study/2021/05/28/HTTP-POST-&-PUT-반복요청-차이점)

### Redirection
[[HTTP] Redirection](/study/2021/05/28/HTTP-Redirection)

### HTTP - HTTPS 차이.


## 3. 네트워크
### Load-Balancer
[[Network] Load Balacing](/study/2021/05/24/Network-Load-Balacing)

### TCP vs UDP
[[Network] TCP & UDP](/study/2021/05/24/Network-TCP-&-UDP)



## 4. 데이터베이스
### 복합키

### Sharding & Replication
[[Database] Sharding & Replication & Clustering](/study/2021/06/01/Database-Sharding-&-Replication-&-Clustering)

### PreparedStatement & Statement
[[Java] Statement & PreparedStatement](/study/2021/05/24/Java-Statement-&-PreparedStatement)



## 5. 스프링
### 커넥션 풀
### @Transactional
### 빈 스코프

### Spring AOP
### SpringBoot Autoconfiguration 작동방식.
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

getAutoConfigurationEntry() → `getCandidateConfigurations()` : META-INF/spring.factories에 정의된 자동 설정 클래스를 불러옴.  
- sping.factories 목록에 있는 모든 것들이 bean으로 등록되는 것은 아님
- Configuration 클래스를 들어가보면 **Conditional**로 시작하는 애노테이션의 조건을 만족하는 클래스만 bean으로 등록된다.


Classpath에 라이브러리 jar 파일이 등록되면 spring.factories에 있는 관련 설정이 실행된다.  

자동 설정 후보 클래스의 @Conditional* 조건에 따라 빈으로 등록된다.  

spring-configuration-metadata는 자동 설정에 사용할 프로퍼티 정의 파일로, application.yml에 작성한 값으로 프로퍼티를 세팅한 후, 구현되어 있는 자동 설정에 값을 주입시켜준다.  


## 6. 디자인 패턴
### 전략패턴
[전략패턴 정리 글](https://github.com/BryceYangS/Java/blob/main/DesignPattern/posting/1_Strategy_Pattern.md)
### 옵저버 패턴
[옵저버 패턴 정리 글](https://github.com/BryceYangS/Java/blob/main/DesignPattern/posting/2_Observer.md)


## 7. 동시성
### 프로세스 작동 방식 & 쓰레드 작동 방식
[[Java] Thread](/study/2021/02/22/Java-Thread/)


## 8. OS
### Linux 메모리 꽉 찰 경우 발생하는 현상 - swap memory