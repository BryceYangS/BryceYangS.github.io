---
layout: post
title: "[Spring] IoC(DI) & DL"
subtitle: "IoC(DI), dl"
categories: study
tags: springboot
---
> Spring의 IoC (DI) 및 DL 개념 정리

# IoC (DI)
- IoC
    - 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것   
- DI
    - 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것
    - 의존관계 주입 장점
        - 관심사의 분리를 통해 얻어지는 높은 응집도
        - 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
        - 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다. 


## IoC 용어 정리
- Bean  
    + 스프링 컨테이너에 등록된 객체 : `스프링 빈`
    + 스프링이 IoC 방식으로 관리하는 오브젝트. 스프링이 직접 생성과 제어를 담당하는 오브젝트만을 빈이라고 부름  
    + 스프링 빈은 기본적으로 `싱글톤` 방식으로 관리됨.
        * **스프링 빈 주의사항**
            1. 여러 클라이언트가 하나의 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지하게 설계하면 안됨
            2. ***무상태*** 설계!!! 해야 한다.
            3. 특정 클라이언트에 의존적인 필드가 있으면 안됨
            4. 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안됨
            5. 가급적 읽기만 가능해야 함
            6. 필드 대신에 자바에서 공유되지 않는 `지역변수`, `파라미터`, `ThreadLocal` 등을 사용해야 한다

- Bean Factory
    + 스프링 컨테이너의 최상위 인터페이스
    + 스프링의 IoC를 담당하는 핵심 컨테이너. 빈을 조회, 관리한다.
    + 일반적으로 직접 사용하는 경우 거의 없음

- Application Context : 스프링 컨테이너 
    + Bean Factory를 상속해 확장한 IoC 컨테이너. Bean Factory 기능 + 부가기능을 추가해서 만든 것이다.
    + 부가기능?
        + MessageSource : 메시지소스를 활용한 국제화 기능 - 언어권 별로 다르게 출력
        + EnvironmentCapable : 환경변수 - 로컬, 개발, 운영 등을 구분해서 처리
        + ApplicationEventPublisher : 애플리케이션 이벤트 - 이벤트를 발행하고 구독하는 모델을 편리하게 지원
        + ResourceLoader : 편리한 리소스 조회 - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

- Configuration Metadata  
    + 애플리케이션 컨텍스트가 IoC를 적용하기 위해 사용하는 메타정보

- IoC Container  
    + `객체를 생성하고 관리`하면서 `의존관계를 연결`해 주는 것
    + IoC방식으로 빈을 관리한다는 의미에서 Application Context/Container/IoC 컨테이너 등으로 부른다.


## 싱글톤 컨테이너
- 스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도, 객체 인스턴스를 `싱글톤`으로 관리  
- 스프링 컨테이너는 싱글톤 컨테이너 역할을 수행. → `싱글톤 레지스트리` : 싱글톤 객체를 생성하고 관리하는 기능
- DIP, OCP, 테스트, private 생성자로부터 자유롭게 싱글톤 사용 가능
- 싱글톤 객체를 재사용하기 때문에 사용자의 수많은 요청을 처리하는 데 있어 효율적이다
- 스프링의 기본 빈 등록 방식이 싱글톤이지만, 매번 새로운 객체를 생성해서 반환하는 기능도 제공.

### 싱글톤 방식 주의점
> 무상태 설계  
- 특정 클라이언트에 의존적인 필드가 있으면 안됨
- 특정 클라이언트가 값을 변경할 수 있는 필드 있으면 안됨
- 가급적 읽기만 가능해야 함
- 필드 대신에 자바에서 공유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야 함

## DL (Dependecy Lookup)
런타임 시 의존관계를 맺을 오브젝트를 결정하는 것과 오브젝트의 생성 작업은 외부 컨테이너에게 IoC로 맡기지만, 이를 가져올 때는 메소드나 생성자를 통한 주입 대신 스스로 컨테이너에게 요쳥하는 방법을 사용

### DI와 DL의 차이점
- 의존관계 검색 방식에서는 검색하는 오브젝트는 자신이 스프링의 빈일 필요가 없다