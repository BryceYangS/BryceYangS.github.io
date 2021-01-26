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

- IntelliJ 의 경우 `MapStruct Support` 플러그인을 제공하고 있음

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
- 적용 과정에서 Builder의 경우 `private final String customerId;` 와 같이 필수 입력 값을 설정하는 Builder 패턴을 사용할 경우 에러가 발생 : 결국 final 변수가 없는 Builder 패턴 사용

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

  public CustomerDTO(Customer customer) {
    this.customerId = customer.getCustomerId();
    this.name = customer.getName();
    this.birthDay = customer.getBirthDay();
    this.gender = customer.getGender();
    this.phoneNumber = customer.getPhoneNumber();
    this.address = customer.getAddress();
  }
  
}
```

- Entity 파일
    + Lombok : `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor` 추가

    ```java
    @Entity
    @Builder
    @NoArgsConstructor
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

    }

    ```

- Mapper 인터페이스 파일
    + `@Mapper` : componentModel 속성 값 "sprint" 부여
    + `CustomerMapper INSTANCE = Mappers.getMapper(CustomerMapper.class);`

    ```java
    @Mapper(componentModel = "spring")
    public interface CustomerMapper {

    CustomerMapper INSTANCE = Mappers.getMapper(CustomerMapper.class);

    Customer toEntity(CustomerDTO customerDto);
    CustomerDTO toDto(Customer customer);
    }
    ```

- 컴파일 후 `CustomerMapperImpl` 생성됨

    ```java
    @Generated(
        value = "org.mapstruct.ap.MappingProcessor",
        date = "2021-01-27T01:36:24+0900",
        comments = "version: 1.4.1.Final, compiler: IncrementalProcessingEnvironment from gradle-language-java-6.7.1.jar, environment: Java 14.0.2 (Oracle Corporation)"
    )
    @Component
    public class CustomerMapperImpl implements CustomerMapper {

        @Override
        public Customer toEntity(CustomerDTO customerDto) {
            if ( customerDto == null ) {
                return null;
            }

            CustomerBuilder customer = Customer.builder();

            customer.customerId( customerDto.getCustomerId() );
            customer.name( customerDto.getName() );
            customer.birthDay( customerDto.getBirthDay() );
            customer.gender( customerDto.getGender() );
            customer.phoneNumber( customerDto.getPhoneNumber() );
            customer.address( customerDto.getAddress() );

            return customer.build();
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