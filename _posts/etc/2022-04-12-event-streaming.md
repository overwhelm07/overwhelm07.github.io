---
layout: page
title: "[Markdown] Syntax"
excerpt: "Markdown으로 블로그를 작성하기 위해 Markdown의 기본 문법들을 정리했다. "
categories: ETC
tags: [Kafaka, 분산]
comments: true
published: true 
 
#last_modified_at: 2022-01-02
toc: true
date: 2022-01-02

---


## 데이터 수평 확장 처리

### 의식의 흐름에 따른 분산 처리 
+ Commander Send, 큐, Receiver로 분산처리가능
+ 반복적인 RDS(Relational Database Service) fetch를 해결해야 함
+ 파티션으로 분할된 값을 local data로 구성할 수 있다면 RDS 구성이 필요없음 

### 기술 스택 
#### Event Streaming Platform or Event-Driven Architecture 
1. AWS SQS
Simple Queue Service(SQS)는 마이크로 서비스, 분산 시스템 및 서비스 애플리케이션을 쉽게 분리하고 확장할 수 있도록 지원하는 완전관리형 메시지 대기열 서비스이다. 
SQS에서는 표준 대기열(최대 처리량, 최선 노력순서를 최소 1회 전달), SQS FIFO 대기열로 구성된다. 
1. Kafka



1. Tivco RV


---
## 카프카, 카프카 스트림즈
## 베타적인 지역관리 Uber H3
## 코드를 몰라도 이해 가능 

