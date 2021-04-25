---
layout: post
title: "[Database] DB Lock"
subtitle: "db lock"
categories: study
tags: db
---
> Database의 Lock에 대한 이해  
# DB Lock
```
💡 Lock은 트랜잭션 처리의 순차성을 보장해주는 기능을 제공
```
- 트랜잭션 : **데이터베이스의 데이터를 조작하는 작업의 단위**
    + 트랜잭션의 ACID 4가지 특징
        1. Atomicity(원자성)
            * 트랜잭션의 작업이 부분적으로 성공하는 일이 없도록 보장
            * All or Nothing
        2. Consistency(일관성)
            * 트랜잭션이 끝날 때 DB의 여러 제약조건에 맞는 상태를 보장
        3. Isolation(독립성)
            * 트랜잭션이 진행되는 중간 상태의 데이터를 다른 트랜잭션이 볼 수 없도록 보장
        4. Durability(영구성)
            * 트랜잭션이 성공했을 경우 해당 결과가 영구적으로 적용됨을 보장
- Lock은 하나의 트랜잭션이 완벽하게 끝날 때까지 다른 요청을 막아줌

## Transaction의 Isolation Level
> ACID 원칙은 완벽히 지켜지지 않는다   

ACID 원칙을 strict하게 지키려면 **동시성이 매우 떨어지게** 된다.  
DB 엔진은 ACID 원칙을 희생해 동시성을 얻을 수 있는 방법을 제공한다. 바로 ***Transaction의 Isolation Level***이다.  
Isolation 원칙을 덜 지키는 level을 사용할수록 문제가 발생할 가능성은 커지지만 **더 높은 동시성**을 얻을 수 있다.  

- Isolation Level : 상위레벨(0 ~ 3)로 갈수록 격리 수준이 높음
    0. `READ UNCOMMITED`
        - **커밋되지 않은 내용**까지 다 읽을수 있도록 하는 **느슨한 격리** 수준
        - 배타적 잠금이 걸려있어도 읽을 수 있도록 해준다
        - `Dirty Read`의 문제가 발생 : 변경 중인 값을 읽어오면 그 값이 commit 되는 순간 틀린 값이 되기 때문
    1. `READ COMMITED`
        - **커밋된 내용만 읽을수** 있도록 하는 가장 많이 쓰이는 격리 수준
        - 공유잠금된 내용은 읽을 수 ⭕️, 배타적 잠금이 걸려있으면 읽을 수 ❌
        - `NON_REPEATABLE_READ` 문제가 발생 : 배타적 잠금이 걸리기 전에 읽은 값이 직후에 변경된다면 다시 조회했을 때 처음과 다른 값이 나오기 때문
    2. `REPEATABLE READ`
        - 한 트랜잭션 동안엔 해당되는 **row에 대하여 DML이 일어날 수 없음**
        - 공유잠금이 걸리면 해당 격리수준이 된다
        - `PHANTOM READ` 문제 발생 : 한 트랜잭션 안에서 일정 범위의 레코드를 두 번 이상 읽을 때, 첫번째 쿼리에서 없던 레코드가 두번째 쿼리에서 나타나는 현상
    3. `SERIALIZABLE`
        - 한 세션이 잡고 있는 동안 **다른 세션의 트랜잭션이 모두 접근할 수 없도록** 하는 격리 수준
        - 동시성이 매우 떨어짐
        - 기본적으로 REPEATABLE READ와 동일. 대신, SELECT 쿼리가 전부 `SELECT ... FOR SHARE`로 자동으로 변경된다.
        - 데이터를 안전하게 보호할 수는 있지만 **쉽게 데드락에 걸릴 수 있다**


## Inno DB의 Lock
Inno DB는 다양한 종류의 lock을 사용

1. Row-level Lock

2. Record Lock
3. Gap Lock
4. 


## Lock 종류
### 1. Shared Lock(공유 잠금)
- Read Lock, 읽기 잠금
- SELECT for share
- `읽기` 활동을 할 때만 발생
- 공유잠금끼리는 충돌하지 않음 : Read Lock은 같은 Read Lock 끼리 동시 접근 가능
- 공유잠금이 걸린 동안 DML (Insert, Update, Delete)을 할 수 없음
- Transaction Isolation을 REPEATABLE READ로 만들어줌

### 2. Exclusive Lock(배타적 잠금)
- Write Lock, 쓰기 잠금
- INSERT, UPDATE, DELETE
- SELECT for update
- 배타적 잠금이 발생하면 하나의 리소스만 해당 부분을 점유할 수 있음



### [References]
- [https://centbin-dev.tistory.com/40](https://centbin-dev.tistory.com/40)
- [https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/](https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/)
- [https://www.notion.so/05a1d0a9e9c44d12a69d4bf9f1cd2086?v=e617ae925eda4c68805b79bae32bff01&p=0a4bfd8b40184a8182677fe662d7b2ef](https://www.notion.so/05a1d0a9e9c44d12a69d4bf9f1cd2086?v=e617ae925eda4c68805b79bae32bff01&p=0a4bfd8b40184a8182677fe662d7b2ef)