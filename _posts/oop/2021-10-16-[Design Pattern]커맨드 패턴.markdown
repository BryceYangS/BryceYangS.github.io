---
layout: post
title: "[Design Pattern]커맨드 패턴"
subtitle: "디자인 패턴 커맨드 패턴"
categories: study
tags: oop
---
> 디자인 패턴의 `커맨드 패턴` 정리

# Command

- 메소드 호출을 캡슐화
- 요청 내역을 객체로 캡슐화하여 클라이언트를 서로 다른 요청 내역에 따라 매개변수화할 수 있다. 요청을 큐에 저장하거나 로그로 기록할 수도 있고 작업취소 기능을 지원할 수도
  있다.
- 메타 커맨드 패턴을 이용하면 명령들로 이루어진 매크로를 만들어서 여러 개의 명령을 한 번에 실행할 수 있다.

![command](/assets/img/oop/command.png)

- 커맨드 패턴을 이용하면 `요청을 하는 객체`와 `그 요청을 수행하는 객체`를 `분리`시킬 수 있다.
- 이렇게 분리시키는 과정의 중심에는 커맨드 객체가 있으며, 이 객체가 행동이 들어있는 리시버를 캡슐화한다.
- 인보커에서는 요청을 할 때는 커맨드 객체의 execute() 메소드를 호출하면 됨. execute() 메소드에서는 리시버에 있는 행동을 호출한다.
- 인보커는 커맨드를 통해서 매개변수화될 수 있다. 이런 실행중에 동적으로 설정할 수도 있음.
- execute() 메소드가 마지막으로 호출되기 전의 기존 상태로 되돌리기 위한 작업취소 메소드를 구현하면 커맨드 패턴을 통해서 작업취소 기능을 지원할 수도 있다.
- 매크로 커맨드는 커맨드를 확장해서 여러 개의 커맨드를 한꺼번에 호출할 수 있게 해주는 간단한 방법이다. 매크로 커맨드에서도 어렵지 않게 작업취소 기능을 지원할 수 있다.
- 요청 자체를 리시버한텐 넘기지 않고 자기가 처리하는 "스마트" 커맨드 객체를 사용하는 경우도 종종 있다.
- 커맨드 패턴을 활용해 로그 및 트랜잭션 시스템을 구현하는 것도 가능