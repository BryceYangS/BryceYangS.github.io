---
layout: post
title: "[SpringCloud] Hystrix"
subtitle: "Hystrix"
categories: study
tags: springboot
---

> Spring Cloud Netflix Hystrix

์ฐธ์กฐ์ฝ๋ : [https://github.com/BryceYangS/bank/tree/hystrix](https://github.com/BryceYangS/bank/tree/hystrix)


## ๐ Hystrix๋?
Netflix์์ ๊ฐ๋ฐํ ์คํ์์ค๋ก, ์๊ฒฉ ์์คํ์ด๋ ์๋น์ค๋ฅผ ํธ์ถํ๋ ๊ตฌ๊ฐ์ ๊ฒฉ๋ฆฌํด ๊ด๋ฆฌ ๋ฐ ๋ชจ๋ํฐ๋ง์ ๊ฐ๋ฅ์ผ ํ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ.  
`Circuit Breaker` ๋ก ํธ์ถ์ ๋ํ timeout ๊ณผ ๊ฐ์ ์ค์ ์ ํ  ์ ์์. 

### ๐ป Hystrix ํน์ง
1. ๋ค๋ฅธ ์๋น์ค์ ์คํจ์ ๋ฐ๋ฅธ ๋ด ์๋น์ค์ ์ง์ฐ ๋๋ ์คํจ๋ฅผ ๋ฐฉ์ง
2. ๋ถ์ฐ ์์คํ์ ๋ณต์กํ ์ฐ์ ์คํจ ๋ฐฉ์ง
3. ๋น ๋ฅด๊ฒ ์คํจํ๊ณ  ๋น ๋ฅด๊ฒ ๋ณต๊ตฌ
4. Fallback๊ณผ Gracefully degrade(์๋ฒฝํ ๋ง๋ฌด๋ฆฌ)
5. real-time์ ์ ์ฌํ ๋ชจ๋ํฐ๋ง๊ณผ ์๋

### ๐ป ๋ถ์ฐ ์์คํ์์์ ๋ฌธ์ 
๋ณต์กํ ๋ถ์ฐ ์ํคํ์ฒ์ ์์ฉ ํ๋ก๊ทธ๋จ์๋ ์์ญ ๊ฐ์ ์์กด์ฑ์ด ์์ผ๋ฉฐ, ๊ฐ ์์กด์ฑ์ ์ด๋ ์์ ์์ ํ์ฐ์ ์ผ๋ก ์คํจ.  
ํธ์คํธ ์์ฉ ํ๋ก๊ทธ๋จ์ด ์ด๋ฌํ ์ธ๋ถ ์ค๋ฅ๋ก๋ถํฐ ๊ฒฉ๋ฆฌ๋์ง ์์ผ๋ฉด ํจ๊ป ์ค๋จ๋  ์ํ ์กด์ฌ.  
๋ฟ๋ง ์๋๋ผ ์ ๋ง์ ์์ฒญ์ด ๊ณ์ํด์ ์คํจํจ์ ๋ฐ๋ผ ์ ํ๋ฆฌ์ผ์ด์ ๊ฐ์ ์ง์ฐ ์๊ฐ์ ์ฆ๊ฐ์ํด. ์ด๋ก ์ธํด Queue, Thread ๋ฐ ๊ธฐํ ์์คํ ๋ฆฌ์์ค๋ฅผ ๋ฐฑ์ํ์ฌ ์์คํ ์ ์ฒด์์ ๋ ๋ง์ ๊ณ๋จ์ ์คํจ๋ฅผ ์ ๋ฐํ  ์ ์์ด ๋ ์ํํจ.


#### Hystrix ์๋  
- ์ด๋ ํ ๋จ์ผ ์์กด์ฑ๋ ๋ชจ๋  ์ปจํ์ด๋(ie. Tomcat) ์ฌ์ฉ์ ์ค๋ ๋๋ฅผ ์ฌ์ฉํ์ง ๋ชปํ๋๋ก ๋ฐฉ์ง
- Queue์ ๋ฃ๋ ๋์  ๋ถํ๋ฅผ ์ค์ด๊ณ  ๋น ๋ฅด๊ฒ ์คํจ
- ์ฌ์ฉ์๋ฅผ ์คํจ๋ก๋ถํฐ ๋ณดํธํ๊ธฐ ์ํด ๊ฐ๋ฅํ ๋ชจ๋  ๊ณณ์ fallback ์ ๊ณต
- ๊ฒฉ๋ฆฌ ๊ธฐ์  (ie. bulkhead, swimlane, circuit breaker ํจํด)์ ์ฌ์ฉํด ํ ์์กด์ฑ์ ์ํฅ์ ์ ํ
- ์ค์๊ฐ์ ๊ฐ๊น์ด ๋งคํธ๋ฆญ์ค, ๋ชจ๋ํฐ๋ง, ๊ฒฝ๋ก๋ฅผ ํตํด ๊ฒ์์๊ฐ ์ต์ ํ
- ๊ตฌ์ฑ ๋ณ๊ฒฝ์ ๋ฎ์ ์ง์ฐ ์ ํ๋ฅผ ํตํด ๋ณต๊ตฌ ์๊ฐ์ ์ต์ ํํ๊ณ  Hystrix์ ๋๋ถ๋ถ์ ์ธก๋ฉด์์ ๋์  ์์ฑ ๋ณ๊ฒฝ์ ์ง์ํ๋ฏ๋ก ์ง์ฐ ์๊ฐ์ด ์งง์ ํผ๋๋ฐฑ ๋ฃจํ๋ก ์ค์๊ฐ ์ด์ ์์  ๊ฐ๋ฅ
- ๋คํธ์ํฌ ํธ๋ํฝ๋ฟ๋ง ์๋๋ผ ์ ์ฒด ์์กด์ฑ ํด๋ผ์ด์ธํธ ์คํ์ ์คํจ๋ก๋ถํฐ ๋ณดํธ


##### ํต์ฌ ํด๋ผ์ด์ธํธ ํ๋ณต์ฑ ํจํด
- Circuit breaker Pattern : ๋๋ฆฌ๊ฒ ์คํ๋๊ณ , ์ฑ๋ฅ์ด ์ ํ๋ ์์คํ ํธ์ถ์ ์ข๋ฃํด ๋นจ๋ฆฌ ์คํจ์ํค๊ณ  ์์ ๊ณ ๊ฐ์ ๋ฐฉ์งํ๋ค.
- Fall back Pattern : ๊ฐ๋ฐ์๊ฐ ์๊ฒฉ ์๋น์ค ํธ์ถ์ด ์คํจํ๊ฑฐ๋ ํธ์ถ์ ๋ํ ํ๋ก ์ฐจ๋จ๊ธฐ๊ฐ ์คํจํ  ๋ ๋์ฒดํ  ์ฝ๋ ๊ฒฝ๋ก๋ฅผ ์ ์ํ  ์ ์๋ค.
- Bulkhead Pattern : ์๊ฒฉ ํธ์ถ์ ์๋ก ๊ฒฉ๋ฆฌํ๊ณ  ์๊ฒฉ ์๋น์ค ํธ์ถ์ ์์ฒด ์ค๋ ๋ ํ๋ก ๋ถ๋ฆฌํ๋ค.

#### Hystrix๋ ํด๋น ๋ชฉ์ ๋ค์ ์ด๋ป๊ฒ ๋ฌ์ฑํ๋๊ฐ? 
- ์ผ๋ฐ์ ์ผ๋ก ๋ณ๋์ ์ฐ๋ ๋ ๋ด์์ ์คํ๋๋ HystixCommand ๋๋ HystrixObservableCommand ๊ฐ์ฒด์์ ์ธ๋ถ ์์คํ(๋๋ "์ข์์ฑ")์ ๋ํ ๋ชจ๋  ํธ์ถ์ ๋ํํฉ๋๋ค.(์ปค๋งจ๋ ํจํด์ ์)
- ์ ํํ ์๊ณ๊ฐ๋ณด๋ค ์ค๋ ๊ฑธ๋ฆฌ๋ ์๊ฐ ์ด๊ณผ ํธ์ถ. ๊ธฐ๋ณธ๊ฐ์ด ์์ง๋ง ๋๋ถ๋ถ์ ์์กด์ฑ์ ๋ํด "properties"๋ฅผ ํตํด ์ด๋ฌํ ์๊ฐ ์ ํ์ ์ฌ์ฉ์ ์ง์ ํ์ฌ ๊ฐ ์ข์์ฑ์ ๋ํด ์ธก์ ๋ 99.5๋ฒ์งธ ๋ถ๋ถ์ ์ ์ฑ๋ฅ๋ณด๋ค ์ฝ๊ฐ ๋๊ฒ ์ค์ 
- ๊ฐ ์์กด์ฑ์ ๋ํด ์์ **์ค๋ ๋ ํ** (๋๋ semaphore)์ ์ ์ง. ๊ฐ๋ ์ฐจ๋ฉด ํด๋น ์์กด์ฑ์ผ๋ก ํฅํ๋ ์์ฒญ์ด ๋๊ธฐ์ด์ ์ถ๊ฐ๋๋ ๋์  ์ฆ์ ๊ฑฐ๋ถ๋จ
- ์ฑ๊ณต, ์คํจ(ํด๋ผ์ด์ธํธ์์ ๋ฐ์ํ ์์ธ), ์๊ฐ ์ด๊ณผ ๋ฐ ์ค๋ ๋ ๊ฑฐ๋ถ๋ฅผ ์ธก์ 
- ์๋น์ค์ ๋ํ ์ค๋ฅ ๋ฐฑ๋ถ์จ์ด ์๊ณ๊ฐ์ ์ด๊ณผํ๋ ๊ฒฝ์ฐ ์๋ ๋๋ ์๋์ผ๋ก ์ผ์  ๊ธฐ๊ฐ ๋์ ํน์  ์๋น์ค์ ๋ํ ๋ชจ๋  ์์ฒญ์ ์ค์งํ๋๋ก ํ๋ก ์ฐจ๋จ๊ธฐ๋ฅผ ์คํ
- ์์ฒญ์ด ์คํจ, ๊ฑฐ๋ถ, ์๊ฐ ์ด๊ณผ ๋๋ ๋จ๋ฝ ๋  ๋ ๋์ฒด ๋ผ๋ฆฌ๋ฅผ ์ํ
- ๊ฑฐ์ ์ค์๊ฐ์ผ๋ก ๋งคํธ๋ฆญ์ค ๋ฐ ๊ตฌ์ฑ ๋ณ๊ฒฝ์ ๋ชจ๋ํฐ๋ง
![soa-4-isolation-640](/assets/img/springcloud/soa-4-isolation-640.png)

โ๏ธCommand ํจํด  
 ์ปค๋งจ๋ ํจํด์ ์ด์ฉํ๋ฉด ์๊ตฌ ์ฌํญ์ ๊ฐ์ฒด๋ก ์บก์ํํ  ์ ์์ผ๋ฉฐ, ๋งค๊ฐ๋ณ์๋ฅผ ์จ์ ์ฌ๋ฌ ๊ฐ์ง ๋ค๋ฅธ ์๊ตฌ ์ฌํญ์ ์ง์ด๋ฃ์ ์๋ ์์ต๋๋ค. ๋ํ ์์ฒญ ๋ด์ญ์ ํ์ ์ ์ฅํ๊ฑฐ๋ ๋ก๊ทธ๋ก ๊ธฐ๋กํ  ์๋ ์์ผ๋ฉฐ, ์์์ทจ์ ๊ธฐ๋ฅ๋ ์ง์ ๊ฐ๋ฅ


## ๐ ์๋๋ฐฉ์
![hystrix-command-flow-chart](/assets/img/springcloud/hystrix-command-flow-chart.png)

1. HystrixCommand ๋๋ HystrixObservableCommand instance๋ฅผ ์์ฑ
2. ์์ฑํ instace ์คํ
3. ์๋ต์ ๋ํ Cache ์ฌ๋ถ ํ์ธ
4. Circuit Open ์ฌ๋ถ ํ์ธ
5. Thread Pool/Queue/Semaphore ๊ฐ ๊ฐ๋์ฐผ๋์ง ํ์ธ
6. HystrixObservableCommand.construct() ๋๋ HystrixCommand.run()
7. ํ๋ก ์ํ ์ฐ์ฐ
8. fallback ์คํ
9. ์ฑ๊ณต ์๋ต ๋ฆฌํด

## ๐ Circuit Breaker ๊ตฌ์กฐ
circuit-breaker ๋ fast-fail(๋น ๋ฅธ ์คํจ)์ ์ฌ๋ถ๋ฅผ ์ ํ๋ ํ๋ก๊ฐ์ ์ญํ ์ ์ํ. circuit-breaker๋ hystrix ๋์์ caching ํ์ธ ํ ๋ฐ๋ก ๋ค์์ ํ์ธ ๋๋ฉฐ ์ด๋ ค ์์ ๊ฒฝ์ฐ fallback ๋ฉ์๋๋ฅผ ์คํํ๋ค. 

![circuit-breaker](/assets/img/springcloud/circuit-breaker-1280.png)

circuit์ ์ด์ง ๋ซ์์ง ํ๋จ์ ๊ธฐ์ค์ **์ผ์  ์๊ฐ๋์ ์ผ์ ๋ ์ด์์ ์์ฒญ์ ๋ํด ์ด๋์ ๋์ ์คํจ์จ์ ๊ฐ์ง๋๋**์ด๋ค. ์ด๋ ค์๋ circuit์ *ํน์ ์๊ฐ์ด ์ง๋ ํ ๋ค์ 1๋ฒ ๋ก์ง์ ๋๋ ค* ์ฑ๊ณต์ฌ๋ถ์ ๋ฐ๋ผ ์ด์ด๋์ง ๋ค์ ๋ซ์์ง ํ๋จํ๋ค.

## ๐ ์ ์ฉ๋ฐฉ๋ฒ

> Consumer ์๋น์ค(ํ์๋น์ค๋ฅผ ํธ์ถํ๋ ์๋น์ค)์๋ง ์ค์ ์ ์ ์ฉํ๋ฉด ๋จ 

### ์์กด์ฑ ์ถ๊ฐ
- build.gradle ํ์ผ
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
    - Srping Cloud๋ `Hostonx.SR8`์ ์ฌ์ฉ
    - `Sprint Boot Starter Actuator` ์์กด์ฑ ์ถ๊ฐ.(Actuaotr๋ ์ ํ๋ฆฌ์ผ์ด์์ ์ํ์ ๋ํ ์ขํฉ์ ์ธ ๋ถ์ ๊ธฐ๋ฅ ์ ๊ณต)
    - `Hystrix` ๋ฐ `Hystrix Dashboard` ์์กด์ฑ ์ถ๊ฐ
        + Hystrix : Circuit Breaker ๊ธฐ๋ฅ
        + Hystrix Dashboard : Command ์คํ ๊ฒฐ๊ณผ ์ค์๊ฐ ์์ง ๋ฐ UI ์ ๊ณต
 
### Application java ํ์ผ
- Main ์คํ ํ์ผ
    + @EnableCircuitBreaker : Hystrix ์ฌ์ฉ
    + @EnableHystrixDashboard : Hystrix Dashboard ์ฌ์ฉ

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

- properties ํ์ผ
    + execution.isolation.thread.timeoutInMilliseconds : ํ์์์ ์ค์ 
    + hystrix.command.retrieveCustomer : ํน์  commandKey์ ์ค์ ์ ๋ฐ๋ก ์ค์ ํ  ์ ์์
    + `Hystrix Dashboard` ์ฌ์ฉ์ ์ํด ํ์ 2๊ฐ property ์ค์ 
    
```properties
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=10000
hystrix.command.retrieveCustomer.execution.isolation.thread.timeoutInMilliseconds=10000

hystrix.dashboard.proxy-stream-allow-list=*
management.endpoints.web.exposure.include=*
```


- Service ํ์ผ
    + @HystrixCommand ์ด๋ธํ์ด์ ์ถ๊ฐ
        * commandKey ๋ ํด๋น ๋ฉ์๋์ ๋ํ Hystrix์ ๊ณ ์ ํ key ๊ฐ์ผ๋ก properties์ ํน์  ์ค์ ์ ๊ฐ๋ฅํ๊ฒ ํจ.
    + @HystrixCommand ์ด๋ธํ์ด์ ๋์  `HystrixCommand<String>` ํด๋์ค๋ฅผ ์์ ๋ฐ์ ์ง์  java ํด๋์ค๋ก ์ค์  ํ ์ ์์.
        * commandKey๋ฅผ ๋์ ์ผ๋ก ์์ฑํ๊ณ ์ ํ  ๋ ์ฌ์ฉ ๊ฐ๋ฅ
    
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
    String msg = "restTemplate๋ฅผ ์ด์ฉํ์ฌ " + customerId + " ๊ณ ๊ฐ์ ๋ณด ์กฐํ ์๋น์ค ํธ์ถ์ ๋ฌธ์ ๊ฐ ์์ต๋๋ค.";
    log.error(msg, t);
    throw new Exception();
  }

}
```

### Hystrix Dashboard
> ์ค์๊ฐ ๋ถ์ UI

- URL : `http://{WAS_IP}:{PORT}/{CONTEXT-PATH}/actuator/hystrix.stream` ์๋ ฅ

![Hystrix Dashboard](/assets/img/springboot/Hystrix-Dashboard.png)

- URL์ ์๋ ฅํ๋ฉด ์๋์ ๊ฐ์ ๋์๋ณด๋ ํ๋ฉด์ผ๋ก ๋์ด๊ฐ๋ฉฐ ์ค์๊ฐ ๋ชจ๋ํฐ๋ง์ ํ  ์ ์๋ค.

![Hystrix Dashboard](/assets/img/springboot/Hystrix-Dashboard2.png)



### [References]
- Neflix-Hystrix : [Hystrix Github Repository](https://github.com/Netflix/Hystrix)
- chanwookpark๋์ ๋ธ๋ก๊ทธ ๊ธ : [์๊ฒฉ ์๋น์ค(Remote Service) ๊ด๋ฆฌ๋ฅผ ์ํ Hystrix ์ ์ฉ ๊ฒฝํ๊ธฐ](https://chanwookpark.github.io/hystrix/spring/2015/11/29/hystrix/#7-hystrix-๋์๋ณด๋-์ฌ์ฉํ๊ธฐ)
- [https://github.com/Netflix/Hystrix/wiki](https://github.com/Netflix/Hystrix/wiki)
- [https://sabarada.tistory.com/52](https://sabarada.tistory.com/52)