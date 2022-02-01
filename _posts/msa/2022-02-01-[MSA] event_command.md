---
layout: post
title: "[MSA] Event & Command"
subtitle: "Event & Command"
categories: study
tags: msa
---

> Event & Command


# 1. Event & Command

> 핵심 차이점 : `책임`과 `추상화 수준`

| Event                                                        | Command                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 메시지                                                       | 메시지                                                       |
| 과거에 일어난 일                                             | 과거에 일어난 일 X. <br />요청. 조건에 따라 거부될 수  있음  |
| 많은 시스템, 마이크로서비스가 이벤트에 관심을 가질 수 있으므로,<br/>이벤트는 `여러 번 처리`될 수 있음 | 단일 수신기에서 `한 번`만 처리. <br/>명령= 애플리케이션에서 수행하려는 단일 작업 또는 트랜잭션 |
| publish ⭕️                                                    | publish ❌                                                    |
| Domain                                                       | DTO : 명령을 수행하기 위해 필요한 데이터                     |



## 1.1 Event

- 과거에 일어난 일, 되돌릴 수 없는 것
- 발신자는 수신자에게 어떻게 반응할지 알려주지 않고 알림만 제공
- 일반적으로 다수의 Consumer가 있음
- 도메인 상태 변경

![](/assets/img/msa/event_command/event.gif)



## 1.2 Command

- 무언가 행해지기를 바람을 의미. 해당 행위는 거부될 수 있음
- 다른 응용 프로그램에서 제공하는 기능을 호출해야 할 때 사용
- 발신자가 호출하려는 수신자의 기능이나 메소드를 지정
- 한 대상에만 지정됨

![](/assets/img/msa/event_command/command.gif)

## 1.3 Command Handler

### 1.3.1 Mediator 패턴 (in a Single microservice)

- 로깅, 유효성 검사, audit, 보안 등과 같은 추가 동작 등의 수단 제공

![mediator-cqrs-microservice](/assets/img/msa/event_command/mediator-cqrs-microservice.png)

### 1.3.2 메시지 큐 활용

- 비동기식 Command가 실제 유용 & 사용되는가???

  - 출처 : https://groups.google.com/g/dddcqrs/c/xhJHVxDx2pM/m/WP9qP8ifYCwJ

  ```
  Burtsev Alexey
  읽지 않음,
  2012. 5. 9. 오후 7:56:00
  받는사람 ddd...@googlegroups.com
  I find lot's of code where people use async command handling or one way command messaging without any reason to do so (they are not doing some long operation, they are not executing external async code, they do not even cross application boundary to be using message bus). Why do they introduce this unnecessary complexity? And actually I haven't seen a CQRS code example with blocking command handlers so far, thought it will work just fine in most cases.
  
  Oddly I find this especially often in ASP .NET WEB environment, where people begin to use AJAX, e-mail, or any other back channel to inform client on command result.
  As a fresh example of this is Microsoft Patterns & Practices CQRS Journey, where they use async command handling, and then use ugly while(NotDone) blocking loop in page controller.
  ```

  ```
  Yevhen Bobrov
  읽지 않음,
  2012. 5. 9. 오후 9:51:13
  받는사람 ddd...@googlegroups.com
  Alexey,
  
  You're absolutely right. I see this everywhere. Similar abuse happens with Sagas.
  If you don't really need command to be executed asynchronously - don't do it.
  
  In our app we execute all user-created commands synchronously.
  The only commands which got executed asynchronously are system-generated commands.
  
  In fact I don't believe that there is such thing as asynchronous command at all, because the sending agent is usually interested in the result of its execution.
  So when the command is created as a result of user initiated action - it's exactly this case. You just execute it and immediately communicate the status back - ok or exception.
  
  The other case, is when the command got created by the system, in response to some event. Because event handling is asynchronous by nature, ie fire-and-forget, so all commands generated in response to this event will also be asynchronous as a consequence.
  
  In fact, even Greg admitted that here
  
  Yevhen
  ```

![add-ha-message-queue](/assets/img/msa/event_command/add-ha-message-queue.png)


## 참조

- https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/microservice-application-layer-implementation-web-api
- [Events vs. Commands in DDD](https://blog.ttulka.com/events-vs-commands-in-ddd)
- https://medium.com/ingeniouslysimple/command-vs-event-in-domain-driven-design-be6c45be52a9
- https://github.com/gregoryyoung/m-r
- https://martinfowler.com/eaaDev/EventNarrative.html#EventsAndCommands
- https://www.enterpriseintegrationpatterns.com/CommandMessage.html
- https://www.enterpriseintegrationpatterns.com/EventMessage.html