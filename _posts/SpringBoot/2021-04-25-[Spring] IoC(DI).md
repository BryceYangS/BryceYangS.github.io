---
layout: post
title: "[Spring] IoC(DI)"
subtitle: "IoC(DI)"
categories: study
tags: springboot
---
> Spring의 IoC (DI) 개념 정리

# IoC (DI)

## IoC 용어 정리
- Bean  
스프링이 IoC 방식으로 관리하는 오브젝트. 스프링이 직접 생성과 제어를 담당하는 오브젝트만을 빈이라고 부름

- Bean Factory  
스프링의 IoC를 담당하는 핵심 컨테이너. 빈을 등록, 생성, 조회, 관리한다.

- Application Context  
빈 팩토리를 확장한 IoC 컨테이너. 빈 팩토리에다 스프링의 부가기능을 추가해서 만든 것이다.

- Configuration Metadata  
애플리케이션 컨텍스트가 IoC를 적용하기 위해 사용하는 메타정보

- IoC Container  
IoC방식으로 빈을 관리한다는 의미에서 Application Context/Container/IoC 컨테이너 등으로 부른다.