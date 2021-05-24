---
layout: post
title: "[Interview] 면접 공부"
subtitle: "interview"
categories: study
tags: etc
---
> 면접 공부  

# 1. Java
## 1.1. HashMap & TreeMap
[[Java] HashMap vs TreeMap](/study/2021/05/24/Java-HashMap-vs-TreeMap)

## 1.2. primitive & reference
### 🚀기본형 타입 (Primitive Type)
- 8가지의 기본형 타입 : boolean / byte, short, int, long / float, double / char
- 기본값이 있기 때문에 Null이 존재하지 않음
- 실제 값을 저장하는 공간으로 `스택`메모리에 저장됨
- 담을 수 있는 크기를 벗어나면 컴파일 시점에 에러 발생함

#### 💻장점
1. 접근 속도  
원시 타입은 `스택` 메모리에 값이 존재. 반면에 참조 타입은 인스턴스이기 때문에 `스택`메모리에는 참조값만 있고, 실제 값은 `힙`메모리에 존재. 그리고 값을 필요로 할 때마다 언박싱 과정을 거쳐야 하니 원시 타입과 비교해서 접근 속도가 느려지게 됨.  
(예외적으로 엄청 큰 숫자를 복사해야 한다면, 참조값만 넘길 수 있는 참조 타입이 좋을 수도 있음)

2. 차지하는 메모리 양  
참조 타입이 메모리를 더 차지함

### 🚀참조형 타입 (Reference Type)
- 기본형 타입을 제외한 타입
- Null이 존재
- 값이 저장되어 있는 곳의 주소값을 저장하는 공간으로 `힙` 메모리에 저장
- 실제 객체는 힙 메모리에 저장, 참조 타입 변수는 스택 메모리에 실제 객체들의 주소를 저장. 객체를 사용할 때마다 참조 변수에 저장된 객체의 주소를 불러와 사용하는 방식
- 문법상으로는 문제가 없어 컴파일 시점에 에러가 잡히지 않으며, 런타임 시에 에러가 발생한다. (ie. NullPointerException)

### 🚀1.2. 참조
- [https://gbsb.tistory.com/6](https://gbsb.tistory.com/6)
- [https://www.baeldung.com/java-primitives-vs-objects](https://www.baeldung.com/java-primitives-vs-objects)

## 1.3. Boxing & Unboxing
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



### 🚀1.3. 참조
- [https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type](https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type)
- [https://siyoon210.tistory.com/139](https://siyoon210.tistory.com/139)


## 1.4. JVM 메모리
### 🚀자바의 스택 메모리 & 힙 공간
#### 💻스택 메모리
Java의 스택 메모리는 **정적 메모리 할당** 및 **스레드 실행에 사용**됩니다. 여기에는 메서드에 고유한 기본 값과 메서드에서 참조되는 힙에 있는 객체에 대한 참조가 포합됩니다.  
이 메모리에 대한 액세스는 `LIFO` 순서. 새 메서드가 호출될 때마다 스택 상단에 기본 변수 및 객체에 대한 참조와 같이 해당 메서드에 특정한 값을 포함하는 새 블록이 생성됩니다.  
메서드가 실행을 마치면 해당 스택 프레임이 플러시되고 흐름이 호출 메서드로 돌아가고 다음 메서드에 공간을 사용할 수 있게 됩니다.  

##### 💻스택 메모리 주요기능
- 새로운 메서드가 각각 호출되고 반환됨에 따라 확장 및 축소됩니다.
- 스택 내부의 변수는 변수를 생성한 *메서드가 실행되는 동안에만 존재*합니다.
- 메서드 실행이 완료되면 *자동으로 할당되고 할당 해제*됩니다.
- 이 메모리가 가득 차면 *java.lang.StackOverFlowError*를 발생시킵니다.
- 이 메모리에 대한 액세스는 *힙 메모리에 비해 빠릅*니다.
- 이 메모리는 각 스레드가 자체 스택에서 작동하므로 *스레드로부터 안전*합니다.

#### 💻힙 Space
**Java의 힙 공간은 런타임시 Java 객체 및 JRE 클래스에 대한 동적 메모리 할당에 사용**됩니다. 새 객체는 항상 힙 공간에 생성되며 이 객체에 대한 참조는 스택 메모리에 저장됩니다.  
이러한 객체는 전역 액세스 권한이 있으며 응용 프로그램의 어느 곳에서나 액세스 할 수 있습니다.  

자바 힙 공간 세 부분  
    1. Young Generation : 모든 새로운 객체가 할당되고 노화되는 곳.  
    2. Old Generation : 오래 살아남은 객체가 저장되는 곳.  
    3. Permanent Generation : 런타임 클래스 및 애플리케이션 메서드에 대한 JVM 메타 데이터로 구성됩니다.  

##### 💻힙 메모리 주요 기능
- Young, Old, Permanent Generation을 포함한 복잡한 메모리 관리 기술을 통해 액세스
- 힙 공간이 가득 차면 Java에서 *java.lang.OutOfMemoryError*가 발생합니다.
- 이 메모리에 대한 액세스는 *스택 메모리보다 상대적으로 느립*니다.
- 스택과 달리 메모리는 *자동으로 할당 해제되지 않습니다*. 메모리 사용의 효율성을 유지하기 위해 사용하지 않는 객체를 확보하려면 가비지 콜렉터가 필요합니다.
- 스택과 달리 힙은 *스레드로부터 안전하지 않으며* 코드를 적절하게 *동기화*해 보호해야 합니다.

#### 💻예

```java
class Person {
    int id;
    String name;

    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

public class PersonBuilder {
    private static Person buildPerson(int id, String name) {
        return new Person(id, name);
    }

    public static void main(String[] args) {
        int id = 23;
        String name = "Bryce";
        Person person = null;
        person = buildPerson(id, name);
    }
}
```  

#### 💻단계별 분석
1. main() 메서드에 들어가면 스택 메모리에 이 메서드의 기본 요소와 참조를 저장하기 위해 공간이 생성됨
    - int id의 원시 값은 스택 메모리에 직접 저장됨
    - Person 유형의 참조 변수 person도 힙의 실제 객체를 가리키는 스택 메모리에 생성됨
2. main() 에서 매개 변수화 된 생성자 Person(int,String)에 대한 호출은 이전 스택 위에 추가 메모리를 할당. 다음을 저장
    - 스택 메모리에서 호출 객체의 this 객체 참조
    - 스택 메모리의 기본 값 id
    - 힙 메모리의 문자열 풀에서 실제 문자열을 가리키는 문자열 인수 name의 참조 변수
3. main() 메서드는 buildPerson() 정적 메서드를 호출한다. 
4. 하지만, 새로 생성된 객체에 대한 Person 타입의 person, 모든 인스턴스 변수는 힙 메모리에 저장된다.

![java-heap-stack-diagram](/assets/img/etc/java-heap-stack-diagram.webp)

||스택 메모리|힙 Space|
|--|--|--|
|Application            |스택은 스레드 실행 중에 한 번에 하나씩 부분적으로 사용됨|전체 애플리케이션은 런타임 동안 힙 공간을 사용|
|Size                   |스택은 OS에 따라 크기 제한이 있으며, 일반적으로 힙보다 작음|사이즈 제한 없음|
|Storage                |힙 공간에서 생성된 객체에 대한 참조와 기본 변수만 저장|새로 생성된 모든 객체가 저장됨|
|Order                  |LIFO 메모리 할당 시스템 사용해 액세스|Young/Old/Permanent Generation을 포함하는 복잡한 메모리 관리 기술을 통해 액세스|
|Life                   |현재 메서드가 실행되는 동안에만 존재|애플리케이션이 실행되는 동안 존재|
|Efficieny              |힙에 비해 상대적으로 훨씬 빠른 할당|스택에 비해 할당 속도 느림|
|Allocation/Deallocation|이 메모리는 메서드가 각각 호출되고 반환될 때 자동을 할당 및 할당 해제|새 객체가 생성될 때 힙 공간이 할당 / 더 이상 참조되지 않는 객체에 대해 가비지 콜렉터에 의해 할당 해제|



### 🚀1.4. 참조
- [https://www.baeldung.com/java-stack-heap](https://www.baeldung.com/java-stack-heap)

## 1.5. Generic
> 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해주는 기능  

### 🚀Generics의 장점
 - 타입 안정성 제공
 - 타입 체크와 형변환의 생략 -> 코드 간결해짐.

### 🚀타입 안정성?
- 의도하지 않은 타입의 객체 저장 막음
- 저장된 객체 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄일 수 있음


 즉, Generics는 **다룰 객체의 타입을 미리 명시해줌으로써 번거로운 형변환을 줄여주는 것**이다.


# 2. HTTP
## 2.1. POST & PUT 동일 요청 반복 경우 어떠한 결과가 나오는가

### 🚀idempotent?
> f(x) = f(f(x))  

우리나라 말로 멱등법칙 또는 멱등성이라 한다. 연상을 여러 번 적용하더라도 결과가 달라지지 않는 성질을 의미한다.  
POST와 PUT의 경우 멱등성에 있어 차이를 보인다. POST는 idempotent하지 않다.  

### 🚀POST
리소스 생성에 사용되는 HTTP 메서드.  
반복해서 요청할 경우 서로 다른 id를 가진 리소스를 생성한다. 즉, **idempotent 하지 않다**. 

### 🚀PUT
특정 리소스에 대한 수정을 요청할 때 사용되는 HTTP 메서드.  
반복해서 요청을 하더라도 동일한 결과를 보장한다. 즉, **idempotent** 하다.  

### 🚀2.1. 참조
- [http://1ambda.github.io/javascripts/rest-api-put-vs-post/](http://1ambda.github.io/javascripts/rest-api-put-vs-post/)


## 2.2. Redirection
### 🚀3xx (Redirection)
> 요청을 완료하기 위해 유저 에이전트(웹 브라우저)의 추가조치 필요  

웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동  

### 🚀Redirection 종류 3가지
#### 💻1. 영구 리다이렉션
- 특정 리소스의 URI가 영구적으로 이동
- 원래의 URL을 사용❌, 검색 엔진 등에서도 변경 인지
- HTTP 상태 코드
    1. 301 Moved Permanently
        - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음
    2. 308 Permanent Redirect
        - 301과 기능 동일
        - 리다이렉트시 요청 메서드와 본문 유지
- ie) /members → /users
- ie) /event → /new-event

#### 💻2. 일시 리다이렉션
일시적인 변경  
- 리소스의 URI가 일시적으로 변경
- 검색 엔진 등에서 URL을 변경하면 안됨
- HTTP 상태 코드
    1. 302 Found
        - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음
    2. 307 Temporary Redirect
        - 302와 기능 동일
        - 리다이렉트시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다)
    3. 303 See Other
        - 302와 기능 동일
        - 리다이렉트시 요청 메서드가 GET으로 변경
- PRG : Post/Redirect/Get
    - POST로 주문 후에 새로 고침으로 인한 중복 주문 방지
    - POST로 주문 후에 주문 결과 화면을 GET 메서드로 리다이렉트
    - 새로고침해도 결과 화면을 GET으로 조회
    - 중복 주문 대신 결과 화면만 GET으로 다시 요청
- 302 ? 307 ? 303 ? 어떤 것 사용?
    - 302 : GET으로 변경 될 수 있음
    - 307 : 메서드가 변하면 안 됨
    - 303 : 메서드가 GET으로 변경
    - 원칙
        - 302 스펙의 의도는 HTT 메서드 유지
        - 웹 브라우저 대부분 GET으로 변경
        - 모호한 302 대신 307, 303 등장
    - 현실
        - 구체적인 307, 303 사용을 권장하나 많은 애플리케이션 라이브러리들이 302 기본 사용
        - 자동 리다이렉션시 GET으로 변해도 되면 302 사용해도 큰 문제 ❌

#### 💻3. 특수 리다이렉션
결과 대신 `캐시`를 사용  
- HTTP 상태 코드
    1. 300 Multiple Choices
        - 사용 ❌
    2. 304 Not Modified
        - 캐시를 목적으로 사용
        - 클라이언트에게 리소스가 수정되지 않았음을 알려줌. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용(캐시로 리다이렉트)
        - 304 응답은 응답에 메시지 바디를 포함하면 ❌ (로컬 캐시 사용해야 하기 때문)
        - 조건부 GET, HEAD 요청시 사용

### 🚀2.2.참조
- [https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections](https://developer.mozilla.org/ko/docs/Web/HTTP/Redirections) 
- 인프런, 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#)


# 3. 네트워크
## 3.1. Load-Balancer
> 여러 대의 서버가 분산 처리할 수 있도록 요청을 나누어주는 서비스  

클라이언트가 증가해 응답해야 하는 서버에 과부하가 걸리는 현상이 발생. 서버가 멈추는 경우가 발생하면 안되기 때문에 서버 증가가 필요하게 됨.  

### 🚀서버 성능 증가 방법
1. Scale-Up : 서버가 더 빠르게 동작하기 위해 하드웨어 성능을 올림
2. Scale-Out : 서버의 개수를 늘려 여러 서버가 요청을 나눠서 처리


### 🚀Load Balancer 종류
OSI 7 Layer 기준으로 어떤 것을 나누는지에 따라 다름  

#### 💻1. L2
Mac 주소 기준 Load Balancing  

#### 💻2. L3
IP 주소 기준  

#### 💻3. L4
Transport Layer (IP & Port) 레벨에서 Load Balancing
서버 A, 서버 B로 로드 밸런싱  
IP와 Port정보를 보고 정해진 정책에 따라 라우팅  


#### 💻4. L7
Application Layer(User Request) 레벨에서 Load Balancing (HTTPS, HTTP, FTP)  
IP와 Port정보 뿐만 아니라 패킷의 URL 정보, 쿠키, payload 정보들을 보고 정해진 정책에 따라 라우팅  
하위 로드밸런서보다 세부적인 로드 밸런싱이 가능  
마이크로서비스 간의 통신은 보통 HTTP 사용. 그래서 HTTP 기반 로드 밸런서는 이러한 통신을 쉽게 처리할 수 있다는 장점도 있음  
하지만, 패킷 내용을 복호화하여 처리를 해야하기 때문에 부하가 많이 걸릴 수 있음.  

### 🚀3.1. 참조
- [10분 테코톡] 🐿 제이미의 Forward Proxy, Reverse Proxy, Load Balancer : [https://www.youtube.com/watch?v=YxwYhenZ3BE](https://www.youtube.com/watch?v=YxwYhenZ3BE)
- [https://peemangit.tistory.com/197](https://peemangit.tistory.com/197)
- [https://gruuuuu.github.io/network/lb01/](https://gruuuuu.github.io/network/lb01/)

## 3.2. TCP vs UDP
### 🚀TCP
전송 제어 프로토콜 (Transmission Control Protocol)  
1. 연결지향 - TCP 3 way handshake
    1) Client - SYN : 접속 요청
    2) Server - SYN + ACK : 요청 수락
    3) Client - ACK : ACK와 함께 데이터 전송도 가능. 즉, 4단계 생략 가능
    4) Client - 데이터 전송
2. 데이터 전달 보증
3. 순서 보장

#### 💻TCP 신뢰성을 유지할 수 있는 방법
##### 1. 잘 받았으면 ACK, 못 받았으면 NAK
오류가 날 경우 보냈던 TCP Segment를 다시 보내주게 된다. 이를 위해 수신자는 잘 받았다면 ACK(Positive Acknowledge)를, 중간에 오류가 났다면 NAK(Negative Acknowledge)를 송신자에게 보내주게 된다. 이를 통해 송신자는 다시 TCP Segment를 보낼지 말지 결정하게 된다.

##### 2. 어떻게 수신자는 TCP Segment에 오류가 있는지 알 수 있는가?
TCP Segment의 Header 부분

### 🚀UDP
사용자 데이터그램 프로토콜(User Datagram Protocol)  
- 연결 지향 - TCP 3 way handshake❌
- 데이터 전달 보증 ❌
- 순서 보장 ❌
- 단순하고 빠름
- IP 통신과 거의 같음 + 알파 : Port, 체크섬 기능 정도만 추가됨

### 🚀3.2. 참조
- [https://wjdtn7823.tistory.com/37](https://wjdtn7823.tistory.com/37)
- 인프런, 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC#)

# 4. 데이터베이스
## 4.1. 복합키

## 4.2. Sharding & Replication

## 4.3. PreparedStatement & Statement

### 🚀Statement
1. Statement 객체는 Connection 클래스의 createStatement() 메서드 호출을 통해 생성.
2. Statement 객체가 생성되면 executeQuery() 메서드를 호출해 SQL문을 실행시킬 수 있다. 메서드의 인수로 SQL문을 담은 String 객체를 전달한다.
3. Statement는 정적인 쿼리문을 처리할 수 있다. 즉 쿼리문에 값이 미리 입려되어 있어야 한다.

### 🚀PreparedStatement
1. PreparedStatement 객체는 Connection 객체의 preparedStatement() 메서드를 사용해서 생성. 메서드 인수로 SQL문을 담은 String 객체가 필요
2. SQL 문장이 미리 컴파일되고, 실행 시간동안 인수값을 위한 공간을 확보할 수 있다는 점에서 Statement 객체와 다름
3. Statement 객체의 SQL은 실행될 때 매 번 서버에서 분석해야 하는 반면, PreparedStatement 객체는 한 번 분석되면 재사용 용이
4. 각각의 인수에 대해 위치홀더(placeholder)를 사용해 SQL 문장을 정의할 수 있게 해준다. 위치홀더는 ? 로 표현됨
5. 동일한 SQL문을 특정 값만 바꾸어서 여러 번 실행해야 할 때, 인수가 많아서 SQL문을 정리해야 될 필요가 있을 때 사용하면 유용


![prepared-statement-performance-1](/assets/img/etc/prepared-statement-performance-1.png)

- PreparedStatement의 재사용 수준은 두 가지
    1. JDBC 드라이버에 의한 PreparedStatement 재사용
    2. DB에 의한 PreparedStatement 재사용

### 🚀어떤 것을 사용해야 하는가?
Statement의 경우 매 번 쿼리에 대한 컴파일을 수행한다. PreparedStatement의 경우 미리 컴파일된 쿼리를 사용하기 때문에 수행 속도가 더 빠르다는 장점이 있다.  
SQL Injection 을 막고, 쿼리문 재사용을 통한 성능 상의 이점을 위해 PreparedStatement를 사용한다.  
무조건 PreparedStatement를 사용할 경우 DB에 캐싱된 SQL문이 과도해질 수 있다. 이는 성능을 낮출 수 있으니, 자주 사용되는 구문만 PreparedStatement로 하는 것이 좋은 경우도 있다.

### 4.3. 참조
- [http://tutorials.jenkov.com/jdbc/preparedstatement.html](http://tutorials.jenkov.com/jdbc/preparedstatement.html)
- [https://devbox.tistory.com/entry/Comporison](https://devbox.tistory.com/entry/Comporison)

# 5. 스프링
## 5.1. 커넥션 풀
## 5.2. @Transactional
## 5.3. 빈 스코프

# 6. 디자인 패턴
## 6.1. 전략패턴
## 6.2. 옵저버 패턴

# 7. 동시성
## 7.1. 프로세스 작동 방식 & 쓰레드 작동 방식

# 8. OS
## 8.1. Linux 메모리 꽉 찰 경우 발생하는 현상