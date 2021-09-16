---
layout: page
current: page
cover:  assets/built/images/datamining.png
navigation: True
title: 프로세스 마이닝 개요 2
tags: [processmining]    
class: post-template
subclass: 'post tag-processmining'
author: SeongJae Yu  
sitemap:
lastmod: 2021-09-13
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %} 

  

 프로세스 마이닝- 비즈니스 프로세스 관리(BPM: Business Process Management) 기법들을 보완한다
 
 BPM
    
    - 워크플로우 관리 (WFM :workflow management)의 확장 / WFM: 비즈니스 프로세스의 자동화 초점
    
    - 비즈니스 프로세스 운영을 개선하는 것으로 목표
 
 PAISs(Process-Aware Information systems)
     
    - 프로세스 인지 정보 시스템
     
    - WFM 시스템 포함 
    
    - 대규모 ERP 시스템(SAP,Oracle), 고객 관리 관계(CRM), 규칙 기반 시스템, 콜센터 소프트웨어, 고성능 미들웨어 
    
 프로세스 모델의 목적   
    
    - 통찰력 
    
    - 토의
    
    - 문서화
    
    - 검증 
    
    - 명세화 : PAIS 구현 전에 프로세스 모델을 설계하고 ,작성된 모델은 개발자와 최종 사용자/관리자 사이의 프로세스 명세 계약서로 사용함

 프로세스 마이닝 
    
    - BPM 라이프사이클을 설명듬
        
        |_  1. 설계(design) 단계에서 프로세스 설계 
            
            2. 구성/구현 (configuration/implementation)단계에서 실행 시스템으로 변환함
            
            3. 실행/모니터링(enactment/monitoring)
            
            4. 진단/요구분석(diagnosis/requirements)
            
            5. 재설계
            
 프로세스 마이닝은 세가지 방법으로 이벤트 로그를 분석할수 있다
    
    - 도출(discovery) : 이벤트 로그에서 프로세스에 관련된 다양한 모델을 생성시킴 / a-알고리즘(도출 기법) : 페트리넷 모델 생성함 
    
    - 적합도 검사(conformance checking) : 프로세스 모델과 이벤트 로그를 비교한다/ 로그에 기록된 현실이 모델에 부합하는지, 모델이 현실에 부합하는지 확인하는데 사용함
    
     - 향상(enhancement) : 이벤트 로그에 기록된 실제 프로세스 관련된 정보를 이용하여 기존의 프로세스 모델을 확장하거나 개선하는 것 
        
        |_ 향상의 기법에는 수선(repair), 확장(extension)이 있다 
           
           |_ 수선 : 현실을 잘 반영하기 위해 모델을 변경
            
           |_ 확장 : 프로세스 모델에 새로운 측면을 추가하는 것
 
 프로세스 마이닝 관점
   
    - 프로세스 흐름 관점(control-flow perspective) : 제어흐름, 작업의 순서에 초점을 맞춤 / 페트리넷, EPC, BPMN, UML, AD으로 표현될 수 있는 프로세스 모델에서 가능한 실행 경로를 모두 찾아 특성 파악 
   
    - 조직 관점 (organizational perspective) : 로그에 담긴 자원에 관한 정보에 초점을 맞춤 /소셜네트워크를 도출하는 것
   
    - 케이스 관점 (case perspective) : 케이스의 데이터 값에 의해서 구분될 수 있다.
   
    - 시간 관점 (time perspective) : 이벤트의 발생 시간 및 발생 횟수와 관련이 있다/ 병목 현상 도출, 서비스 수준 측정 , 자원 활용도 측정 
 
 프로세스 마이닝의 도전 과제 
  
    - 과대적합(overfitting) : 모델이 너무 구체적이여서 우연히 관찰된 행위만을 기록하는 것 
   
    - 과소적합(underfitting) : 모델이 너무 일반적이여서 관찰된 행위와 무관한 행위들도 반영하는 경우 
   
    - 위 둘이 서로 균형을 맞추는 것 

 이벤트 로그와 프로세스 모델을 연결하는 세 가지 방법 
    
    - 모형화(play-in) :실제 일어났던 현상으로 부터 모델을 구축 / 추론 / a-알고리즘
     
    - 실제화(play-out) : 페트리넷 부터 프로세스 흐름 패턴을 생성해 볼수 있다
    
    - 리플레이(replay)
 
 #### 트랜지션 시스템
    - 상태(state) , 트랜지션으로 구성 
    
    - 트랜지션은 아크(arc)로 표현됨
    
    - 유한 상태 머신(FSM) , 유한상태오토머턴
    
    - TS =(S,A,T) / S 상태의 집합, A 작업의 집합, T 트랜지션의 집합

 #### 페트리넷 
    - 병렬 상황을 모델링 
    
    - 프로세스 모델링 언어
    
    - 그래픽 표기법
    
    - 플레이스 , 트랜지션으로 구성된 이분 그래프이다.
    
    - 토큰의 상태에 따라 결정 : 마킹
    
    - N =(P,T,F) / P 플레이스 집합, T 트랜지션 집합, F 흐름관계 

 