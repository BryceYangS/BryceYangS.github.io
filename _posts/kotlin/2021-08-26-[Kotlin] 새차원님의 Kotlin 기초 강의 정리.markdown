---
layout: post
title: "[Kotlin] 새차원님의 Kotlin 기초 강의 정리"
subtitle: "Kotlin 기초 강의"
categories: study
tags: kotlin
---

> 새차원님의 Kotlin 기초 강의 정리 [새차원 Youtube](https://www.youtube.com/c/%EC%83%88%EC%B0%A8%EC%9B%90/videos)

## 4. Basic Types
### 기본타입  

 - 코틀린에서 모든 것은 객체
    - Primitive & Reference를 구분하는 Java와 다른 점

### 숫자  

 - 기본적으로 자바와 비슷

 - Java에서 숫자형이던 char가 코틀린에서는 숫자형이 아님

 - Underscores in numeric literals (since 1.1) : 숫자형을 읽기 쉽게 표기하는 방법
    ```kotlin
    val oneMillion = 1_000_000
    val creditCardNumber = 1234_5678_9012_3456L
    val socialSecurityNumber = 999_99_9999L
    val hexBytes = 0xFF_EC_DE_5E
    val bytes = 0b11010010_01101001_10010100_10010010
    ```
    
- **Representation**

    - Java 플랫폼에서 숫자형은 JVM primitive type으로 저장됨

    - Nullable이나 제네릭의 경우에는 박싱됨

    - 박싱된 경우 identity를 유지하지 않음

        ```kotlin
        // JVM primitive 
        val a: int = 100
        print(a === a) // true
        
        // Boxed
        val boxedA: Int? = a
        val anotherBoxedA: Int? = a
        println("==: ${boxedA == anotherBoxedA}") // true
        println("===: ${boxedA === anotherBoxedA}") // true
        ```

        

-  Explicit Conversions

    - 작은 타입은 큰 타입의 하위 타입이 아님, 즉 작은 타입에서 큰 타입으로의 대입이 안됨

        ```kotlin
        val a: Int = 1
        //val b: Long = a // 오류
        val b: Long = a.toLong()
        // println(a == b) // 오류
        ```

    - 명시적으로 변환 해줘야 함

        ```kotlin
        val i: Int = b.toInt()
        ```

### 문자

- char는 숫자로 취급되지 않음

### 배열

- 배열은 `Array` 클래스로 표현

- get(), set()

- size 등의 유용한 멤버 함수 포함

  ```kotlin
  var array: Array<String> = arrayOf("코틀린", "강좌")
  println(array.get(0))
  println(array[0])
  println(array.size)
  ```

- 배열 생성

  1. Array의 팩토리 함수 사용
  2. arrayOf() 등의 라이브러리 함수 이용

  ```kotlin
  val b = Array(5, {i -> i.toString()})
  val a = arrayOf("0", "1", "2")
  ```

- 특별한 Array 클래스

  - Primitive 타입의 박싱 오버헤드를 없애기 위한 배열
  - IntArray, ShortArray
  - Array를 상속한 클래스들은 아니지만, Array와 같은 메소드와 프로퍼티를 가짐
  - size 등 유용한 멤버 함수 포함

  ```kotlin
  val x: IntArray = intArrayOf(1, 2, 3)
  x[0] = 7
  println(x.get(0)) // 7
  println(x[0]) // 7
  println(x.size)
  ```

### 문자열

- 문자열은 String 클래스로 표현

- String은 character로 구성

- immutable이므로 변경 불가

- **문자열 리터럴**

  - *escaped string ("Kotlin")*
    - 전통적인 방식으로 Java String과 거의 비슷
    - Backslash를 사용해 escaping 처리
  - raw string("""Kotlin""")
    - escaping 처리 필요 없음
    - 개행이나 어떠한 문자 포함 가능

  ```kotlin
  val s = "Hello, world!\n"
  
  val s = """
  "'이것은 코틀린의
  raw String
  입니다.'"
  """
  ```

  

## 5. Control Flow

- Java랑 상당히 다름

- Kotlin 특징 : When 문, if문이 값을 반환 등

- **if else 문**

  - Java와 거의 유사 
  - if 문이 식으로 사용되는 경우 값을 반환
  - **if 식**의 경우 반드시 `else`를 동반해야

  ```kotlin
  val max = if(a > b) a else b
  ```

  - if 식의 branch들이 블록을 가질 수 있음
  - 블록의 마지막 구문이 반환 값이 됨

  ```kotlin
  val max = if(a > b) {
    print("Choose a")
    a
  } else {
    print("Choose b")
    b
  }
  ```

- 삼항 연산자가 없음 → if 문이 삼항 연산자 역할을 잘 해내고 있기 때문

- **when**

  - when 문은 각각의 branch의 조건문이 만족할 때까지 위에서 부터 순차적으로 인자를 비교
  - Java와 달리 break문이 필요 없음. 만족하는 조건에 대해서만 실행하기 때문

  ```kotlin
  when(x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
      print("x is neither 1 nor 2")
    }
  }
  ```

  - when문이 식으로 사용된 경우에는 조건을 만족하는 branch의 값이 전체 식의 결과 값이 됨
  - *when이 식으로 사용된 경우* `else` 문이 필수
  - *when이 식으로 사용된 경우* 컴파일러가 else문이 없어도 된다는 것을 입증할 수 있는 경우에는 else 생략 가능

  ```kotlin
  var res = when (x) {
  	100 -> "A"
    90 -> "B"
    80 -> "C"
    else -> "F"
  }
  ```

  ```kotlin
  // else 생략 가능
  var res = when (x) {
    true -> "correct"
    false -> "wrong"
  }
  ```

  - *여러 조건들이 같은 방식으로 처리될 수 있는 경우* branch의 조건문에 `콤마(,)`를 사용해 표기

  ```kotlin
  when (x) {
  	0, 1 -> print("x ==0 or x == 1")
    else -> print("otherwise")
  }
  ```

  - branch의 조건문에 함수나 식을 사용할 수 있음

    ```kotlin
    when (x) {
      parseInt(x) -> print("s encodes x")
      1 + 3 -> print("4")
      else -> print("s doen't encode x")
    }
    ```

  - range나 collection에 `in`이나 `!in`으로 범위 검사 가능

    ```kotlin
    val validNumbers = listOf(3,6,9)
    when (x) {
      in validNumbers -> print("x is valid")
      in 1..10 -> print("x is in the range")
      !in 10..20 -> print("x is outside the range")
      else -> print("none of the above")
    }
    ```

  - `is` 나  `!is` 를 이용해 타입도 검사 가능

    - *스마트캐스트* 적용됨

    ```kotlin
    fun hasPrefix(x: Any) = when(x) {
    	is String -> x.startsWith("prefix")
      else -> false
    }
    ```

  - when은 *if*-*else if* 체인 대체 가능

  - when에 인자를 입력하지 않으면, 논리연산으로 처리됨

    ```kotlin
    when {
      x.isOdd() -> print("x is odd")
      x.isEven() -> print("x is even")
      else -> print("x is funny")
    }
    ```

- **For Loop**

  - for문은 iterator를 제공하는 모든 것 반복 가능

    ```kotlin
    for (item in collection)
    	print(item)
    ```

  - For 문을 지원하는 **iteraotr** 의 조건

    - 멤버함수나 확장함수 중에 
      - `iterator()` 를 반환하는 것이 있는 경우
      - `next()` 를 가지는 경우
      - `hasNext()`: Boolean을 가지는 경우
    - 위의 세 함수는 **operator** 로 표기 되어야 함

    ```kotlin
    val myData = MyData()
    for(item in myData) {
      print(item)
    }
    
    class MyData {
      operator fun iterator(): MyIterator {
        return MyIterator()
    	}
    }
    
    class MyIterator {
      val data = listOf(1,2,3,4,5)
      var idx = 0
      
      operator fun hasNext(): Boolean {
        return data.size > idx
      }
      
      operator fun next(): Int {
        return data[idx++]
      }
    }
    ```

  - index 를 이용하고 싶다면 `indices` 를 이용

    ```kotlin
    val array = arrayOf("가", "나", "다")
    for (i in array.indices) {
      println("$i: ${array[i]}")
    }
    ```

    - `withIndex()` 를 이용할 수도 있음

    ```kotlin
    val array = arrayOf("가", "나", "다")
    for ((index, value) in array.withIndex()) {
      println("$index: ${value}")
    }
    ```

- **while Loop**

  - while, do-while문은 Java와 유사

  - *Java와 다른 점* : do-while 문에서 body의 지역변수를 do-while문의 조건문이 참조 가능

    ```kotlin
    do {
      val y = retrieveData()
    } while (y != null) // y is visible here
    ```



## 6. Packages, Return and Jumps



