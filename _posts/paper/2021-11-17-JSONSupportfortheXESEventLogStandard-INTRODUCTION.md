---
layout: page
current: page
cover:  assets/built/images/book.png
navigation: True
title: JSON Support for the XES Event Log Standard - INTRODUCTION
tags: [thesis]  
class: post-template
subclass: 'post tag-thesis'  
author: SeongJae Yu    
sitemap:
lastmod: 2021-11-17
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include JSONSupportfortheXESEventLogStandard.html %}

# JXES: JSON Support for the XES Event Log Standard


## Madhavi Bangalore Shankara Narayana, Hossameldin Khalifa, Wil van der Aalst


## Process and Data Science Department, RWTH Aachen University

## Process and Data Science department, Lehrstuhl fur Informatik 9 52074 Aachen, Germany

## Emails: madhavi.shankar@pads.rwth-aachen.de, hossameldin.khalifa@rwth-aachen.de, wvdaalst@pads.rwth-aachen.de


### 출처사이트 - [http://www.padsweb.rwth-aachen.de/wvdaalst/publications/p1115.pdf](http://www.padsweb.rwth-aachen.de/wvdaalst/publications/p1115.pdf)

###   출처 - http://www.padsweb.rwth-aachen.de/wvdaalst/publications/publications.html

Abstract

Process mining assumes the existence of an event log where each event refers to a case, an activity, and a point in
time. XES is an XML based IEEE approved standard format for
event logs supported by most of the process mining tools. JSON
(JavaScript Object Notation) is a lightweight data-interchange
format.

In this paper, we present JXES, the JSON standard
for the event logs and also provide implementation in ProM for
importing and exporting event logs in JSON format using 4 different parsers. The evaluation results show notable performance
differences between the different parsers (Simple JSON, Jackson,
GSON, Jsoninter).

Keywords–JXES; Event Log

format; IEEE Format

개요

프로세스 마이닝은 이벤트 로그가 있다고 가정합니다.
여기서 각 이벤트는 케이스, 활동 및 포인트를 나타냅니다.
XES는 대부분의 프로세스 마이닝 도구에서 지원하는 이벤트 로그들을 위한 XML 기반 IEEE 승인 표준 형식입니다.
JSON (JavaScript Object Notation)는 가벼운 데이터 교환 형식입니다.

본 논문에서는 이벤트 로그에 대한 JSON 표준인 JXES를 설명한다.
또한 4개의 다른 파서들를 사용하여 JSON 형식에서 이벤트 로그들을 가져오기와 내보내기를 ProM에서 구현을 제공합니다. 평가 결과는 서로 다른 파서들(단순 JSON, 잭슨,GSON, Jsoninter)간의 주목할 만한 성능 차이점들을 보여줍니다.

키워드–JXES; 이벤트 로그

형식; IEEE 형식

# 서론

I. INTRODUCTION

Process mining is a growing discipline in Data Science that
aims to use event logs obtained from information systems to
extract information about the business process.

A lot of research in Process mining has been about proposing techniques
to discover the process models starting from event logs.

I. 서론

프로세스 마이닝은 비즈니스 프로세스에 대한 정보를 추출하기 위해 정보 시스템으로 부터 얻은 이벤트 로그들을 사용하는 것이 목적인 데이터 과학에서 성장하고 있는 분야입니다.

프로세스 마이닝의 많은 연구는 이벤트 로그들로 부터 시작하는 프로세스 모델들을 도출하는 제안 기술에 관한 것이었습니다.


Event logs are being produced in research and practice,
each system group has developed their own logging mechanism for their logging system.

An event log can be seen as a collection of cases and a case can be seen as a trace/sequence of events.

Different researchers and companies use different formats for storing their event logs.

Event data can come from a database system, a CSV (comma-separated values), a spreadsheet, JSON file, a transaction log, a message log and even from APIs providing data from websites or social media.

이벤트 로그는 연구 및 실습에서 생성되고 있으며, 각 시스템 그룹은 로깅 시스템을 위한 고유한 로깅 메커니즘을 개발했습니다.

이벤트 로그는 케이스들의 모음으로 볼 수 있고 케이스는 이벤트들의 trace/시퀀스로 볼 수 있습니다.

다른 연구원들과 회사들은 이벤트 로그들을 저장하기 위해 다른 형식을 사용합니다.

이벤트 데이터는 데이터베이스 시스템, CSV(쉼표로 구분된 값), 스프레드시트, JSON 파일, 트랜잭션 로그, 메시지 로그는 물론 웹사이트나 소셜 미디어으로 부터 데이터를 제공하는 API에서도 생겨날 수 있습니다.



The XES (eXtensible Event Stream), an XML-based standard has been developed to support the easy exchange of event logs between different tools.

This format is supported by Celonis, Disco, UiPath, ProM, PM4Py, Apromore, QPR ProcessAnalyzer, ProcessGold, etc.

The JXES structure is based on a JSON Object, which contains all the necessary data and information for event logs.

ProM is a process mining framework that contains a large set of pre-processing, process discovery, conformance and performance/enhancement algorithms.

In this paper, we define the JSON format for event logs and also provide new plugin implementations for importing/exporting events logs with JSON format.

XES(eXtensible Event Stream), XML 기반 표준은 서로 다른 도구들 간에 이벤트 로그들을 쉽게 교환할 수 있도록 개발되었습니다.

이 형식은 Celonis, Disco, UiPath, ProM, PM4Py, Apromore, QPR ProcessAnalyzer, ProcessGold 등에서 지원됩니다.

JXES 구조는 모든 필요한 데이터와 이벤트 로그들의 정보를 포함하는 JSON 객체를 기반으로 합니다.

ProM은 사전 처리, 프로세스 도출, 적합성 및 성능/향상 알고리즘들의 대규모 세트를 포함하는 프로세스 마이닝 프레임워크입니다.

이 논문에서는 이벤트 로그들에 대한 JSON 형식을 정의하고 JSON 형식으로 이벤트 로그들을 가져오거나 내보내기 위한 새로운 플러그인 구현방법들도 제공합니다.

The reminder of this paper is structured as follows.

In Section II, we discuss the related work and also define the JSON file standard for event log.

In Section III, we describe the parser implementation.

In Section IV we discuss the evaluation criteria.

In Section V we provide information on accessing this plugin and also its demonstration information and Section VI concludes this paper.

이 논문의 상기시키는 것은 다음과 같이 구성되어 있다.

섹션 II에서는, 관련 작업에 대해 논의하고 이벤트 로그에 대한 JSON 파일 표준을 정의한다.

섹션 III에서는, 파서 구현에 대해 설명합니다.

섹션 IV에서 우리는 평가 기준에 대해 논의합니다.

섹션 V에서는 이 플러그인에 접속하는 방법과 입증 정보를 제공하고 섹션 VI에서 이 논문을 마무리합니다.
