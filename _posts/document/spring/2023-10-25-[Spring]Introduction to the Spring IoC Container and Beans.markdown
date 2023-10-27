---
layout: post
title: "[Spring] Spring IoC Container, Bean 개괄"
subtitle: "IoC 컨테이나와 Bean"
categories: document
tags: spring-doc
---

> Spring 공식 문서 정리  
> - Introduction to the Spring IoC Container & Beans
> - Container Overview
> - Bean Overview

## [Introduction to the Spring IoC Container and Beans](https://docs.spring.io/spring-framework/reference/core/beans/introduction.html)

### IoC (Inversion Of Control)
- 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 **외부**에서 관리

### DI
- 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것
- 장점
  - 관심사의 분리를 통한 **높은 응집도**
  - 클라이언트 코드 변경 없이, 클라이언트가 호출하는 대상의 타입 인스턴스 변경 가능
  - 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계 쉽게 변경

### 스프링에서의 IoC & DI
- IoC Container가 Bean을 생성하고 의존성을 주입
- 스프링 IoC Container 패키지 : `org.springframework.beans`, `org.springframework.context`
- **`BeanFactory`** : 기본 기능 제공
- **`ApplicationContext`** : BeanFactory 기능과 더해 엔터프라이즈 특화 기능 추가 제공
- **`Bean`** : 스프링 IoC Conatiner가 관리하는 오브젝트

## [Container Overview](https://docs.spring.io/spring-framework/reference/core/beans/basics.html)
- `org.springframework.context.ApplicationContext` 인터페이스
  - 스프링 IoC Container의 대표
  - 인스턴스화, 구성, 빈들의 조합을 책임짐
  - 메타 데이터 기반 어떤 오브젝트를 빈으로 생성할지 판단.
  - 메타 데이터는 XML, 자바 어노테이션, 자바 코드 포맷 사용 가능
  - Springboot는 어떤 클래스를 사용할까?
    - `AnnotationConfigServletWebServerApplicationContext` : 서블릿 기반 웹 애플리케이션, Tomcat
    - `AnnotationConfigReactiveWebServerApplicationContext` : 리액티브 웹 애플리케이션, Reactor/Netty

## [Bean Overview](https://docs.spring.io/spring-framework/reference/core/beans/definition.html)

## [Reference]
 - Spring doc. The IoC Container : [document url](https://docs.spring.io/spring-framework/reference/core/beans.html)