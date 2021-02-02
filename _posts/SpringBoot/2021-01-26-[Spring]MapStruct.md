---
layout: post
title: "[Spring]MapStruct"
subtitle: "MapStruct"
categories: study
tags: springboot
---

> MapStruct : Entity <-> DTO 객체 전환 매핑

## 1. MapStruct란?
> MapStruct is a code generator that greatly simplifies the implementation of mappings between Java bean types based on a convention over configuration approach.  
> The generated mapping code uses plain method invocations and thus is fast, type-safe and easy to understand.

Java Bean 간의 매핑 구현을 단순화한 코드 생성기. 즉, 서로 다른 유형의 Bean 객체 간의 전환을 간편하게 처리하는 것을 도와주는 코드 생성기이다.  

일반 메서드 호출을 사용하기 때문에 **빠르고** 형식이 안전하며 이해하기 쉬움  

Spring 에선 주로 `DTO` 와 `Entity` 간의 객체 매핑에 쓰인다.

## 2. MapStruct 설치
- IntelliJ 의 경우 `MapStruct Support` 플러그인을 제공하고 있음
- **Lombok과 같이 적용할 경우 롬복 순서가 <u>더 먼저/위</u>에 있어야 함**

### 2-1. Maven
**pom.xml 파일**
```xml
...
<properties>
    <org.mapstruct.version>1.4.1.Final</org.mapstruct.version>
</properties>
...
<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source> <!-- depending on your project -->
                <target>1.8</target> <!-- depending on your project -->
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- other annotation processors -->
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### 2-2. Gradle
**build.gradle 파일**
- Gradle 4.6 버젼 이상일 경우

    ```gradle
    dependencies {
        ...
        implementation 'org.mapstruct:mapstruct:1.4.1.Final'
        annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.1.Final'
    }
    ```

- Gradle 4.6 버젼 미만일 경우

    ```gradle
    plugins {
        ...
        id 'net.ltgt.apt' version '0.21'
    }
    dependencies {
        ...
        compile 'org.mapstruct:mapstruct:1.4.1.Final'
        apt 'org.mapstruct:mapstruct-processor:1.4.1.Final'
    }
    ```

- Lombok 함께 적용 시 : **lombok 먼저!**
    
    ```gradle
    dependencies {
        ...
        // lombok
        compileOnly 'org.projectlombok:lombok:1.18.16'
        annotationProcessor 'org.projectlombok:lombok:1.18.16'

        // MapStruct
        implementation 'org.mapstruct:mapstruct:1.4.1.Final'
        annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.1.Final'
    }
    ```

## 3. MapStruct 활용

### 3-1. MapStruct 적용 전
- DTO 파일
    + DTO에서 Entity로 전환하는 로직 직접 작성 : DTO -> Entity (public Customer toEntity() {...})
    + Entity에서 DTO로 전환하는 로직 직접 작성 : Entity -> DTO (public CustomerDTO(Customer customer){...})

    ```java
    @Getter
    @Setter
    public class CustomerDTO {

    private String customerId;
    private String name;
    private LocalDate birthDay;
    private String gender;
    private String phoneNumber;
    private String address;
    
    public CustomerDTO() {
        super();
    }

    public CustomerDTO(Customer customer) {
        this.customerId = customer.getCustomerId();
        this.name = customer.getName();
        this.birthDay = customer.getBirthDay();
        this.gender = customer.getGender();
        this.phoneNumber = customer.getPhoneNumber();
        this.address = customer.getAddress();
    }

    public Customer toEntity() {
        return new Customer.Builder(customerId).name(name).birthDay(birthDay).gender(gender).phoneNumber(phoneNumber).address(address).build();
    }

    }
    ```
- Entity 파일

    ```java
    @Entity
    @NoArgsConstructor
    public class Customer {

    @Id @GeneratedValue
    private Long no;

    private String customerId;
    private String name;
    private LocalDate birthDay;
    private String gender;
    private String phoneNumber;
    private String address;

    public Long getNo() {
        return no;
    }
    public String getCustomerId() {
        return customerId;
    }
    public String getName() {
        return name;
    }
    public LocalDate getBirthDay() {
        return birthDay;
    }
    public String getGender() {
        return gender;
    }
    public String getPhoneNumber() {
        return phoneNumber;
    }
    public String getAddress() {
        return address;
    }

    public static class Builder {

        // Required parameters
        private final String customerId;

        // Optional parameters
        private String name;
        private LocalDate birthDay;
        private String gender;
        private String phoneNumber;
        private String address;

        public Builder(String customerId) {
        this.customerId = customerId;
        }

        public Builder name(String name) {
        this.name = name;
        return this;
        }
        public Builder birthDay(LocalDate birthDay){
        this.birthDay = birthDay;
        return this;
        }
        public Builder gender(String gender) {
        this.gender = gender;
        return this;
        }
        public Builder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
        }
        public Builder address(String address) {
        this.address = address;
        return this;
        }

        public Customer build() {
        return new Customer(this);
        }
    }

    private Customer(Builder builder) {
        customerId = builder.customerId;
        name = builder.name;
        birthDay = builder.birthDay;
        gender = builder.gender;
        phoneNumber = builder.phoneNumber;
        address = builder.address;
    }
    }
    ```

### 3-2. MapStruct 적용 후
- 참고사항) Builder 패턴 적용이 잘 되지 않고 있음. 일관성 있게 적용되지 않고 있어 생성자 방식으로 Mapper 구현.

- DTO 파일

```java
@Getter
@Setter
public class CustomerDTO {
  private String customerId;
  private String name;
  private LocalDate birthDay;
  private String gender;
  private String phoneNumber;
  private String address;

  public CustomerDTO() {
    super();
  }
}
```

- Entity 파일
    + Lombok : `@Builder`, `@AllArgsConstructor` 추가
    + **@NoArgsConstructor 를 사용할 경우** Mapper에서 DTO -> Entity 전환 메서드 구현체 로직이 정상적으로 생성되지 않는 문제가 있음
        * 만약 @NoArgsConstructor 를 사용해야 하는 경우 `@NoArgsConstructor(access = AccessLevel.PRIVATE)` 와 같이 접근제어자를 Mapper의 전환 메서드가 사용하지 못하도록 막으면 됨.

    ```java
    @Entity
    @Builder
    @Getter
    @AllArgsConstructor
    public class Customer {

    @Id
    @GeneratedValue
    private Long no;

    private String customerId;
    private String name;
    private LocalDate birthDay;
    private String gender;
    private String phoneNumber;
    private String address;

    }
    ```

- Mapper 인터페이스 파일
    + `@Mapper` : componentModel 속성 값 "spring" 부여

    ```java
    @Mapper(componentModel = "spring")
    public interface CustomerMapper {
        Customer toEntity(CustomerDTO customerDto);
        CustomerDTO toDto(Customer customer);
    }
    ```

- 컴파일 후 `CustomerMapperImpl` 생성됨

    ```java
    @Generated(
    value = "org.mapstruct.ap.MappingProcessor",
    date = "2021-02-03T01:50:11+0900",
    comments = "version: 1.4.1.Final, compiler: IncrementalProcessingEnvironment from gradle-language-java-6.7.1.jar, environment: Java 1.8.0_261 (Oracle Corporation)"
    )
    @Component
    public class CustomerMapperImpl implements CustomerMapper {

        @Override
        public Customer toEntity(CustomerDTO customerDto) {
            if ( customerDto == null ) {
                return null;
            }

            String customerId = null;
            String name = null;
            LocalDate birthDay = null;
            String gender = null;
            String phoneNumber = null;
            String address = null;

            customerId = customerDto.getCustomerId();
            name = customerDto.getName();
            birthDay = customerDto.getBirthDay();
            gender = customerDto.getGender();
            phoneNumber = customerDto.getPhoneNumber();
            address = customerDto.getAddress();

            Long no = null;

            Customer customer = new Customer( no, customerId, name, birthDay, gender, phoneNumber, address );

            return customer;
        }

        @Override
        public CustomerDTO toDto(Customer customer) {
            if ( customer == null ) {
                return null;
            }

            CustomerDTO customerDTO = new CustomerDTO();

            customerDTO.setCustomerId( customer.getCustomerId() );
            customerDTO.setName( customer.getName() );
            customerDTO.setBirthDay( customer.getBirthDay() );
            customerDTO.setGender( customer.getGender() );
            customerDTO.setPhoneNumber( customer.getPhoneNumber() );
            customerDTO.setAddress( customer.getAddress() );

            return customerDTO;
        }
    }
    ```



### [References]
- MapStruct 공식 사이트 : [MapStruct](https://mapstruct.org)
- Mapping Framework 비교 설명 : [Performance of Java Mapping Frameworks](https://www.baeldung.com/java-performance-mapping-frameworks)