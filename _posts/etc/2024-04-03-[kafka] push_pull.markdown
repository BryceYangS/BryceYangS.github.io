---
layout: post
title: "[Kafka] Push vs Pull"
subtitle: "kafka design push and pull"
categories: study
tags: etc
---
> Kafka 설계에서 Push 와 Pull 모델

# Push vs Pull
Kafka Consumer는 브로커에 메시지 가져오기 요청을 보냄. : 즉, `Pull 모델`으로 메시지 컨슘  

Push 방식으로 처리할 경우, 브로커가 데이터 전송 속도를 제어하기 때문에 consumer가 처리 속도를 따라가지 못 할 수 있음.  


# 참조
- [Kafka Doc](https://kafka.apache.org/documentation/#design_pull)