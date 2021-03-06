---
layout: post
title: "[Effective Java] 9장 일반적인 프로그래밍 원칙"
subtitle: "effective java 9장"
categories: various
tags: techBook
---
> Effective Java 9장. 일반적인 프로그래밍 원칙

- 지역변수, 제어구조, 라이브러리, 데이터 타입, 리플렉션, 네이티브 메서드, 최적화, 명명규칙

## item57. 지역변수의 범위를 최소화
- 지역 변수의 범위를 줄이는 가장 강력한 기법은 **가장 처음 쓰일 때 선언하기**다
- 거의 모든 지역변수는 선언과 동시에 초기화해야 한다.
- while문보다 for문을 사용하라
    - while 문에서 사용된 변수가 while 외부에서도 사용될 경우 의도치 않은 버그를 발생시킬 수 있음
- 메서드를 작게 유지하고 한 가지 기능에 집중

## item58. 전통적인 for 문보다는 for-each문을 사용하라
for-each문을 사용할 경우 for문보다 에러 발생 가능성을 낮출 수 있음  
- for-each문 사용할 수 없는 상황 3가지
    1. 파괴적인 필터링 : 컬렉션을 순회하면서 선택된 원소를 제거해야 한다면 반복자의 remove 메서드를 호출해야 한다. 자바 8부터는 Collection의 removeIf메서드를 사용해 컬렉션을 명시적으로 순회하는 일을 피할 수 있다
    2. 변형 : 리스트나 배열을 순회하면서 그 원소의 값 일부 혹은 전체를 교체해야 한다면 리스트의 반복자나 배열의 인덱스를 사용해야 한다.
    3. 병렬 반복 : 여러 켈렉션을 병렬로 순회해야 한다면 각각의 반복자와 인덱스 변수를 사용해 엄격하고 명시적으로 제어해야 한다.

**핵심정리**  
전통적인 for 문과 비교했을 때 for-each문은 명료하고, 유연하고, 버그를 예방해준다. 성능 저하도 없다. 가능한 모든 곳에서 for 문이 아닌 for-each 문을 사용하자

## item59. 라이브러리를 익히고 사용하라
- 자바 프로그래머라면 적어도 `java.lang`, `java.util`, `java.io`와 그 하위 패키지들에는 익숙해져야 한다
- 컬렉션 프레임워크, 스트림 라이브러리
- java.util.concurrent : 동시성

> 우선 라이브러리 사용을 시도 → 고품질의 서드파티 라이브러리(ie. 구아바) 사용 → 직접 구현  

## item60. 정확한 답이 필요하다면 float와 double은 피하라
실수형 float과 double을 사용할 경우 의도치 않은 결과가 나올 수 있다.  
**금융 계산에는 BigDecimal, int 혹은 long을 사용해야 한다.**  

### 🚀 BigDecimal의 단점 두 가지
기본 타입보다 쓰기가 훨씬 불편하고 느림.  

### 🚀 핵심정리
코딩 시의 불편함이나 성능 저하를 신경 쓰지 않겠다면 BigDecimal을 사용하라.  
성능이 중요하고 소수점을 직접 추적할 수 있고 숫자가 너무 크지 않다면 int나 long을 사용하라.  
~ 9자리 십진수 표현 시 int 사용  
~ 18자리 십진수 표현 시 long 사용  
18자리  BigDecimal 사용  


## item61. 박싱된 기본 타입보다는 기본 타입을 사용하라


## item62. 다른 타입이 적절하다면 문자열 사용을 피하라

## item63. 문자열 연결은 느리니 주의하라

## item64. 객체는 인터페이스를 사용해 참조하라

## item65. 리플렉션보다는 인터페이스를 사용하라

## item66. 네이티브 메서드는 신중히 사용하라

## item67. 최적화는 신중히 하라

## item68. 일반적으로 통용되는 명명 규칙을 따르라