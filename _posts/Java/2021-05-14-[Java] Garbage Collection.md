---
layout: post
title: "[Java] Garbage Collection"
subtitle: "GC"
categories: study
tags: java
---

> Garbage Collection 정리 글 : 네이버 D2 포스팅 정리.


## 가비지 컬렉션 과정
`stop-the-world`   
- GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것
- stop-the-world가 발생하면 GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다.
- 대부분의 GC 튜닝이란 stop-the-world 시간을 줄이는 것
<br/>
<br/>

JVM은 GC를 통해 자동으로 메모리 관리를 하기 때문에 개발자가 코드를 통해 직접 메모리를 해제 하지 않음. System.gc() 메서드를 자바에서 제공하고 있으나, 이펙티브 자바에서 따르면 개발자가 직접 GC 발생을 일으키는 것을 권장하지 않고 있음. 메모리가 중요한 경우 WeakReference를 사용할 수도 있음.
<br/>
<br/>

가비지 컬렉터가 더이상 사용되지 않는 객체를 찾아 지우는 작업을 수행.  
가비지 컬렉터의 2가지 전제  
- 대부분의 객체는 금방 접근 불가능 상태가 된다.
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

→ 이러한 가설을 `weak generational hypothesis` 라고 한다.  
이러한 가설을 바탕으로 2개의 물리적 공간으로 나뉨. `Young` & `Old`영역.  
1. Young Generation
    - 새롭게 생성한 객체의 대부분이 여기에 위치
    - 대부분의 객체가 금방 접근 불가능한 상태가 되기 때문에 많은 객체가 Young 영역에 생성되었다가 사라짐
    - 이 영역에서 객체가 사라질때 `Minor GC`가 발생
2. Old Generation
    - 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사
    - 대부분 Young 영역보다 크게 할당, Young 영역보다 GC 적게 발생
    - 이 영역에서 객체가 사라질 때 `Major GC` 발생
3. Permanent Generation
    - Method Area
    - 객체나 억류(intern)된 문자열 정보를 저장하는 곳
    - Old 영역에서 살아남은 객체가 영원히 남아 있는 곳 ❌
    - 이 영역에서도 GC가 발생. `Major GC` 횟수에 포함됨
<br/>


## Young 영역의 구성
`Eden` & `Survivor 2개` 로 세 영역으로 구성됨  
처리 절차  
1. 새로 생성한 대부분의 객체는 `Eden 영역`에 위치
2. Eden 영역에서 `GC`가 한 번 발생한 후 살아남은 객체는 `Survivor 영역` 중 하나로 이동됨
3. Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓임
4. 하나의 Survivor 영역이 가득 차게 되면 그 중에서 살아남은 객체를 `다른 Survivor 영역`으로 이동한다. 그리고 가득 찬 Survivor 영역은 아무 데이터도 없는 상태로 됨
5. 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 `Old 영역`으로 이동
<br/>


## Old 영역 GC
### GC 방식 5가지
1. Serial GC
    - Young 영역 GC : 위에 설명한 방식으로 일어남.
    - Old 영역 : `Mark-Sweep-Compact` 알고리즘 사용  
    - Mark-Sweep-Compact 작동 방식
        1. Old 영역에 살아 있는 객체를 식별 - Mark
        2. 힙(Heap)의 앞 부분부터 확인해 살아있는 것만 남김 - Sweep
        3. 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눈다 - Compaction
    - 언제 적합?
        - `적은 메모리`와 `적은 CPU 코어 개수`
2. Parallel GC
    - JDK 1.8 디폴트 GC
    - 기본적으로 Serial GC와 동일 알고리즘.
    - GC를 처리하는 스레드가 하나인 Serial GC와 다른 점은 GC를 처리하는 스레드가 `여러 개`.
    - Serial GC보다 빠르게 객체를 처리
    - 언제 적합?
        - 메모리 충분, 코어의 개수 많을 때 유리
    - Throughput GC 라고도 불림
3. Parallel Old GC
    - JDK 5부터 제공됨
    - Parallel GC와 Old 영역의 GC 알고리즘만 다름
    - `Mark-Summary-Compaction` 단계
        - Summary : 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서 Mark-Sweep-Compaction과 다름. 조금 더 복잡한 단계를 거침
4. Concurrent Mark & Sweep GC (CMS)
    - Initial Mark 단계 : 클래스 로더에 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝냄. 따라서, 멈추는 시간 짧음
    - Concurrent Mark : 방금 살아 있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인. 
        - 다른 스레드가 실행 중인 상태에서 `동시에 진행`된다는 특징
    - Remark : Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체 확인.
    - Concurrent Sweep : 쓰레기를 정리하는 작업
        - 다른 스레드가 실행되고 있는 상황에서 진행됨
    - stop-the-world 시간 매우 짧음
    - 모든 애플리케이션의 `응답 속도가 매우 중요`할 때 사용.
    - Low Latency GC라고도 불림
    - 단점❗️
        - 다른 GC보다 `메모리와 CPU`를 더 많이 사용
        - Compaction 단계가 기본적으로 제공되지 않음
            - 조각난 메모리가 많아 Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 더 길기 때문에 Compaction 작업의 발생 주기 및 횟수를 고려해야함
5. G1 GC
    - JDK 9 ~ 10 디폴트 GC
    - Young, Old 영역을 없앰
    - n개의 영역으로 분리, 각 영역에 객체를 할당하고 GC를 실행. 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행.
    - Young의 세가지 영역, Old 영역으로의 이동 단계가 사라진 GC 방식
    - 장점 : `성능`
        - GC 중 가장 빠름.


### 결론
각 서비스의 WAS에서 생성하는 객체의 크기와 생존 주기가 모두 다르고, 장비의 종류도 다양. 따라서 WAS의 스레드 개수, 장비당 WAS 인스턴스 개수, GC 옵션 등은 지속적인 튜닝과 모니터링을 통해 최적화를 해나가야 함


### [참조]
- 네이버 D2 "Java Garbage Collection" 글 : [https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)

