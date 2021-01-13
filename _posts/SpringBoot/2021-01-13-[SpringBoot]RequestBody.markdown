---
layout: post
title: "[SpringBoot]RequestBody"
subtitle: "RequestBody annotation"
categories: study
tags: springboot
---

> @RequestBody

## @RequestBody
- `POST` 방식으로 전송된 HTTP 요청의 body를 자바 객체로 매핑하는 역할

```java
  @ApiOperation(value = "고객등록", httpMethod = "POST", notes = "고객등록")
  @PostMapping("/customer/v1.0")
  public Long createCustomer(@RequestBody CustomerDTO customerDto) throws Exception{
    return customerService.createCustomer(customerDto);
  }
```

   - POST 방식의 request 내 body를 CustomerDTO 객체에 매핑해줌