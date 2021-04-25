---
layout: post
title: "[Spring] Spring DB connection & SQL Mapper"
subtitle: "Spring DB connection"
categories: study
tags: springboot
---
> Spring DB connection

# Spring DB connection

## 커넥션 풀 & 히카리CP
Spring boot 2.00 M2 버전부터 기본적으로 사용되는 커넥션 풀이 톰캣에서 히카리CP로 변경됨.  
애플리케이션과 데이터베이스를 연결할 때 이를 효과적으로 관리하기 위해 사용되는 라이브러리.  
상용 WAS를 사용할 경우 일반적으로는 제조사에서 제공되는 커넥션 풀을 사용한다. 오픈소스로는 Common DBCP, Tomcat JDBC, BoneCP, C3P0 등의 라이브러리가 많이 사용된다.  
- 히카리CP 장점
    1. 빠른 속도
    2. 안정성

## MyBatis
SQL Mapper 프레임워크. JDBC를 이용할 경우 개발자가 반복적으로 작성해야 할 코드가 많고, 서비스 로직 코드와 쿼리 분리가 어려워집니다. 
> 마이바티스는 개발자가 지정한 SQL, Stored Procedure 그리고 몇 가지 고급 매핑을 지원하는 퍼시스턴스 프레임워크이다. 마이바티스는 JDBC로 처리하는 상당부분의 코드와 파라미터 설정 및 결과 매핑을 대신해 준다. 마이바티스는 데이터베이스 레코드에 원시타입과 Map 인터페이스 그리고 자바 POJO를 설정해서 매핑하기 위해 XML과 애노테이션을 사용할 수 있다.  
