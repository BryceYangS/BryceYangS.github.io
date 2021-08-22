---
layout: post
title: "[SpringCloud] Hystrix"
subtitle: "Hystrix"
categories: study
tags: springboot
---

> Spring Cloud Netflix Hystrix

참조코드 : [https://github.com/BryceYangS/bank/tree/hystrix](https://github.com/BryceYangS/bank/tree/hystrix)


## 🚀 Hystrix란?
Netflix에서 개발한 오픈소스로, 원격 시스템이나 서비스를 호출하는 구간을 격리해 관리 및 모니터링을 가능케 하는 라이브러리.  
`Circuit Breaker` 로 호출에 대한 timeout 과 같은 설정을 할 수 있음. 

### 💻 Hystrix 특징
1. 다른 서비스의 실패에 따른 내 서비스의 지연 또는 실패를 방지
2. 분산 시스템의 복잡한 연쇄 실패 방지
3. 빠르게 실패하고 빠르게 복구
4. Fallback과 Gracefully degrade(완벽한 마무리)
5. real-time에 유사한 모니터링과 알람

### 💻 분산 시스템에서의 문제
복잡한 분산 아키텍처의 응용 프로그램에는 수십 개의 의존성이 있으며, 각 의존성은 어느 시점에서 필연적으로 실패.  
호스트 응용 프로그램이 이러한 외부 오류로부터 격리되지 않으면 함께 중단될 위험 존재.  
뿐만 아니라 수 많은 요청이 계속해서 실패함에 따라 애플리케이션 간의 지연 시간을 증가시킴. 이로 인해 Queue, Thread 및 기타 시스템 리소스를 백업하여 시스템 전체에서 더 많은 계단식 실패를 유발할 수 있어 더 위험함.


#### Hystrix 작동  
- 어떠한 단일 의존성도 모든 컨테이너(ie. Tomcat) 사용자 스레드를 사용하지 못하도록 방지
- Queue에 넣는 대신 부하를 줄이고 빠르게 실패
- 사용자를 실패로부터 보호하기 위해 가능한 모든 곳에 fallback 제공
- 격리 기술 (ie. bulkhead, swimlane, circuit breaker 패턴)을 사용해 한 의존성의 영향을 제한
- 실시간에 가까운 매트릭스, 모니터링, 경로를 통해 검색시간 최적화
- 구성 변경의 낮은 지연 전파를 통해 복구 시간을 최적화하고 Hystrix의 대부분의 측면에서 동적 속성 변경을 지원하므로 지연 시간이 짧은 피드백 루프로 실시간 운영 수정 가능
- 네트워크 트래픽뿐만 아니라 전체 의존성 클라이언트 실행의 실패로부터 보호


##### 핵심 클라이언트 회복성 패턴
- Circuit breaker Pattern : 느리게 실행되고, 성능이 저하된 시스템 호출을 종료해 빨리 실패시키고 자원 고갈을 방지한다.
- Fall back Pattern : 개발자가 원격 서비스 호출이 실패하거나 호출에 대한 회로 차단기가 실패할 때 대체할 코드 경로를 정의할 수 있다.
- Bulkhead Pattern : 원격 호출을 서로 격리하고 원격 서비스 호출을 자체 스레드 풀로 분리한다.

#### Hystrix는 해당 목적들을 어떻게 달성하는가? 
- 일반적으로 별도의 쓰레드 내에서 실행되는 HystixCommand 또는 HystrixObservableCommand 개체에서 외부 시스템(또는 "종속성")에 대한 모든 호출을 래핑합니다.(커맨드 패턴의 예)
- 정확한 임계값보다 오래 걸리는 시간 초과 호출. 기본값이 있지만 대부분의 의존성에 대해 "properties"를 통해 이러한 시간 제한을 사용자 지정하여 각 종속성에 대해 측정된 99.5번째 분부위 수 성능보다 약간 높게 설정
- 각 의존성에 대해 작은 **스레드 풀** (또는 semaphore)을 유지. 가득 차면 해당 의존성으로 향하는 요청이 대기열에 추가되는 대신 즉시 거부됨
- 성공, 실패(클라이언트에서 발생한 예외), 시간 초과 및 스레드 거부를 측정
- 서비스에 대한 오류 백분율이 임계값을 초과하는 경우 수동 또는 자동으로 일정 기간 동안 특정 서비스에 대한 모든 요청을 중지하도록 회로 차단기를 실행
- 요청이 실패, 거부, 시간 초과 또는 단락 될 때 대체 논리를 수행
- 거의 실시간으로 매트릭스 및 구성 변경을 모니터링
![soa-4-isolation-640](/assets/img/springcloud/soa-4-isolation-640.png)

❗️Command 패턴  
 커맨드 패턴을 이용하면 요구 사항을 객체로 캡슐화할 수 있으며, 매개변수를 써서 여러 가지 다른 요구 사항을 집어넣을 수도 있습니다. 또한 요청 내역을 큐에 저장하거나 로그로 기록할 수도 있으며, 작업취소 기능도 지원 가능


## 🚀 작동방식
![hystrix-command-flow-chart](/assets/img/springcloud/hystrix-command-flow-chart.png)

1. HystrixCommand 또는 HystrixObservableCommand instance를 생성
2. 생성한 instace 실행
3. 응답에 대한 Cache 여부 확인
4. Circuit Open 여부 확인
5. Thread Pool/Queue/Semaphore 가 가득찼는지 확인
6. HystrixObservableCommand.construct() 또는 HystrixCommand.run()
7. 회로 상태 연산
8. fallback 실행
9. 성공 응답 리턴

## 🚀 Circuit Breaker 구조
circuit-breaker 는 fast-fail(빠른 실패)의 여부를 정하는 회로같은 역할을 수행. circuit-breaker는 hystrix 동작시 caching 확인 후 바로 다음에 확인 되며 열려 있을 경우 fallback 메서드를 실행한다. 

![circuit-breaker](/assets/img/springcloud/circuit-breaker-1280.png)

circuit을 열지 닫을지 판단의 기준은 **일정 시간동안 일정량 이상의 요청에 대해 어느정도의 실패율을 가지느냐**이다. 열려있는 circuit은 *특정시간이 지난 후 다시 1번 로직을 돌려* 성공여부에 따라 열어둘지 다시 닫을지 판단한다.

## 🚀 적용방법

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
    - Srping Cloud는 `Hoxton.SR8`을 사용
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



### [References]
- Neflix-Hystrix : [Hystrix Github Repository](https://github.com/Netflix/Hystrix)
- chanwookpark님의 블로그 글 : [원격 서비스(Remote Service) 관리를 위한 Hystrix 적용 경험기](https://chanwookpark.github.io/hystrix/spring/2015/11/29/hystrix/#7-hystrix-대시보드-사용하기)
- [https://github.com/Netflix/Hystrix/wiki](https://github.com/Netflix/Hystrix/wiki)
- [https://sabarada.tistory.com/52](https://sabarada.tistory.com/52)