---
layout: post
title: "[Book] Refactoring"
subtitle: "refactoring"
categories: various
tags: techBook
---

> 마틴 파울러 `Refactoring`

## 2.4 언제 리팩토링?
> 3의 법칙
>
> 1. 처음에는 그냥 한다
> 2. 비슷한 일을 두 번째로 하게 되면, 일단 계속 진행
> 3. 비슷한 일을 세 번째 하게 되면 리팩터링

리팩터링하기 가장 좋은 시점은 코드베이스에 기능을 새로 추가하기 직전

### 리팩터링하지 말아야 할 때

- 굳이 수정할 필요가 때
- 처음부터 새로 작성하는 게 쉬울 때

# Chapter 3. 코드에서 나는 악취

## 3.1 기이한 이름

### 함수 선언 바꾸기 / 변수 이름 바꾸기 / 필드 이름 바꾸기

코드는 단순하고 명료하게 작성.

이름만 보고도 각각이 무슨 일을 하고 어떻게 사용해야 하는지 명확히 알 수 있도록 엄청나게 신경써서 이름을 지어야 한다.



## 3.2 중복 코드

### 함수 추출하기

- 한 클래스에 딸린 두 메서드가 똑같은 표현식 사용하는 경우

### 문장 슬라이드하기

- 코드가 비슷하긴 한데 완전히 똑같지 않은 경우
- 비슷한 부분을 한 곳에 모아 함수 추출하기를 더 쉽게 적용할 수 있는지 살펴봄

### 메서드 올리기

- 같은 부모로부터 파생된 서브 클래스들에 코드 중복된 경우



## 3.3 긴 함수

짧은 함수로 구성된 코드를 이해하기 쉽게 만드는 가장 확실한 방법은 좋은 이름. 

💡함수 이름을 잘 지어두면 본문 코드를 볼 이유가 사라짐

- 함수 이름은 동작 방식이 아닌 `의도`가 드러나게

### 함수 추출하기

- 함수 본문에서 따로 묶어 빼내기

### 임시 변수를 질의 함수로 바꾸기

- 추출 함수에 매개변수가 많을 경우 임시 변수의 수를 줄임

### 매개변수 객체 만들기 / 객체 통째로 넘기기

- 매개변수의 수를 줄임

### 함수를 명령으로 바꾸기

- 위 리팩터링들을 적용해도 여전히 임시 변수와 매개변수가 많을 경우

### 조건문 / 반복문 추출하기

#### 조건문 분해하기

#### 함수 추출하기

- 거대한 switch문을 구성하는 case문마다 함수 추출
- case의 본문을 함수 호출문 하나로 바꿈

#### 조건부 로직을 다형성으로 바꾸기

- 같은 조건을 기준으로 나뉘는 switch문이 여러 개인 경우

### 반복문 쪼개기 



## 3.4 긴 매개 변수 목록

### 매개변수를 질의 함수로 바꾸기

- 다른 매개변수에서 값을 얻어올 수 있는 매개변수가 존재할 경우.  

### 객체 통째로 넘기기

- 사용 중인 데이터 구조에서 값들을 뽑아 각각을 별개의 매개변수로 전달하는 코드

### 매개변수 객체 만들기

- 항상 함께 전달되는 매개변수들 하나로 묶기

### 플래그 인수 제거하기

- 함수의 동작 방식을 정하는 플래그 역할의 매개변수 제거

### 여러 함수를 클래스로 묶기

- 여러 개의 함수가 특정 매개변수들의 값을 공통으로 사용할 때 유용



## 3.5 전역 데이터

### 변수 캡슐화하기



## 3.6 가변 데이터

데이터 변경으로 인한 예상치 못한 결과, 버그 발생하는 경우 발생 가능.

함수형 프로그래밍 : 데이터는 불변. 데이터를 변경하려면 반드시 변경하려는 값에 해당하는 복사본을 만들어서 반환

### 변수 캡슐화하기

- <u>정해놓은 함수</u>를 거쳐야만 값을 수정할 수 있도록 하면 값이 어떻게 수정되는지 감시하거나 코드를 개선하기 쉬움

### 변수 쪼개기

- 하나의 변수에 용도가 다른 값들을 저장하느라 값을 갱신하는 경우
- 용도별로 독립 변수에 저장해 값 갱신이 문제를 일으킬 여지를 없앰

### 문장 슬라이드하기 / 함수 추출하기

- 무언가를 갱신하는 코드로부터 부작용이 없는 코드를 분리

### 질의 함수와 변경 함수 분리하기

### 세터 제거하기

### 파생 변수를 질의 함수로 바꾸기

### 여러 함수를 클래스로 묶기

### 여러 함수를 변환 함수로 묶기

### 참조를 값으로 바꾸기



## 3.7 뒤엉킨 변경

단일 책임 원칙이 제대로 지켜지지 않을 때 나타남. 

### 단계 쪼개기

### 함수 옮기기

- 전체 처리 과정 곳곳에서 각기 다른 맥락의 함수를 호출하는 빈도가 높다면, 각 맥락에 해당하는 적당한 모듈들을 만들어서 관련 함수를 모음

### 함수 추출하기

- 여러 맥락의 일에 관여하는 함수가 있다면 옮기기 전에 함수 추출하기 선행

### 클래스 추출하기



## 3.8 산탄총 수술

코드를 변경할 때마다 자잘하게 수정해야 하는 클래스가 많은 경우.

CF. 뒤엉킨 변경
<table>
<thead>
  <tr>
    <th></th>
    <th>뒤엉킨 변경</th>
    <th>산탄총 수술</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>원인</td>
    <td colspan="2">맥락을 잘 구분하지 못함</td>
  </tr>
  <tr>
    <td>해법(원리)</td>
    <td colspan="2">맥락을 명확히 구분</td>
  </tr>
  <tr>
    <td>발생 과정(현상)</td>
    <td>한 코드에 섞여 들어감</td>
    <td>여러 코드에 흩뿌려짐</td>
  </tr>
  <tr>
    <td>해법(실제 행동)</td>
    <td>맥락 별로 분리</td>
    <td>맥락별로 모음</td>
  </tr>
</tbody>
</table>
### 함수 옮기기 / 필드 옮기기

- 함께 변경되는 대상들을 한 모듈에 묶어두기

### 여러 함수를 클래스로 묶기

- 비슷한 데이터를 다루는 함수가 많은 경우

### 여러 함수를 변환 함수로 묶기

- 데이터 구조를 변환하거나 보강하는 함수

### 함수 인라인하기 / 클래스 인라인하기

- 어설프게 분리된 로직에 적용



## 3.9 기능 편애

어떤 함수가 자기가 속한 모듈의 함수나 데이터보다 다른 모듈의 함수나 데이터와 상호작용 할 일이 더 많을 때.

### 함수 옮기기

### 함수 추출하기

- 함수의 일부에서만 기능을 편애할 경우 그 부분만 독립 함수로 분리한 후 원하는 모듈로 옮김. 

**어디로 옮길지 명확하게 드러나지 않을 경우**

- 가장 많은 데이터를 포함한 모듈로 옮김
- 또는, 함수를 여러 조각으로 나눈 후 각각을 적합한 모듈로 옮김



## 3.10 데이터 뭉치

### 클래스 추출하기

- 필드 형태의 데이터 뭉치를 찾아 하나의 객체로 묶음

### 매개변수 객체 만들기 / 객체 통째로 넘기기

- 매개변수 수 ↓



## 3.11 기본형 집착

### 기본형을 객체로 바꾸기

- 의미 있는 자료형들로 변형 가능

### 타입 코드를 서브클래스로 바꾸기 → 조건부 로직을 다형성으로 바꾸기

- 기본형으로 표현된 코드가 조건부 동작을 제어하는 타입 코드로 쓰인 경우

### 클래스 추출하기 / 매개변수 객체 만들기

- 자주 함께 몰려다니는 기본형 그룹



## 3.12 반복되는 switch문

똑같은 조건부 로직 (switch/case문, 길게 나열된 if/else문)이 여러 곳에서 반복해 등장하는 코드에 집중!

중복된 switch문이 문제가 되는 이유는 조건절을 하나 추가할 때마다 다른 switch문들도 모두 찾아서 함께 수정해야 하기 때문.

### 조건부 로직을 다형성으로 바꾸기



## 3.13 반복문

### 반복문을 파이프라인으로 바꾸기



## 3.14 성의 없는 요소

역할이 줄어든 요소. 본문 코드를 그대로 쓰는 것과 진배없는 함수 등. 굳이 분리할 필요가 없는 요소들(함수, 클래스, 인터페이스 등) 정리

### 함수 인라인하기 / 클래스 인라인하기

### 계층 합치기



## 3.15 추측성 일반화

당장은 필요 없는 모든 종류의 후킹 포인트와 특이 케이스 처리 로직을 작성해둔 코드에서 풍기는 냄새

→ 당장 걸리적거리는 코드는 눈앞에서 치워버려라

### 계층 합치기

- 하는 일이 거의 없는 추상 클래스 제거

### 함수 인라인하기 / 클래스 인라인하기

- 쓸데없이 위임하는 경우

### 함수 선언 바꾸기

- 본문에서 사용되지 않는 매개변수 제거



## 3.16 임시 필드

특정 상황에서만 값이 설정되는 필드를 가진 클래스 존재. 혼란 야기

### 클래스 추출하기 → 함수 옮기기

### 특이 케이스 추가하기

- 임시 필드들이 유효한지를 확인한 후 동작하는 조건부 로직이 있는 경우.
- 필드들이 유효하지 않을 때를 위한 대안 클래스를 만들어서 제거 



## 3.17 메시지 체인

메시지 체인 : 클라이언트가 한 객체를 통해 다른 객체를 얻은 뒤 방금 얻은 객체에 또 다른 객체를 요청하는 식. <u>다른 객체를 요청하는 작업이 연쇄적으로 이어지는 코드</u>.

### 위임 숨기기

메서드 체인이 무조건 나쁜건 아닌듯..? 어디까지 드러내고 어디까지 숨길 것인가에 따라 적용 수준이 다른 듯



## 3.18 중개자

위임이 지나치면 문제가 됨. 클래스가 제공하는 메서드 중 절반이 다른 클래스에 구현을 위임하고 있다면?? 

### 중개자 제거하기

- 실제로 일을 하는 객체와 직접 소통



## 3.19 내부자 거래

### 함수 옮기기 / 필드 옮기기

### 위임 숨기기

- 다른 모듈이 중간자 역할을 하도록

### 서브클래스를 위임으로 바꾸기 / 슈퍼클래스를 위임으로 바꾸기



## 3.20 거대한 클래스

한 클래스가 너무 많은 일을 하는 경우 : 필드 수가 늘어남. → 중복 코드 발생 가능성 높음

### 클래스 추출하기

- 필드들 일부를 따로 묶음

### 슈퍼클래스 추출하기

- 분리할 컴포넌트를 원래 클래스와 상속 관계로 만드는게 좋은 경우

### 타입 코드를 서브클래스로 바꾸기
