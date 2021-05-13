---
layout: post
title: "[SpringCloud]Load Balancing-Ribbon"
subtitle: "Ribbon"
categories: study
tags: springboot
---

> SpringCloud Load Balancing : Riboon 라이브러리 활용

[참조코드 Git Repository](https://github.com/BryceYangS/bank/tree/load-balancing)


# Load Banlancing

## 1. Ribbon Client
`Netflix OSS` 라이브러리 중 하나.   
L7 Layer에서 `Client Side Load Balancer` 역할 담당.  
최근 RestTemplate 대신 사용하는 `FeignClient`의 경우 이미 Ribbon 기능이 포함되어 있음.

## 2. Ribbon Client 장점
1. REST API를 호출하는 서비스에 탑제되는 SW 모듈
2. 주어진 서버 목록에 대해 Load Balancing 기능
3. 다양한 설정 가능 (서버 선택, 실패시 skip 시간, ping 체크 등)
4. Ribbon에는 Retry 기능 내장
5. Eureka와 함께 사용될 경우 강력한 기능 제공


## 3. Ribbon Client 적용
### 3-1. 라이브러리 Dependency 추가

```gradle
implementation('org.springframework.cloud:spring-cloud-starter-Netflix-ribbon')
```

### 3-2. application.yml 또는 application.properties

yaml 파일

```yaml
minibank-customer:
  ribbon:
    listOfServers: localhost:8076, localhost:9076
```

properties 파일

```properties
# eureka 사용 여부 : eureka 사용 시 동적 서버 목록 사용 가능
minibank-customer.ribbon.eureka.enabled = false
# 정적 서버 목록
minibank-customer.ribbon.listOfServers=localhost:8076,localhost:9076
```

### 3-3. 연결 요청을 수행 할 RestTemplate 에 @LoadBalanced Annotation 추가(Feign 사용할 경우 불필요) 

```java
@LoadBalanced
public RestTemplate restTemplate(){|
  return new RestTemplate();
}
```

### 3-4. RestTemplate의 호출 URL을 application.yml(.properties)에 지정한 이름으로 변경

```java
  public Customer retrieveCustomer(String customerId) throws Exception {
    String apiUrl = "http://minibank-customer/minibank/customer/customer/v1.0/{cstmId}";

    return this.restTemplate.getForObject(apiUrl,Customer.class, customerId);
  }
```

### 3-5. 기타 load balancing 관련 추가 옵션 application.yml 파일에 지정

**yml 파일**

```yaml
serviceB:
    ribbon:
        listOfServers: localhost:8082, localhost:8083
        MaxAutoRetries: 0
        MaxAutoRetriesNextServer: 1 # 실패시 다른 서버로 재시도 하는 횟수
```

**properties 파일**

```properties
# 서버 목록 새로고침 interval time
minibank-customer.ribbon.ServerListRefreshInterval=15000
# 첫 호출 실패 시 같은 서버로 접속 시도 횟수
minibank-customer.ribbon.MaxAutoRetries=0
```