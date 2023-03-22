---
layout: post
title: "[etc] Event Catalog 활용한 Spring EDA 문서 만들기"
subtitle: "vagrant ssh"
categories: study
tags: etc
---

# Event Catalog
Event-Driven 아키텍처를 위한 Documentation 툴.
- 이벤트, 컨슈머 서비스, 프로듀서 서비스, 도메인, 스키마 문서화에 도움을 주는 도구
- 문서 + 시각화 기능 제공
- Node.js version 14 이상
- 마크다운 파일으로 구성. => 직접 구성해야 하는 단점을 보완하기 위해 plugin 제공 
  - [plugin-generator-asyncapi](https://www.eventcatalog.dev/docs/api/plugins/@eventcatalog/plugin-doc-generator-asyncapi)


```
my-catalog
├── services
│   ├── Basket Service
│   │     └──index.md
│   ├── Data Lake
│   │     └──index.md
│   ├── Payment Service
│   │     └──index.md
│   ├── Shipping Service
│   │     └──index.md
├── events
│   ├── AddedItemToCart
│   │     └──versioned
│   │     │  └──0.0.1
│   │     │     └──index.md
│   │     │     └──schema.json
│   │     └──index.md
│   │     └──schema.json
│   ├── OrderComplete
│   │     └──index.md
│   │     └──schema.json
│   ├── OrderConfirmed
│   │     └──index.md
│   │     └──schema.json
│   ├── OrderRequested
│   │     └──index.md
│   ├── PaymentProcessed
│   │     └──index.md
├── static
│   └── img
├── eventcatalog.config.js
├── package.json
├── README.md
└── yarn.lock
```

- /services : 서비스 아키텍처 설명 마크다운
- /events : 이벤트 관련
- /static : 이미지 등의 static 파일
- eventcatalog.config.js : 설정 파일
  - Site metadata : 타이틀, url 등
  - Generators : 서드 파티를 활용한 documentation 가능.
  - Users

## Events
![event-catalog-events](/assets/img/etc/event_catalog.png)

- 이벤트 명
- 이벤트 상세 정보 : 이벤트 관련 부가 설명을 서술할 수 있음. 하지만, 개발자가 직접 작성 필요. (=> 관리 포인트)
  - github의 markdown 파일을 직접 수정하는 방식
- 이벤트 프로듀서 서비스
- 이벤트 컨슈머 서비스
- 이벤트 스키마
- 이벤트 프로듀서-컨슈머 다이어그램

## Services
![event-catalog-services](/assets/img/etc/event_catalog_services.png)

- 서비스 명, 서비스 상세 정보
- Pub, Sub 이벤트 목록
- swagger OpenAPI도 가능

## Domains
- `/domains/{Domain Name}/index.md` : 도메인에 대한 index
  - `/domains/{Domain Name}/events/{Event Name}` : 도메인에 대한 이벤트 디렉토리. 상위의 /events와 동일 구조를 가짐
  - `/domains/{Domain Name}/services/{Service Name}` : 도메인에 대한 서비스 디렉토리. 상위의 /events와 동일 구조를 가짐

# 참조
- [EventCatalog 공식 홈페이지](https://www.eventcatalog.dev/)
- [Kafka Auto Generated Documentation for Spring with Event-Catalog](https://medium.com/dogus-tech-digital-solutions/kafka-auto-generated-documentation-for-spring-with-event-catalog-c5862b9e1ea9)