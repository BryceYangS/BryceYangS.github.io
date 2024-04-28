---
layout: post
title: "[Data] Apache Pinot Cluster 구축"
subtitle: "apache pinot"
categories: study
tags: etc
---
> Apache Pinot Cluster 구축

# Apache Pinot Cluster 구축 따라해보기
**클러스터 구성**  
- Zookeeper : 1
- Controller : 2
- Broker : 2
- Server : 2


## Pinot Cluster Components
### 1) Zookeeper
- `메타데이터 저장소`

### 2) Pinot Controller
- Controller는 Apache Helix를 호스팅하며, `클러스터 관리` 기능 제공
- web UI를 통한 다양한 기능 제공
  - 클러스터 전체 상태 조회 및 관리
  - Query Console 제공
  - Zookeeper 노드 값 조회 및 수정
  - API 기능 Swagger

### 3) Broker
- 쿼리를 받아 데이터 서버에 전달. 쿼리 결과를 데이터 서버로부터 받아 전달

### 4) Server
- 데이터 세그먼트를 호스팅, 쿼리 제공 역할 

### 5) Minion
- 클러스터 내부에서 효율적인 데이터 이동 및 세그먼트 생성
- optional 컴포넌트 : 필수 아님
- 브로커나 서버와는 독립적으로 Controller로부터 지시를 받아 task를 실행함
- 일회성 혹은 주기적으로 task 실행 가능
- eg) Avro/JSON 데이터를 segement 파일로 변환. 또는 개인정보 보호를 위해 주기적으로 개인정보 제거

## Pinot 논리 구성요소
- Tenant
- Table
- Schema
- Segment

### 1) Tenant
- Helix태그를 제공하여 형성된 **노드의 논리적 그룹**
- 클러스터에 `DefaultTenanat` 이름의 기본 tenant 있음
- Zookeer의 `{clusterName}/CONFIGS/PARTICIPANT/Broker_172.17.0.4_8000` 정보를 보면 tenant가 있는 것을 볼 수 있음.
  ```json
  {
    "id": "Broker_172.17.0.4_8000",
    "simpleFields": {
      "HELIX_HOST": "172.17.0.4",
      "HELIX_PORT": "8000",
      "queryMailboxPort": "38125"
    },
    "mapFields": {},
    "listFields": {
      "TAG_LIST": [
        "DefaultTenant_BROKER"
      ]
    }
  }
  ```
- Zookeer의`{clusterName}/CONFIGS/PARTICIPANT/Server_172.17.0.4_7050` 정보를 보면 tenant가 있는 것을 볼 수 있음.
  ```json
  {
    "id": "Server_172.17.0.4_7050",
    "simpleFields": {
      "HELIX_HOST": "172.17.0.4",
      "HELIX_PORT": "7050",
      "adminPort": "7500",
      "grpcPort": "7100",
      "queryMailboxPort": "35413",
      "queryServerPort": "33073",
      "shutdownInProgress": "false"
    },
    "mapFields": {
      "SYSTEM_RESOURCE_INFO": {
        "numCores": "10",
        "totalMemoryMB": "7851",
        "maxHeapSizeMB": "4096"
      }
    },
    "listFields": {
      "TAG_LIST": [
        "DefaultTenant_OFFLINE",
        "DefaultTenant_REALTIME"
      ]
    }
  }
  ```

### 2) Schema
- 데이터를 가져오기 위해서는 스키마와 테이블이 필요함.
- schmea는 열 데이터를 세 가지로 분류
  1. dimensionFieldSpecs : 차원
  2. metricFieldSpecs : 지표
  3. dateTimeFieldSpecs : 시간
- Zookeeper `{clusterName}/PROPERTYSTORE/SCHEMAS`


```json
{
  "schemaName": "transcript",
  "dimensionFieldSpecs": [
    {
      "name": "studentID",
      "dataType": "INT"
    },
    {
      "name": "firstName",
      "dataType": "STRING"
    },
    {
      "name": "lastName",
      "dataType": "STRING"
    },
    {
      "name": "gender",
      "dataType": "STRING"
    },
    {
      "name": "subject",
      "dataType": "STRING"
    }
  ],
  "metricFieldSpecs": [
    {
      "name": "score",
      "dataType": "FLOAT"
    }
  ],
  "dateTimeFieldSpecs": [{
    "name": "timestamp",
    "dataType": "LONG",
    "format" : "1:MILLISECONDS:EPOCH",
    "granularity": "1:MILLISECONDS"
  }]
}
```

### 3) Table
- 스키마와 연동되어 생성 필요.
- Zookeeper `{clusterName}/PROPERTYSTORE/CONFIGS/TABLE`


```json
{
  "tableName": "transcript",
  "tableType":"OFFLINE",
  "segmentsConfig" : {
    "timeColumnName": "timestamp",
    "replication" : "1"
  },
  "tableIndexConfig" : {
    "loadMode"  : "MMAP"
  },
  "tenants" : {
    "broker":"DefaultTenant",
    "server":"DefaultTenant"
  },
  "metadata": {}
}
```

### 4) Segment
- 실제 데이터가 저장되는 단위
- column에 대한 dictionary, index 정보를 압축하여 저장


## Pinot Data Ingestion
데이터를 끌어오기 위해서는 설정 파일이 필요.  

```yml
executionFrameworkSpec:
  name: 'standalone'
  segmentGenerationJobRunnerClassName: 'org.apache.pinot.plugin.ingestion.batch.standalone.SegmentGenerationJobRunner'
  segmentTarPushJobRunnerClassName: 'org.apache.pinot.plugin.ingestion.batch.standalone.SegmentTarPushJobRunner'
  segmentUriPushJobRunnerClassName: 'org.apache.pinot.plugin.ingestion.batch.standalone.SegmentUriPushJobRunner'
jobType: SegmentCreationAndTarPush
inputDirURI: '/Users/npawar/clusters/gettingStarted/apache-pinot-1.1.0-bin/pinot-tutorial/transcript/rawdata/'
includeFileNamePattern: 'glob:**/*.csv'
outputDirURI: '/Users/npawar/clusters/gettingStarted/apache-pinot-1.1.0-bin/pinot-tutorial/transcript/segments/'
overwriteOutput: true
pinotFSSpecs:
  - scheme: file
    className: org.apache.pinot.spi.filesystem.LocalPinotFS
recordReaderSpec:
  dataFormat: 'csv'
  className: 'org.apache.pinot.plugin.inputformat.csv.CSVRecordReader'
  configClassName: 'org.apache.pinot.plugin.inputformat.csv.CSVRecordReaderConfig'
tableSpec:
  tableName: 'transcript'
pinotClusterSpecs:
  - controllerURI: 'http://localhost:9001'
pushJobSpec:
  pushAttempts: 1
```


## Docker로 띄워보기

```script
docker run \
    -p 2123:2123 \
    -p 9000:9000 \
    -p 8000:8000 \
    -p 7050:7050 \
    -p 6000:6000 \
    apachepinot/pinot:1.1.0 QuickStart \
    -type batch
```  

- 2123: Zookeeper Port
- 9000: Pinot Controller Port
- 8000: Pinot Broker Port
- 7050: Pinot Server Port
- 6000: Pinot Minion Port



### [References]
- [How to setup a Pinot cluster Youtube](https://www.youtube.com/watch?v=cNnwMF0pOJ8&t=15s)
  - [실습 Github Repository](https://github.com/npawar/pinot-tutorial)
- [Apache Pinot Docs](https://docs.pinot.apache.org/basics/concepts)