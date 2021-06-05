---
layout: post
title: "[Java] Primitive & Reference & Boxing & Unboxing"
subtitle: "Primitive & Reference & Boxing & Unboxing"
categories: study
tags: java
---

> Primitive & Reference & Boxing & Unboxing

## 🚀기본형 타입 (Primitive Type)
- 8가지의 기본형 타입 : boolean / byte, short, int, long / float, double / char
- 기본값이 있기 때문에 Null이 존재하지 않음
- 실제 값을 저장하는 공간으로 `스택`메모리에 저장됨
- 담을 수 있는 크기를 벗어나면 컴파일 시점에 에러 발생함

### 💻장점
1. 접근 속도  
원시 타입은 `스택` 메모리에 값이 존재. 반면에 참조 타입은 인스턴스이기 때문에 `스택`메모리에는 참조값만 있고, 실제 값은 `힙`메모리에 존재. 그리고 값을 필요로 할 때마다 언박싱 과정을 거쳐야 하니 원시 타입과 비교해서 접근 속도가 느려지게 됨.  
(예외적으로 엄청 큰 숫자를 복사해야 한다면, 참조값만 넘길 수 있는 참조 타입이 좋을 수도 있음)

2. 차지하는 메모리 양  
참조 타입이 메모리를 더 차지함

## 🚀참조형 타입 (Reference Type)
- 기본형 타입을 제외한 타입
- Null이 존재
- 값이 저장되어 있는 곳의 주소값을 저장하는 공간으로 `힙` 메모리에 저장
- 실제 객체는 힙 메모리에 저장, 참조 타입 변수는 스택 메모리에 실제 객체들의 주소를 저장. 객체를 사용할 때마다 참조 변수에 저장된 객체의 주소를 불러와 사용하는 방식
- 문법상으로는 문제가 없어 컴파일 시점에 에러가 잡히지 않으며, 런타임 시에 에러가 발생한다. (ie. NullPointerException)

## 🚀1.2. 참조
- [https://gbsb.tistory.com/6](https://gbsb.tistory.com/6)
- [https://www.baeldung.com/java-primitives-vs-objects](https://www.baeldung.com/java-primitives-vs-objects)


## 🚀Boxing & Unboxing
- Boxing
    > 원시 타입을 참조 타입으로 변환시키는 것  
- Unboxing
    > 참조 타입을 원시 타입으로 변환시키는 것  

자바 1.5부터는 Auto Boxing, Unboxing 기능을 제공함. Auto Boxing, Unboxing은 **메모리 누수**의 원인이 될 수 있음

### 🚀차이점
1. null을 담을 수 있는가?  
원시 타입은 null을 담을 수 없음. 참조 타입은 null 할당 가능

2. 제네릭 타입에서 사용 가능 여부  
원시 타입은 제네릭 타입에서 사용 불가. 참조 타입은 제네릭 타입에서 사용 가능.


#### Wrapper 끼리 비교

```java
public class WrapperTest {

	public static void main(String[] args) {
		Integer a = new Integer(1);
		Integer b = new Integer(1);

		if (a.equals(b)) {
			System.out.println(a.equals(b)); // true
			System.out.println(a == b); // false
            System.out.println(1 == a); // true
		}
	}
}
```


### 🚀1.3. 참조
- [https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type](https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type)
- [https://siyoon210.tistory.com/139](https://siyoon210.tistory.com/139)
