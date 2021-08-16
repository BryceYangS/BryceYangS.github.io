---
layout: post
title: "[Java] JVM Stack & Heap"
subtitle: "JVM Stack & Heap"
categories: study
tags: java
---

> JVM Stack 메모리 & Heap 공간

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

### 정리

![jvm](/assets/img/java/jvm.png)


Method Area, Class Area  
인스턴스가 생성이 안되었어도 접근이 가능한 `static` 관련 **Class Area**에 저장이 됨  
`new` 키워드로 인스턴스를 생성하면 **Heap**에 저장 됨 → 클래스를 만들 때 정보가 필요한데 *Class Area*에 저장된 메타데이터를 활용해서 객체를 생성함  
쓰레드 마다 스택이 할당되고 로컬은 스택에 저장이 됨

### 🚀1.4. 참조
- [https://www.baeldung.com/java-stack-heap](https://www.baeldung.com/java-stack-heap)
