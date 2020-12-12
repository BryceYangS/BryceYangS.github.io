---
layout: post
title: "[SpringBoot]스프링부트 어노테이션"
subtitle: "스프링부트 어노테이션"
categories: study
tags: springboot
---

## SpringBootApplication 어노테이션

스프링 부트의 핵심 어노테이션
1. @EnableAutoConfiguration
 - 스프링 부트의 설정 자동 완료
2. @ComponentScan
 - 기존 스프링은 빈(bean)을 xml에 선언해줘야 함
 - ComponentScan 어노테이션을 사용할 경우 자동으로 여러 컴포넌트 클래스 검색, 컴포넌트 및 빈 클래스를 스프링 어플리케이션 컨텍스트에 등록

3. @Configuration
 - 포함 관계
    -  @SpringBootApplication   >   @SpringBootConfiguration   >   @Configuration
 - 자바 기반 설정 파일에 추가하는 어노테이션
 - 스프링 4.X 버전 부터 자바 기반 설정 가능