---
layout: post
title: "[Test] 테스트 데이터 생성(AutoParams, Instancio, Fixture Monkey)"
subtitle: "get the values of a test, instancio, autoparams, fixture monkey"
categories: study
tags: test
---
> 테스트 데이터 생성

# 테스트 데이터
랜덤한 테스트 데이터는 테스트의 목적을 명확하게 드러내지 못합니다. 테스트 목적에 맞는 데이터를 생성, 사용하기 위한 라이브러리들이 존재합니다.  
현업에서 `AutoParams`, `Instancio`를 적용해보았는데, 아무런 생각없이 사용하고 있다는 생각이 들어 해당 라이브러리들을 비교해보고자 합니다.

- [AutoParams](https://github.com/AutoParams/AutoParams), [Instancio](https://www.instancio.org/), [Fixture Monkey](https://naver.github.io/fixture-monkey/)


## AutoParams
- `@ParameterizedTest` 활용
- 어노테이션 기반으로 테스트 파라미터에 랜덤한 값을 할당, 원하는 경우 커스터마이징한 값 할당 가능
- @AutoSource, @MethodAutoSource, @CsvAutoSource, @ValueAutoSource, @Fix, @Customization 어노테이션을 통해 값 할당
- 객체와 관련된 로직을 작성할 때 @Customization을 활용하거나, 테스트에 불필요한 값을 @AutoSource를 활용한 경험이 있음
    - 테스트에 필요한 값을 세팅하는 경우 특별히 setter가 없는 경우 Customization 클래스에 추가 코드 작성 필요
    - 단건 객체를 사용하는 경우에는 크게 유용하지 않았으나, Collection인 경우에는 유용하게 사용할 수 있었음.

### 간단 예제 코드
```java
@ParameterizedTest
@AutoSource
void testMethod(@Min(1) @Max(10) int value) {
    assertTrue(value >= 1);
    assertTrue(value <= 10);
}
```

## Instancio
- [Generate Unit Test Data in Java Using Instancio - Baeldung](https://www.baeldung.com/java-test-data-instancio)
- 오브젝트 생성 라이브러리
- 랜덤한 값의 객체 생성에서부터 다양한 조건의 객체 생성 가능
- record, sealed 에서도 사용 가능
- AutoParams와 비슷한 기능 제공, but, AutoParam보다 부족한 기능.
    ```java
    @ParameterizedTest
	@InstancioSource
	void singleArgument(Person person) {
        Assertions.assertNotNull(person);    
	}
    ```

### 간단 예제 코드
```java
Person person = Instancio.of(Person.class)
    .ignore(Select.field("id"))
    .set(Select.field("birthDay"), LocalDateTime.now().minusDays(1L))
    .create();
```

## Fixture Mokey
- 네이버 오픈 소스 라이브러리
- 다양한 서드 파티 모듈과 조합 사용 가능 (junit-jupiter, autoparams, jackson, javax-validation, jakarta-validation)
- 커스터 마이징할 수 있는 다양한 옵션들이 존재
    - 조금 더 익숙해지고 다양한 기능들을 사용해봐야 할 듯
    - 기능이 많아서 복잡한 감이 있음. 학습곡선 존재

```java
LabMonkey labMonkey = LabMonkey.labMonkeyBuilder()
	.plugin(new JavaxValidationPlugin())
    .build();

OrderSheet orderSheet = labMonkey.giveMeOne(OrderSheet.class);
```