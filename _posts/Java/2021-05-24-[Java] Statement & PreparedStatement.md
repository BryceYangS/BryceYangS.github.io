---
layout: post
title: "[Java] Statement & PreparedStatement"
subtitle: "Statement & PreparedStatement"
categories: study
tags: java
---

> Statement & PreparedStatement 비교

### 🚀Statement
1. Statement 객체는 Connection 클래스의 `createStatement()` 메서드 호출을 통해 생성.
2. Statement 객체가 생성되면 executeQuery() 메서드를 호출해 SQL문을 실행시킬 수 있다. 메서드의 인수로 SQL문을 담은 String 객체를 전달한다.
3. Statement는 `정적`인 쿼리문을 처리할 수 있다. 즉 쿼리문에 값이 미리 입려되어 있어야 한다.

### 🚀PreparedStatement
1. PreparedStatement 객체는 Connection 객체의 `prepareStatement()` 메서드를 사용해서 생성. 메서드 인수로 SQL문을 담은 String 객체가 필요
2. SQL 문장이 **미리 컴파일**되고, 실행 시간동안 인수값을 위한 공간을 확보할 수 있다는 점에서 Statement 객체와 다름
3. Statement 객체의 SQL은 실행될 때 매 번 서버에서 분석해야 하는 반면, PreparedStatement 객체는 한 번 분석되면 `재사용` 용이
4. 각각의 인수에 대해 위치홀더(placeholder)를 사용해 SQL 문장을 정의할 수 있게 해준다. 위치홀더는 **?** 로 표현됨
5. 동일한 SQL문을 특정 값만 바꾸어서 **여러 번 실행해야 할 때**, 인수가 많아서 SQL문을 정리해야 될 필요가 있을 때 사용하면 유용


![prepared-statement-performance-1](/assets/img/etc/prepared-statement-performance-1.png)

- PreparedStatement의 재사용 수준은 두 가지
    1. JDBC 드라이버에 의한 PreparedStatement 재사용
    2. DB에 의한 PreparedStatement 재사용

### 🚀어떤 것을 사용해야 하는가?
Statement의 경우 매 번 쿼리에 대한 컴파일을 수행한다. PreparedStatement의 경우 미리 컴파일된 쿼리를 사용하기 때문에 수행 속도가 더 빠르다는 장점이 있다.  
SQL Injection 을 막고, 쿼리문 재사용을 통한 성능상의 이점을 위해 PreparedStatement를 사용한다.  
무조건 PreparedStatement를 사용할 경우 DB에 캐싱된 SQL문이 과도해질 수 있다. 이는 성능을 낮출 수 있으니, 자주 사용되는 구문만 PreparedStatement로 하는 것이 좋은 경우도 있다.

### 🚀4.3. 참조
- [http://tutorials.jenkov.com/jdbc/preparedstatement.html](http://tutorials.jenkov.com/jdbc/preparedstatement.html)
- [https://devbox.tistory.com/entry/Comporison](https://devbox.tistory.com/entry/Comporison)
