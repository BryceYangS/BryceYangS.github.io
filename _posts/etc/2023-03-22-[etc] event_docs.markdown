---
layout: post
title: "[etc] Event Catalog 활용한 Spring EDA 문서 만들기"
subtitle: "vagrant ssh"
categories: study
tags: etc
---
> Event-Catalog + Spring

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

# Kafka 이벤트 Documentation 자동 생성 with Spring, Event-Catalog
Swaggr와 같이 자동으로 문서화를 해주는 작업. Event-Catalog의 plugin-generator-asyncapi를 이용하여 이벤트 문서를 자동 생성하고자 함.

> AsyncAPI?
> 이벤트 드리븐 아키텍처를 유지보수하기 위한 오픈소스 툴. 비동기 API를 위한 표준을 사용.


## AsyncAPI 파일 만들기
Spring에서 AsyncAPI를 위해 내부적으로 [`springwolf`](https://springwolf.github.io/) 사용. springwolf는 Kafka, RabbitMQ를 위한 AsyncAPI 설정을 json 파일 생성 기능 제공. 기본적으로 base 패키지부터 스캔을 함.  

[springkafkadoc](https://github.com/DogusTeknoloji/springkafkadoc) : 프로듀서와 컨슈머 패키지가 분리되어 있는 환경을 위해, 기존의 springwolf를 커스터마이징한 오픈 소스 라이브러리.

![spring-kafka-doc](/assets/img/etc/springkafkadoc.webp)


## 사용법
### Config

```java
@Configuration
@EnableAsyncApi
class AsyncApiConfiguration {

    @Bean
    fun asyncApiDocket(): AsyncApiDocket {

        val info = Info.builder()
            .version("1.0.0")
            .title("Customer Service")
            .build()

        val kafkaServer = Server.builder()
            .protocol("kafka")
            .url(BOOTSTRAP_SERVERS)
            .build()

        return AsyncApiDocket.builder()
            .consumerBasePackage(KAFKA_CONSUMER_BASE_PACKAGE)
            .producerBasePackage(KAFKA_PRODUCER_BASE_PACKAGE)
            .info(info)
            .server("kafka", kafkaServer)
            .build()
    }

    private companion object {
        const val BOOTSTRAP_SERVERS = "0.0.0.0:9092"
        const val KAFKA_CONSUMER_BASE_PACKAGE = "your consumer package"
        const val KAFKA_PRODUCER_BASE_PACKAGE = "your producer package"
    }
}
```

### Producer & Consumer 코드 수정
```java
@Component
class CustomerCreatedEventConsumer {

    @KafkaListener(topicPattern = "\${kafka.topics.customerCreated}")
    fun receive(@Payload payload: CustomerCreatedEvent) {
        // TODO
    }
}
```


```java
@Component
class CustomerCreatedEventProducer {

    @AsyncApiDocProducer("customerCreated")
    fun send(@Payload payload: CustomerCreatedEvent) {
        // TODO
    }
}
```

**Springkafkadoc**  
- scan consumers and producers with **@Component**
- scan models with **@Payload**
- scan producers’ topics with **@AsyncApiDocProducer**


# 참조
- [EventCatalog 공식 홈페이지](https://www.eventcatalog.dev/)
- [Kafka Auto Generated Documentation for Spring with Event-Catalog](https://medium.com/dogus-tech-digital-solutions/kafka-auto-generated-documentation-for-spring-with-event-catalog-c5862b9e1ea9)