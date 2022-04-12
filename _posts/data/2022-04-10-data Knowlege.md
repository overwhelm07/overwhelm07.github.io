---
layout: post
title: "Data 기본 지식"
discription: "Data Knowledge"
#hedline: 
categories: Data
tags: [Data]
comments: true
published: true 
toc: true
paramlink: /data/
entries_layout: grid
breadcrumb: true
---
# Data 공부 
## Data Aggregation
## CAP Theorem
CAP Theorem은 분산시스템에서 일관성(Consistency), 가용성(Availability), 분할 용인(Partition tolerance)이라는 세 가지 조건을 모두 만족할 수 없다는 정리로, 세 가지 중 두 가지를 택하라는 것으로 많이 알려졌다. 

<img src="../../img\truth-of-cap-theorem-diagram.png" width="500" height="300"> 

1. 일관성: 모든 노드들은 같은 시간에 동일한 항목에 대하여 같은 내용의 데이터가 사용자에게 보여준다.
1. 가용성: 모든 사용자들이 읽기 및 쓰기가 가능해야 하며, 몇몇 노드의 장애 시에도 다른 노드에 영향을 미치면 안된다. 즉, 모든 요청은 정상 응답을 받는다. 
1. 분할내성: 메시지 전달이 실패하거나 시스템 일부가 망가져도 시스템이 계속 동작할 수 있어야 한다. <b>즉, 노드간 통신이 실패하더라도 시스템은 정상 동작하는 가용성이 보장되어야 한다. </b>


## ACID 
ACID는 DB Transaction이 안전하게 수행된다는 것을 보장하기 위한 성질을 가리키는 약어이다.

1. 원자성(Atomic): 트랜잭션과 관련된 작업들이 모두 수행 or 모두 실행이 안되었는지를 보장
1. 일관성(Consistency): 트랜잭션 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 유지하는 것을 의미
1. 고립성(Isolation): 트랜잭션 수행 중 다른 작업이 Interrupt 불가능 
1. 지속성(Durability): 트랜잭션이 수행이 성공되면 영원히 반영되어야 한다. like commit

## Distribute Cache 
