---
layout: post
title: "[MongoDB] Insert Methods"
subtitle: "Insert Methods"
categories: study
tags: mongo
---

> Mongo DB docs를 참고해서 작성했습니다.

**목차**  
 - [Insert Methods](#insert-methods)  
 - [추가 Insert 메소드들](#insert-기능을-제공하는-추가적인-메소드들)
---

## Insert Methods

| 쿼리 | 설명 |
|---|---|
| db.collection.insertOne() | Collection에 하나의 document를 Insert |
| db.collection.insertMany() | Collection에 다수의 document를 Insert |
| db.collection.insert() | Collection에 하나 또는 다수의 document를 Insert |

#### 1. db.collection.insertOne()

```js
db.collection.insertOne(
   <document>,
   {
      writeConcern: <document>
   }
)
```
   - document : insert 할 document 데이터 하나를 입력
   - writeConcern : `Optional`. 생략할 경우 기본 값으로 설정됨. 트랜잭션에서 실행되는 경우 명시적으로 설정하지 말 것.
   
<br/>

**Return 값**
   - acknowledged : writeConcern 설정으로 insert 성공 시 `true` 값 반환, 그 외의 경우 `false`
   - insertedId : 삽입된 document의 `_id` 값을 리턴

> _id의 경우 직접 명시를 통해 값을 부여할 수도 있으며, 명시하지 않을 경우 자동으로 생성한다.

<br/>

#### 2. db.collection.insertMany()

```js
db.collection.insertMany(
   [ <document 1> , <document 2>, ... ],
   {
      writeConcern: <document>,
      ordered: <boolean>
   }
)
```
   - document들의 배열 : insert 할 document들의 배열을 기입
   - writeConcern :`Optional`. 생략할 경우 기본 값으로 설정됨. 트랜잭션에서 실행되는 경우 명시적으로 설정하지 말 것.
   - ordered : `Optional`. 정렬해서 insert 할지/말지를 설정. 기본값은 `true`

<br/>

**Return 값**
   - acknowledged : writeConcern 설정으로 insert 성공 시 `true` 값 반환, 그 외의 경우 `false`
   - insertedIds : 삽입된 document들의 `_id`값 배열 을 리턴

<br/>

#### 3. db.collection.insert()

```js
db.collection.insert(
   <document or array of documents>,
   {
     writeConcern: <document>,
     ordered: <boolean>
   }
)
```
   - document : document 또는 document들의 배열
   - writeConcern :`Optional`. 생략할 경우 기본 값으로 설정됨. 트랜잭션에서 실행되는 경우 명시적으로 설정하지 말 것.
   - ordered : `Optional`. 정렬해서 insert 할지/말지를 설정. 기본값은 `true`
      + true 경우 : 만약 에러가 발생할 경우 남은 document들에 대한 프로세스를 처리하지 않고 리턴
      + false 경우 : 만약 에러가 발생할 경우 남은 document들에 대한 프로세스 진행

<br/>

**Return 값**
   - 단일 insert 경우 : WriteResult object
   - 다수 insert 경우 : BulkWriteResult object

<br/>

## Insert 기능을 제공하는 추가적인 메소드들
   - db.collection.update() : `upsert: true` option 값을 설정할 경우.
   - db.collection.updateOne() : `upsert: true` option 값을 설정할 경우.
   - db.collection.updateMany() : `upsert: true` option 값을 설정할 경우.
   - db.collection.findAndModify() : `upsert: true` option 값을 설정할 경우.
   - db.collection.findOneAndUpdate() : `upsert: true` option 값을 설정할 경우.
   - db.collection.findOneAndReplace() : `upsert: true` option 값을 설정할 경우.
   - db.collection.save()
   - db.collection.bulkWrite()


<br/>

## 참조

- Mongo DB Docs - Insert Methods : https://docs.mongodb.com/manual/reference/insert-methods/
