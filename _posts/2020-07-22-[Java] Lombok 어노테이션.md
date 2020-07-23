---
layout: post
title: "[Java] Lombok 어노테이션"
subtitle: "Element"
categories: study
tags: java
---


## 자주 사용되는 Lomkok 어노테이션
### @Getter/Setter
 - get/set 메소드를 자동 생성해주는 어노테이션

### @NoArgsConstructor / @RequiredArgsConstructor / @AllArgsConstructor
 - 생성자 자동 생성 어노테이션
 1. @NoArgsConstructor
    - 파라미터가 없는 기본 생성자 생성
 2. @RequiredArgsConstructor 
    - `final` 또는 `@NonNull`인 필드 값만 파라미터로 받는 생성자 생성
 3. @AllArgsConstructor
    - 모든 필드 값을 파라미터로 받는 생성자 생성

### @ToString
 - toString() 메서드 생성
 - `exclude` 속성 사용 시 특정 필드를 toString()에서 제외할 수 있음
 - 출력 결과 : `클래스명(필드명1=필드값1, 필드명2=필드값2)`
 ```java
 @ToString(exclude = "_id")
 @AllArgsConstructor
 @NoArgsConstructor
 public class AdministrativeDong {
     @Id
     private String _id;
     private String type;
     private Geometry geometry;
     private Properties properties;
 }
 ```

### @EqualsAndHashCode
 - 자바 빈을 만들 때 `equals()` , `hashCode()` 메소드 오버라이딩하는 경우 많음.
 - @EqualsAndHashCode 어노테이션이 `equals` & `hashCode` 메소드 자동 생성
 - `callSuper=true` 속성 설정하면 부모 클래스 필드 값들도 동일한지 체크
 - `callSuper=false`(default 값) 속성 설정하면 자신 클래스의 필드 값들만 고려
 ```java
@EqualsAndHashCode(callSuper = true)
public class User extends Domain {
    private String username;
    private String password;
}
 ```  
 ```java
User user1 = new User();
user1.setId(1L);
user1.setUsername("user");
user1.setPassword("pass");

User user2 = new User();
user1.setId(2L); // 부모 클래스의 필드가 다름
user2.setUsername("user");
user2.setPassword("pass");

user1.equals(user2);
// callSuper = true 이면 false, callSuper = false 이면 true
 ```


### @Data
 - `@ToString`, `@EqualsAndHashCode`, `@Getter` 모든필드, `@Setter` final로 선언되지 않은 모든필드, `@RequiredArgsConstructor` 어노테이션을 모두 포함한 어노테이션
 - ***JPA와 같은 `ORM`을 사용할 경우 유의사항 존재***
    - 부모객체와 자식객체의 toString() 간의 문제가 발생할 수 있음
    ```java
    public class Member {
        private String id;
        private String pw;
    
        private Address addr;
    
        @Override
        public String toString() {
            return "Member [id=" + id + ", pw=" + pw + ", addr=" + addr + "]";
        }
    }

    public class Address {
        private String zipcode;
        private Member member;
        
        @Override
        public String toString() {
            return "Address [zipcode=" + zipcode + ", member=" + member + "]";
        }
    }
    ```
    - Member 객체의 `toString()` 호출 시 Address의 `toString()`이 호출 되면서, 다시 Member 객체의 `toString()`을 호출하게 되면서 <u>무한루프</u> 발생  
    <i>---> `@Data` 어노테이션 보다 `@Getter`, `@Setter`를 이용하는 것이 더 안전</i>

