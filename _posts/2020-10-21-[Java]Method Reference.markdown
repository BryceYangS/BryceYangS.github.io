---
layout: post
title: "[Java]Method Reference"
subtitle: "Method Reference"
categories: study
tags: java
---

## 메서드 레퍼런스(Method Reference)

### 3가지 형태

1. 클래스::인스턴스메서드 (public)
2. 클래스::정적메서드 (static)
3. 객체::인스턴스메서드 (new)

**첫번째 Case**
 - 첫번째 파라미터가 메서드의 수신자가 되고, 나머지 파라미터는 해당 메서드롤 전달
> *String::compareToIgnoreCase* 는  
> *(x,y) -> x.compareToIgnoreCase(y)* 와 동일

**두번째 Case**
 - 모든 파라미터가 정적 메서드로 전달됨
> *Object::isNull* 은  
> *x -> Object.isNull(x)* 와 동일

**세번째 Case**
 - 주어진 객체에서 메서드가 호출되며, 파라미터는 인스턴스 메서드로 전달됨
> *System.out::println* 은  
> *x -> System.out.println(x)* 와 동일


```java
Optional<Comment> byId = commentRepository.findById(100l);
assertThat(byId).isEmpty();
Comment comment = byId.orElse(new Comment());
byId.orElseThrow(() -> new IllegalArgumentException());
byId.orElseThrow(IllegalArgumentException::new); // -> method reference
```