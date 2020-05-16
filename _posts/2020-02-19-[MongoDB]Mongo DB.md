<<<<<<< HEAD
---
layout: post
title: "[MongoDB]Mongo DB"
subtitle: "Mongo DB"
categories: study
tags: mongo
---

<a href="https://www.mongodb.com"><img src="/assets/img/mongodb.jpeg" width="400px" alt="MongoDB"></a>

# Mongo DB

- NoSQL

## Database

- 데이터베이스는 Collection들의 물리적 컨테이너.
- 각 데이터베이스는 파일시스템에서 파일들의 집합
- 일반적으로 하나의 mongo DB 서버는 여러개의 데이터베이스를 가짐

## Collection

- Document들의 집합.
- RDBMS의 table과 동일한 개념
- shema 없음
- 한 Collection 안의 document들은 서로 다른 필드를 가질 수 있음
- 일반적으로, 한 Collection 안의 document들은 비슷하거나 연관된 목적을 가짐

## Document

- RDBMS의 record와 유사한 개념
- JSON object 형태의 key-value의 쌍으로 이루어진 데이터 구조
- 다이나믹한 schema를 가짐 : 같은 collection이어도 다른 field, structure를 가질 수 있음
- value : 다른 document, array, document array가 포함 가능

## Sample Document

```
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview',
   description: 'MongoDB is no sql database',
   by: 'tutorials point',
   url: 'http://www.tutorialspoint.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100,
   comments: [
      {
         user:'user1',
         message: 'My first comment',
         dateCreated: new Date(2011,1,20,2,15),
         like: 0
      },
      {
         user:'user2',
         message: 'My second comments',
         dateCreated: new Date(2011,1,25,7,45),
         like: 5
      }
   ]
}

```

- 각 document는 `_id`라는 고유값을 가짐(12byte의 16진수)
  - 시간(4byte)/머신id(3byte)/프로세스id(2byte)/순차번호(3byte) 로 구성되면 값의 고유성을 보장

| RDBMS       | MongoDB            |
| ----------- | ------------------ |
| Database    | Database           |
| Table       | Collection         |
| Tuple/Row   | Document           |
| column      | Field              |
| Table Join  | Embedded Documents |
| Primary Key | Primary Key (Default  key _id provided by mongodb itself) |

| Database Server and Client |        |
| -------------------------- | ------ |
| Mysqld/Oracle              | mongod |
| mysql/sqlplus              | mongo  |

## 참조

- tutorials point : https://www.tutorialspoint.com/mongodb/mongodb_overview.htm
- Poiemaweb : https://poiemaweb.com/mongdb-basics
=======
---
layout: post
title: "[MongoDB]Mongo DB"
subtitle: "Mongo DB"
categories: study
tags: mongo
---

<a href="https://www.mongodb.com"><img src="/assets/img/mongodb.jpeg" width="400px" alt="MongoDB"></a>

# Mongo DB

- NoSQL

## Database

- 데이터베이스는 Collection들의 물리적 컨테이너.
- 각 데이터베이스는 파일시스템에서 파일들의 집합
- 일반적으로 하나의 mongo DB 서버는 여러개의 데이터베이스를 가짐

## Collection

- Document들의 집합.
- RDBMS의 table과 동일한 개념
- shema 없음
- 한 Collection 안의 document들은 서로 다른 필드를 가질 수 있음
- 일반적으로, 한 Collection 안의 document들은 비슷하거나 연관된 목적을 가짐

## Document

- RDBMS의 record와 유사한 개념
- JSON object 형태의 key-value의 쌍으로 이루어진 데이터 구조
- 다이나믹한 schema를 가짐 : 같은 collection이어도 다른 field, structure를 가질 수 있음
- value : 다른 document, array, document array가 포함 가능

## Sample Document

```
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview',
   description: 'MongoDB is no sql database',
   by: 'tutorials point',
   url: 'http://www.tutorialspoint.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100,
   comments: [
      {
         user:'user1',
         message: 'My first comment',
         dateCreated: new Date(2011,1,20,2,15),
         like: 0
      },
      {
         user:'user2',
         message: 'My second comments',
         dateCreated: new Date(2011,1,25,7,45),
         like: 5
      }
   ]
}

```

- 각 document는 `_id`라는 고유값을 가짐(12byte의 16진수)
  - 시간(4byte)/머신id(3byte)/프로세스id(2byte)/순차번호(3byte) 로 구성되면 값의 고유성을 보장

| RDBMS       | MongoDB            |
| ----------- | ------------------ |
| Database    | Database           |
| Table       | Collection         |
| Tuple/Row   | Document           |
| column      | Field              |
| Table Join  | Embedded Documents |
| Primary Key | Primary Key (Default  key _id provided by mongodb itself) |

| Database Server and Client |        |
| -------------------------- | ------ |
| Mysqld/Oracle              | mongod |
| mysql/sqlplus              | mongo  |

## 참조

- tutorials point : https://www.tutorialspoint.com/mongodb/mongodb_overview.htm
- Poiemaweb : https://poiemaweb.com/mongdb-basics
>>>>>>> 234dca0354054c6318765710583dd41de701a912
