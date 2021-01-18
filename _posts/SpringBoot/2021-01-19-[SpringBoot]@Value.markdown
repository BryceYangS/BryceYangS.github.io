---
layout: post
title: "[SpringBoot]@Value"
subtitle: "Value annotation"
categories: study
tags: springboot
---

> @Value

## @Value
- 프로젝트 내 `application.properties` 내 값을 읽는 방법

```properties
customer.api.url = http://localhost:8080/customer/
```

```java
@Value("${customer.api.url}")
private String customerApiUrl;
```