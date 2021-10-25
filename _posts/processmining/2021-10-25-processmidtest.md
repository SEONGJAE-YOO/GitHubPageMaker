---
layout: page
current: page
cover:  assets/built/images/datamining.png
navigation: True
title: 프로세스마이닝 정리
tags: [processmining]    
class: post-template
subclass: 'post tag-processmining'
author: SeongJae Yu  
sitemap:
lastmod: 2021-10-25
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---

#role of a token

1. physical object - product, part, drug
2. information object - message, signal
3. collection - warehouse with parts

#role of a place
1. communication medium - telephone line, middleman
2. buffer -queue or post bin
3. state condition

# transition
1. event - starting an operation, a change seasons 
2. transformation of an object - updating a database
3. transport 

# network structures
1. Causality
2. Parallelism(AND-split,AND-join)
3. Choice(XOR-split,XOR-join)
4. Iteration(XOR-join,XOR-split)

#getting the data
1. process model - event logs
- discovery
- conformance
- enhancement

# log
1. process consists of cases
2. a case consists of events
3. events are ordered
4. events can have attributes
5. attribute names are activity,time,costs,and resource

# XES(eXtensible Event Stream)
1. predecessor : MXML 
2. tools such as ProM

# XES Schema
1. event -> Trace -> Log

# challenges when extracting event logs
1. correlation 
2. timestams 
3. snapshots
4. scoping 


# process discovery = play-in
1. play in -> event log => process model
2. play out -> process modle => event log
3. replay -> event log => process model (predictions,recommendations)

# challenges - evaluating the discovered process
1. Fitness 
2. Precision -> avoid underfitting 
3. generalization -> avoid overfitting 
4. simplicity 

# relations
1. causality -> x->y iff x>y and not y>x
2. parallel -> x||y iff x>y and y>x
3. choice -> x#y iff not x>y and not y>x

#a-algorithm prtri-net 도출 알고리즘
1. TI -> 시작 transition
2. TO -> 종류 

# 프로세스 마이닝 정의
1. 프로세스를 도출하고, 모니터링하며, 개선하기 위해 이벤트 로그로부터 유용한 지식을 추출하는 기법입니다

# BPM라이프사이클 7단계 
1. 구현 단계를 제외한 모든 단계에 활용할 수 있다 

2. 재설계 -> 분석 -> 구현 ->재구성 -> 실행 -> 조정 -> 진단 -> 재설계 

# 프로세스 마이닝 프로젝트 수행 5단계 
1. 계획 및 타당성 평가
2. 추출
3. 프로세스 흐름 모델 도출 
4. 이벤트 로그 연결 
5. 통합 프로세스 모델 생성 
6. 운영 지원 
