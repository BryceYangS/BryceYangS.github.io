---
layout: post
title: "[SpringCloud] Hystrix"
subtitle: "Hystrix"
categories: study
tags: springboot
---

> Spring Cloud Netflix Hystrix

참조코드 : [https://github.com/BryceYangS/bank/tree/hystrix](https://github.com/BryceYangS/bank/tree/hystrix)


## 1. Hystrix란?
Netflix에서 개발한 오픈소스로, 원격 시스템이나 서비스를 호출하는 구간을 격리해 관리 및 모니터링을 가능케 하는 라이브러리.  
`Circuit Breaker` 로 호출에 대한 timeout 과 같은 설정을 할 수 있음. 

## 2. 적용방법

> Consumer 서비스(타서비스를 호출하는 서비스)에만 설정을 적용하면 됨 

### 의존성 추가
- build.gradle 파일
    ```
    dependencies {
        //Actuator
        compile('org.springframework.boot:spring-boot-starter-actuator')
  
        //Hystrix
        compile group: 'org.springframework.cloud', name: 'spring-cloud-starter-netflix-hystrix', version: '2.2.1.RELEASE'
        compile group: 'org.springframework.cloud', name: 'spring-cloud-starter-netflix-hystrix-dashboard', version: '2.2.1.RELEASE'
    }
  
    ext {
        set('springCloudVersion', "Hoxton.SR8")
    }
  
    dependencyManagement {
        imports {
            mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
        }
    }
    ```
    - Srping Cloud는 `Hostonx.SR8`을 사용
    - `Sprint Boot Starter Actuator` 의존성 추가.(Actuaotr는 애플리케이션의 상태에 대한 종합적인 분석 기능 제공)
    - `Hystrix` 및 `Hystrix Dashboard` 의존성 추가
        + Hystrix : Circuit Breaker 기능
        + Hystrix Dashboard : Command 실행 결과 실시간 수집 및 UI 제공
 
### Application java 파일
- Main 실행 파일
    + @EnableCircuitBreaker : Hystrix 사용
    + @EnableHystrixDashboard : Hystrix Dashboard 사용
```java
@EnableCircuitBreaker
@EnableHystrixDashboard
@SpringBootApplication
public class AccountApplication {

    public static void main(String[] args) {
        SpringApplication.run(AccountApplication.class, args);
    }

}
```

- properties 파일
    + execution.isolation.thread.timeoutInMilliseconds : 타임아웃 설정
    + hystrix.command.retrieveCustomer : 특정 commandKey의 설정을 따로 설정할 수 있음
    + `Hystrix Dashboard` 사용을 위해 하위 2개 property 설정
```properties
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=10000
hystrix.command.retrieveCustomer.execution.isolation.thread.timeoutInMilliseconds=10000

hystrix.dashboard.proxy-stream-allow-list=*
management.endpoints.web.exposure.include=*
```


- Service 파일
    + @HystrixCommand 어노테이션 추가
        * commandKey 는 해당 메서드에 대한 Hystrix의 고유한 key 값으로 properties에 특정 설정을 가능하게 함.
    + @HystrixCommand 어노테이션 대신 `HystrixCommand<String>` 클래스를 상속 받아 직접 java 클래스로 설정 할수 있음.
        * commandKey를 동적으로 생성하고자 할 때 사용 가능
    
```java
@Service("customerComposite")
public class CustomerCompositeImpl implements CustomerComposite {

  @Value("${customer.api.url}")
  private String customerApiUrl;

  @Bean
  RestTemplate getRestTemplate() {
    return new RestTemplate();
  }

  @Autowired
  RestTemplate restTemplate;

  @HystrixCommand(commandKey="retrieveCustomer", fallbackMethod="fallbackRetriveCustomer")
  @Override
  public Customer retrieveCustomer(String customerId) throws Exception {
    String apiUrl = "/customer/v1.0/{cstmId}";

    return this.restTemplate.getForObject(customerApiUrl + apiUrl,Customer.class, customerId);
  }

  public Customer fallbackRetriveCustomer (String customerId, Throwable t) throws Exception {
    String msg = "restTemplate를 이용하여 " + customerId + " 고객정보 조회 서비스 호출에 문제가 있습니다.";
    log.error(msg, t);
    throw new Exception();
  }

}
```

### Hystrix Dashboard
> 실시간 분석 UI

- URL : `http://{WAS_IP}:{PORT}/{CONTEXT-PATH}/actuator/hystrix.stream` 입력
![Hystrix Dashboard](/assets/img/springboot/Hystrix-Dashboard.png)

- URL을 입력하면 아래와 같은 대시보드 화면으로 넘어가며 실시간 모니터링을 할 수 있다.
![Hystrix Dashboard](/assets/img/springboot/Hystrix-Dashboard2.png)
![node-logo.png](/assets/img/node-logo.png)


[References]
- Neflix-Hystrix : [Hystrix Github Repository](https://github.com/Netflix/Hystrix)
- chanwookpark님의 블로그 글 : [원격 서비스(Remote Service) 관리를 위한 Hystrix 적용 경험기](https://chanwookpark.github.io/hystrix/spring/2015/11/29/hystrix/#7-hystrix-대시보드-사용하기)