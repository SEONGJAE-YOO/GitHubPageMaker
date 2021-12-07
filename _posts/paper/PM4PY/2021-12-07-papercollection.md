---
layout: page
current: page
cover:  assets/built/images/book.png
navigation: True
title: 프로세스 마이닝 과제를 위해 다양한 논문 공부 및 정리 내용
tags: [thesis]  
class: post-template
subclass: 'post tag-thesis'  
author: SeongJae Yu    
sitemap:
lastmod: 2021-12-07
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---

# The Process Mining ToolKit (PMTK): Enabling Advanced Process Mining in an Integrated Fashion 요약 및 정리



(Extended Abstract)
Alessandro Berti∗
, Chiao-Yun Li†
, Daniel Schuster‡
, and Sebastiaan J. van Zelst (B)
§


Fraunhofer FIT, Birlinghoven Castle, Sankt Augustin, Germany
Process and Data Science (PADS) Chair, RWTH Aachen University, Germany
Email: {alessandro.berti∗
,chiao-yun.li†
,daniel.schuster‡
,sebastiaan.van.zelst§}@fit.fraunhofer.de

출처 - [https://icpmconference.org/2021/wp-content/uploads/sites/5/2021/10/M-The-Process-Mining-ToolKit-PMTK-Enabling-Advanced-Process-Mining-in-an-Integrated-Fashion.pdf](https://icpmconference.org/2021/wp-content/uploads/sites/5/2021/10/M-The-Process-Mining-ToolKit-PMTK-Enabling-Advanced-Process-Mining-in-an-Integrated-Fashion.pdf)

# Abstract

Heaps of event data are being generated and stored
during the execution of (business) processes.

Over the recent years, various process mining solutions have been developed.

Therefore, this paper presents the
Process Mining ToolKit, i.e., PMTK, intended to bridge the gap
mentioned. Building on top of the open-source project PM4Py,
PMTK presents novel process mining algorithms and techniques
in an easy-to-use, fully integrated solution.
Index Terms—process mining, process analytics, visual analytics, data science


이벤트 데이터 힙이 (비즈니스) 프로세스를 실행하는 동안 생성 및 저장됩니다.

최근에 수년 동안 다양한 프로세스 마이닝 솔루션이 개발되었습니다.

따라서 이 논문은 프로세스 마이닝 ToolKit, 즉 PMTK는 격차를 해소하기 위한 것입니다.
말하는. 오픈 소스 프로젝트 PM4Py를 기반으로 구축,
PMTK는 새로운 프로세스 마이닝 알고리즘 및 기술을 제시합니다.
사용하기 쉽고 완전히 통합된 솔루션입니다.
색인 용어 - 프로세스 마이닝, 프로세스 분석, 시각적 분석, 데이터 과학


# I. INTRODUCTION

The execution of (business) processes generates digital
records of historical process behavior, i.e., referred to as event
data. Process mining [1] is concerned with developing techniques and methods that can translate such data into actionable
knowledge of the process.

Examples of typical process mining
techniques include process discovery, i.e., automated discovery
of process models describing the process based on the event
data, and conformance checking, i.e., assessing whether the
execution of a process as recorded in the event data conforms
to a given reference model.

Over the recent years, various
academic and commercial software solutions have been proposed, implementing process mining technology. Commercial
solutions, such as Celonis (http://celonis.com), UiPath (http://
uipath.com), Fluxicon Disco (http://fluxicon.com/disco/), etc.,
often provide basic process discovery functionalities and
various (customizable) statistics of the process. Academical
tools such as ProM [2] (http://promtools.org), Apromore [3]
(http://apromore.org), PM4Py [4] (http://pm4py.org) and bupaR [5] (http://bupar.net) are often open-source and implement
a wider range of process mining technologies. Most of these
solutions are hard to integrate into a business context or require
extensive knowledge of a specific programming language to
be used. To bridge this gap, we present the Process Mining
ToolKit (PMTK). PMTK is built on top of the PM4Py library,
i.e., extending our earlier work presented in [6].


(비즈니스) 프로세스의 실행은 디지털
이벤트라고 하는 역사적 프로세스 행동의 기록
데이터. 프로세스 마이닝[1]은 이러한 데이터를 실행 가능한 것으로 변환할 수 있는 기술 및 방법 개발과 관련이 있습니다.

일반적인 프로세스 마이닝의 기술에는 프로세스 도출,
이벤트를 기반으로 프로세스를 설명하는 프로세스 모델,
데이터 및 적합성 검사, 즉
이벤트 데이터에 기록된 대로 프로세스 실행
주어진 참조 모델에. 최근 몇 년 동안 다양한
프로세스 마이닝 기술을 구현하는 학술 및 상용 소프트웨어 솔루션이 제안되었습니다.

Celonis(http://celonis.com), UiPath(http://
uipath.com), Fluxicon Disco(http://fluxicon.com/disco/) 등,
종종 기본적인 프로세스 검색 기능을 제공하고
프로세스의 다양한 (사용자 정의 가능한) 통계. 학문적
ProM[2](http://promtools.org), Apromore[3]와 같은 도구
(http://apromore.org), PM4Py [4] (http://pm4py.org) 및 bupaR [5] (http://bupar.net)은 종종 오픈 소스이며
더 넓은 범위의 프로세스 마이닝 기술을 구현합니다.

이들 대부분은 솔루션은 비즈니스에 통합하기 어렵거나
특정 프로그래밍 언어에 대한 광범위한 지식
사용된다. 이 격차를 해소하기 위해 우리는 프로세스 마이닝 도구를 제시합니다.
툴킷(PMTK). PMTK는 PM4Py 라이브러리 위에 구축되었으며,
즉, [6]에 제시된 이전 작업을 확장합니다.


# II. TOOL OVERVIEW

In this section, we present a short overview of the core
components of the PMTK tool. A screen recording corresponding to this extended abstract can be found at https://
pmtk.fit.fraunhofer.de/icpm21/demo.mp4.1

We briefly discuss
the overall architecture of PMTK and its main functionalities,
i.e., the work space, the main analysis capabilities and the
integrated filtering.

1) Architecture:

Conceptually, PMTK consists of three different layers: an algorithmic layer (based on PM4Py), a web
service layer (i.e., a controller, based on [6]) and a front-end
layer built using web technologies such as HTML5, Javascript
and Angular. PMTK is available as a standalone tool, i.e.,
including the web services and the web interface, and as a
web application which can be deployed on any application
server.

2) The Work Space:

PMTK provides a work space in which
the user is able to organize various files. Consider Figure 1a,
in which we show a snapshot of the work space. In the work
space, the user can create a folder for each process she is
intending to analyze. Subsequently, various objects, e.g., event
logs and filters can be stored in the corresponding process’
folder. Some objects, e.g., event logs can be imported from
disk, other objects can be generated from within PMTK.

3) Analysis Capabilities:

When the user selects an object
from the work space, various analyses can be applied, based
on the selected object. Currently, the user is able to execute
the following analyses:


이 섹션에서는 핵심에 대한 간략한 개요를 제시합니다.
PMTK 도구의 구성 요소. 이 확장된 초록에 해당하는 화면 녹화는 https://pmtk.fit.fraunhofer.de/icpm21/demo.mp4.1 에서 찾을 수 있습니다.

간단히 PMTK의 전체 아키텍처와 주요 기능,
작업 공간, 주요 분석 기능 및
통합 필터링을 논의합니다.

1) 아키텍처:

개념적으로 PMTK는 세 가지 다른 계층으로 구성됩니다. 알고리즘 계층(PM4Py 기반), 웹
서비스 계층(즉, [6]에 기반한 컨트롤러) 및 프런트 엔드
HTML5, Javascript와 앵귤러 같은 웹 기술을 사용하여 구축된 레이어
웹 서비스 및 웹 인터페이스를 포함하여
모든 애플리케이션에 배포할 수 있는 웹 애플리케이션으로
PMTK는 독립 실행형 도구로 사용할 수 있습니다.


2) 작업 공간:

PMTK는 사용자가 다양한 파일을 구성할 수 있는 작업 공간을 제공합니다.
그림 1a를 고려하십시오.
작업 공간의 스냅샷을 보여줍니다.

작업중
공간에서 사용자는 각 프로세스에 대한 폴더를 만들 수 있습니다.

이 후, 이벤트와 같은 다양한 객체
로그 및 필터는 해당 프로세스 폴더에 저장할 수 있습니다

이벤트 로그와 같은 일부 개체는 디스크에서 가져올 수 있습니다.

3) 분석 능력:

사용자가 작업 공간에서 객체를 선택했을 때,
다양한 분석을 적용할 수 있습니다.

현재 사용자는 다음 분석을 실행할 수 있습니다.



• Statistics; PMTK provides various typical event log
statistics, i.e., absolute/relative activity occurrences, an
overview of the average events per case, events/case
arrivals/active cases over time and throughput statistics.


• Log Exploration; PMTK provides means to explore the
event log in detail, i.e., to gain a better understanding of
the process captured by the event log. Currently, PMTK
implements the following log exploration functionalities:

• 통계; PMTK는 다양한 일반적인 이벤트 로그 통계를 제공합니다.
즉 절대적/상대적 활동 발생,
사례별 평균 이벤트 개요, 이벤트/케이스,
시간 경과에 따른 도착/활성 케이스 및 처리량 통계.


• 로그 탐색; PMTK는
이벤트 로그의 프로세스를 더욱 상세히 얻기위해 이벤트 로그를 자세히 설명합니다. 현재 PMTK
다음 로그 탐색 기능을 구현합니다.

Variant Explorer: In the variant explorer, the user is
able to consult what cases follow the same control-flow
behavior. See Figure 1b, in which we present a small
screenshot of the variant explorer functionality; Dotted
Chart: PMTK implements the dotted chart analysis, i.e., a
visualization of events over time [7]; Performance View:
PMTK implements the performance spectrum, i.e., as
described in [8].


• Process Map; PMTK implements a process map with various filtering options (i.e., filtering of edges and activities).
The layout algorithm implemented is based on [9].


변형 탐색기: 변형 탐색기에서 사용자는
동일한 제어 흐름을 따르는 사례를 참조할 수 있음
그림 1b를 참조하십시오.

변형 탐색기 기능의 스크린샷 점이 찍힌
차트: PMTK는 점 차트 분석을 구현합니다.
시간 경과에 따른 이벤트 시각화 [7]; 성능 보기:
PMTK는 성능 스펙트럼을 구현합니다.
[8]에 설명되어 있습니다.


• 프로세스 맵; PMTK는 다양한 필터링 옵션(즉, 가장자리 및 활동 필터링)으로 프로세스 맵을 구현합니다.
구현된 레이아웃 알고리즘은 [9]를 기반으로 합니다.

4) Integrated Filtering:

In PMTK, event data filtering is
considered a primary citizen.

As such, various event data
filtering functionalities have been implemented. The user is
able to specify custom filters, e.g., based on start/end activities,
time-ranges, etc.

Most of the analysis functionalities described
in subsubsection II-3, provide interactive filtering functionality
as well.

The filters created can be stored in the work space,
e.g., to be re-applied at a later stage of the analysis.

4) 통합 필터링:

PMTK에서 이벤트 데이터 필터링은 기본 citizen으로 간주됩니다.

이와 같이, 다양한 이벤트 데이터 필터링 기능이 구현되었습니다. 사용자는 시작/종료 활동, 시간 범위 등을 기반으로 사용자 정의 필터를 지정할 수 있습니다.

하위 섹션 II-3에 설명된 대부분의 분석 기능은 대화형 필터링 기능도 제공합니다. 생성된 필터는 분석의 나중 단계에서 다시 적용하기 위해 작업 공간에 저장할 수 있습니다.



# III. CONCLUSION

Various software solutions exist that are able to translate
recorded event data into operation insights into the historical
execution of a process.

However, commercial applications only
offer a marginal fraction of the algoritmic possiblities, i.e.,
available in the process mining literatrue.

Academic and open
source solutions do provide a larger range of functionalities,
yet, often in a non-intuitive manner.

In this paper, we have
presented the Process Mining ToolKit (PMTK), which aims
to bridge this gap, i.e., integrating advanced algorithms in
an integrated, user-friendly environment. As such, PMTK, can
be seen as a front-end solution for the advanced open source
process mining library PM4Py.
Tool Maturity & Novelty: The Fraunhofer FIT process
mining team has developed PMTK to provide an extensible,
customizable, easy-to-maintain product to its R&D project
partners. Compared to [6], the web-service architecture has
been redesigned to increase the tool’s modularity. We have
adopted object relational mapping for multi-database support,
offering support for different artefacts (e.g., the integrated
filters) for the same process, i.e., exposed as the work space.

# III. 결론

기록된 이벤트 데이터를 운영 통찰력으로 번역할 수 있는 다양한 소프트웨어 솔루션이 존재합니다.

그러나, 상업용 애플리케이션만
알고리즘 가능성의 한계 부분을 제공합니다.

즉, 프로세스 마이닝 문헌에서 사용할 수 있습니다.

학업 및 오픈
소스 솔루션은 더 넓은 범위의 기능을 제공합니다.
그러나 종종 직관적이지 않은 방식으로 제공합니다.



# Process Mining in the Coatings and Paints Industry: The Purchase Order Handling Process 요약 및 정리

Business Process Intelligence Challenge 2019
Peyman Badakhshan, Sam Gosling, Jerome Geyer-Klingeberg,
Janina Nakladal, Johannes Schukat, Jennifer Gsenger
Celonis SE, Theresienstraße 6, 80333 Munich, Germany
{p.badakhshan, s.gosling, j.geyerklingeberg,
j.nakladal, j.schukat, j.gsenger}@celonis.com




- 출처 - [https://icpmconference.org/2019/wp-content/uploads/sites/6/2019/07/BPI-Challenge-Submission-4.pdf](https://icpmconference.org/2019/wp-content/uploads/sites/6/2019/07/BPI-Challenge-Submission-4.pdf)

# Abstract.

The Business Process Intelligence (BPI) challenge is an annual competition in process mining that is co-located with the International Conference on
Process Mining (ICPM). BPI 2019 is providing participants with a real event log
from the Purchase-to-Pay process of a Dutch company in the field of coatings
and paints.

The process owner is interested to gain insights into the process from
three perspectives.

First, discovering various process models referring to different use cases. Second, focusing on throughput time of the invoicing process.
Third, detecting deviations based on the expected process model. The aim of this
paper is to report the insights and results derived from a comprehensive process
analysis using the Celonis Intelligent Business Cloud (IBC). The report also discusses limitations due to process and data characteristics and outlines recommendations for additional data collection and analysis

비즈니스 프로세스 인텔리전스(BPI) 챌린지는 프로세스 마이닝 분야에서 매년 열리는 국제 회의와 함께 개최되는 대회입니다.
프로세스 마이닝(ICPM). BPI 2019는 참가자에게 실제 이벤트 로그를 제공합니다


세 가지 관점. 첫째, 다양한 사용 사례를 참조하는 다양한 프로세스 모델을 발견합니다. 둘째, 송장 발행 프로세스의 처리 시간에 중점을 둡니다.
셋째, 예상되는 프로세스 모델을 기반으로 편차를 감지합니다. 이 논문의 목적은
Celonis Intelligent Business Cloud(IBC)에서 사용한 포괄적인 프로세스 분석으로부터 파생된 통찰력과 결과를 보고하는 것입니다.
.

또한 프로세스 및 데이터 특성으로 인한 제한 사항에 대해 설명하고 추가 데이터 수집 및 분석에 대한 권장 사항을 설명합니다.


# 1 Introduction

The Purchase-to-Pay (P2P) process is a challenge for all organizations. This challenge
starts with the creation of purchase requisitions and their time-consuming approval processes. It follows with supplier relationship management and ends with handling the
invoices and their respective related subprocesses. Thereby, it is crucial for organizations to deal with lack of ownership and maintain good master data on suppliers, enable
a proper process and data governance, provide suppliers an automated ordering process,
better contract management, and better visibility of payment processing times.

Process mining has proved to be an innovative and efficient method to discover,
analyze, and predict business process behavior [1]. For the analysis of the BPI process
data, we structure our work by two perspectives. On the one hand, we consider the three
main types of process mining including discovery, conformance checking, and enhancement. The process mining types, as described in Figure 1, enable us to discover
2
the as-is process, to conduct process analytics, and to recommend necessary actions for
process improvement.


P2P(구매 후 지불) 프로세스는 모든 조직의 도전과제입니다. 이 도전과제는
구매 요청 작성 및 시간 소모적인 승인 프로세스로 시작합니다.

공급자 관계 관리로 이어지며 송장 및 관련 하위 프로세스 처리로 끝납니다.

따라서 조직이 소유권이 낮은것을 처리하고 공급업체에 대한 우수한 마스터 데이터를 유지 관리하는 것,적절한 프로세스 및 데이터 통치, 공급업체에 자동화된 주문 프로세스를 제공하는 것,더 나은 계약 관리 및 지불 처리 시간의 더 나은 가시성이 중요합니다.

프로세스 마이닝은 혁신적이고 효율적인 방법으로 입증되었습니다.



# 2 Management summary

Using various process mining methods, we answer the process owner’s questions and
provide several suggestions with the aim of giving more insights to the process owner.

Regarding process discovery, we report one general model that represents the full process along with four additional models representing in-depth views.

This process model satisfies four different scenarios including “3-way matching, invoice after goods receipt”, “3-way matching, invoice before goods receipt”, “2-way matching (no goods
receipt needed)”, and “Consignment”. We also design the models using BPMN notation
and use them as a reference model for conformance checking.

Regarding conformance checking, we identify violations to the reference process
using root cause analysis and the Celonis Process AI feature along with detecting deviations from the expected process model.

We also take advantage of the First-Time-Right (FTR) analysis for further detection of compliance issues. Finally, we approach
the enhancement of the process using cycle time analysis.

다양한 프로세스 마이닝 방법을 사용하여, 프로세스 소유자의 질문에 답변하고
프로세스 소유자에게 더 많은 통찰력을 제공하기 위해 몇 가지 제안을 제공합니다.

프로세스 도출과 관련하여 ,우리는 전체 프로세스를 나타내는 하나의 일반 모델과 함께 심층 보기를 나타내는 4개의 추가 모델을 보고합니다.

이 프로세스 모델 "3-way 매칭, 입고 후 송장", "3-way 매칭, 입고 전 송장", "2-way 매칭(상품 없음
영수증 필요)” 및 “위탁”. 또한 BPMN 표기법을 사용하여 모델을 설계합니다.
적합성 검사를 위한 참조 모델로 사용합니다.

적합성 확인과 관련하여, 참조 프로세스에 대한 위반 사항을 식별합니다.
근본 원인 분석 및 Celonis Process AI 기능을 사용하여 예상되는 프로세스 모델과의 편차를 감지합니다.

또한 규정 준수 문제를 추가로 감지하기 위해 FTR(First-Time-Right) 분석을 활용합니다. 마지막으로, 우리는
주기 시간 분석을 사용하여 프로세스를 개선합니다.


# 2.1 Analysis overview

In the following section, we explain the applied analyses as well as the main findings.

Cycle time analysis. With the recent focus on cost reduction, many organizations
are forced to identifying opportunities for long-term savings.

One way to cut costs is
by decreasing the cycle times associated with procuring materials and services.

Cycle
times provide critical information on an organization’s procurement efficiency. With
the help of process mining, purchasing organizations can realize shorter cycle times (i)
by making their and their suppliers’ procurement efficiency transparent, and (ii) by
suggesting measures to improve this efficiency

The cycle time analysis on the provided data set and specific activities is summarized
as the following:
1. The average throughput time from “Record Goods Receipt” to “Record Invoice
   Receipt” is 19.9 days; the median throughput time is 9.8 days.
2. The average duration between the creation of the invoice in the source system
   (Record Invoice Receipt) and the clearing of the invoice is 47.9 days; the median
   is 41.9 days.


다음 섹션에서는 응용 분석과 주요 결과에 대해 설명합니다.

주기 시간 분석. 최근 비용 절감에 중점을 두면서, 많은 조직에서
장기적으로 저축할 기회를 찾을것을 강조합니다.

비용을 줄이는 한 가지 방법은
자재 및 서비스 조달과 관련된 주기 시간을 줄임으로써 주기
시간은 조직의 효율성에 대한 중요한 정보를 제공합니다. 프로세스 마이닝의 도움으로, 구매 조직은 더 짧은 주기 시간(i)을 실현할 수 있습니다. 그리고 공급자의 조달 효율성을 투명하게 만들고 (ii)
이 효율성을 개선하기 위한 조치를 제안하게 되었습니다.

제공된 데이터 세트 및 특정 활동에 대한 주기 시간 분석이 요약됩니다.
다음과 같이:

1. "상품 입고 기록"에서 "송장 기록"까지의 평균 처리 시간
   영수증'은 19.9일입니다. 중간 처리 시간은 9.8일입니다.

2. 소스 시스템에서 송장 생성 사이의 평균 기간
   (영수증 기록) 및 영수증 정산은 47.9일입니다. 중앙값
   41.9일입니다.

# First-Time-Right (FTR) analysis.

Procurement quality means that the procurement
organization ensures to procure the right product, in the right quantity and quality, in
the right place, at the right time, and at the best price.

The goal of the FTR analysis is to evaluate how well the purchasing process really works - by calculating how often
purchase orders run through the process without being touched by rework to change
incorrect orders or recurrent activities.

Importantly, those cases that require additional
effort are analyzed and their root causes and/or potential drivers are identified.

A main finding from this analysis is that many cases are labeled with their respective
classification, however, they are used to clear debit memos created by a vendor.

The suggestion here is to create a specific memo clearing process, which enables an accurate
conformance calculation for these types of process flows.

Another finding is that for those cases where rework activities occur, the process
flow conformance ratio increases.

This is interesting as the ROI for additional manual
effort can be tracked, i.e. when an employee makes a price change after receiving the
invoice, it is done so to comply with the process flow process.

It is advised that the company investigates further what the root causes of these changes are, which has already been started with the analysis below.


조달 품질은 조달
조직은 적절한 장소, 적절한 시간, 최적의 가격으로 올바른 수량과 품질로 조달할 수 있도록 보장합니다.

FTR 분석의 목표는 구매 프로세스가 실제로 얼마나 잘 작동하는지 평가하는 것입니다.
구매 주문은 잘못된 주문 또는 반복적인 활동 변경을 위한 재작업 없이 프로세스를 통해 실행됩니다.

중요한 것은 추가로 필요한 경우 근본 원인 및/또는 잠재적 동인이 식별됩니다.

이 분석의 주요 발견은 많은 사례가 각각의 레이블로 표시된다는 것입니다.

여기서 제안하는 것은 정확하고 이러한 유형의 프로세스 흐름에 대한 적합성 계산 및 메모 삭제 프로세스를 생성하는 것입니다.


또 다른 발견은 재작업 활동이 발생하는 경우 프로세스가
흐름 적합성 비율이 증가합니다.

이것은 추가 매뉴얼에 대한 ROI로서 흥미롭습니다.
직원이 영수증을 받은 후 가격을 변경할 때 트레이스를 추적할 수 있습니다.


# Business Process Intelligence Challenge 2019: Process discovery and deviation analysis of purchase order handling process 요약 및 정리

Jongchan Kim, Jonghyeon Ko, and Suhwan Lee
Ulsan National Institute of Science and Technology, 50 UNIST-gil, Ulsan 44919,
Republic of Korea
{jckim,whd1gus2,ghksdl6025}@unist.ac.kr


- 출처 [https://icpmconference.org/2019/wp-content/uploads/sites/6/2019/07/BPI-Challenge-Student-Submission-3.pdf](https://icpmconference.org/2019/wp-content/uploads/sites/6/2019/07/BPI-Challenge-Student-Submission-3.pdf)

# Abstract.

Process mining is of paramount importance as the number
of event logs cumulated in information systems is growing exponentially
over time.

Increased number of event logs implies that the amount of information that can be mined to be used to meet the stakeholders requirements through the generation of qualitative and quantitative outputs enhances.

In compliance with the goal of the Business Process Intelligence
Challenge to make the result of the analysis useful in the real world, we
take a real-life event log of a company operating in the Netherlands in
order to analyze and suggest improvements to the companys purchase
order handling process for its subsidiaries.

In this paper, we discover and
refine business process models that encompass the four types of flows
in the event log.

Then, we also identify different types of deviation in
the event log from the discovered processes and their extents based on
alignment techniques and various statistical tests.

정보 시스템에 누적되는 이벤트 로그가 기하급수적으로 증가하고 있습니다.

이벤트 로그의 수가 증가한다는 것은 정량적 출력 생성을 통해 이해 관계자 요구 사항을 충족하는 데 사용하기 위해 마이닝할 수 있는 정보의 양이 증가한다는 것을 의미합니다.

비즈니스 프로세스 인텔리전스의 목표에 따라
분석 결과를 실생활에 유용하게 만들기 위한 도전,
네덜란드에서 운영되는 회사의 실제 이벤트 로그를 가져옵니다.

이 논문에서 우리는 발견하고
이벤트 로그에서 네 가지 유형의 흐름을 포함하는 비즈니스 프로세스 모델을 개선합니다.

그런 다음, 발견된 프로세스의 이벤트 로그 및 해당 범위를 정렬 기술 및 다양한 통계 테스트 기반으로 다른 유형의 편차도 식별합니다.


# 2.3 Data exploration



```python
from IPython.display import Image  # 주피터 노트북에 이미지 삽입
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211206_180211_1.png")
```





![output_20_0](./img/thesis/papercollect/output_20_0.png)




# 2.4 Four types of flows

The event log contains four types of flows that can be defined by different combinations of three features, case Goods Receipt, case GR-Based Inv. Verif, and Item Type.

(1) 3-way matching, invoice after goods receipt: One of subsidiaries
submits a purchase order for a specific item to a vendor.

Then, the vendor will create an invoice and send the ordered item to the company. After receiving
the item, the subsidiary will check the list of materials with the purchase
order document and the invoice receipt. The company can revise the price
or quantity of the item before receiving the ordered item.

For the final step, the company will completely clear this order, if there is no problem on the
invoicing process.

(2) 3-way matching, invoice before goods receipt: This flow is similar
to 3-way matching, invoice after goods receipt, except for the fact that invoices can be received before receiving goods receipts. As in 3-way matching,
invoice after goods receipt, the subsidiary will completely clear the order if
there is no problem on this invoicing process.

(3) 2-way matching: In this process, goods receipt message is not required
as all orders made in this flow are framework orders.

(4) Consignment: As the name of the flow implies, invoice messages are
not required since it is handled in a separate process operated by the third party company. Therefore, messages on goods receipt can only be observed
in this flow.

이벤트 로그에는 항목 형식, 케이스 입고, 케이스 GR-Based Inv의 세 가지 기능의 서로 다른 조합으로 정의할 수 있는 네 가지 유형의 흐름이 포함되어 있습니다.


(1) 3-way 매칭, 입고 후 영수증 : 자회사 중 하나의 회사에게
특정 품목에 대한 구매 주문서를 공급업체에 제출합니다.

그러면 판매자는 영수증을 작성하고 주문한 물품을 회사에 보냅니다. 물품을 받은 후
, 자회사는 구매와 함께 물품 목록와 주문 문서 및 송장 영수증을 확인합니다.

회사는 주문한 물품을 받기 전에 물품의 양 또는 가격을 수정할 수 있습니다.

마지막 단계의 경우, 회사는 문제가 없으면 이 주문을 완전히 끝마칠수 있다.


(2) 3방향 매칭, 입고 전 영수증 : 이 흐름도가

상품 수령 전에 영수증을 수령할 수 있다는 사실을 제외하고. 3-way 매칭과 유사하다

영수증이 문제가 없으면, 자회사는 주문을 완전히 끝 마칠수 있다.

(3) 2-way 매칭 : 이 과정에서, 입고 메시지가 필요하지 않습니다.
이 흐름에서 이루어진 모든 주문은 프레임워크 주문이기 때문입니다.

(4) 위탁: 흐름에서 알 수 있듯이 영수증 메시지는
제3자 회사에서 운영하는 별도의 프로세스로 처리되기 때문에 필요하지 않습니다. 따라서 이 흐름에서 입고 메시지만 관찰할 수 있습니다.



```python
from IPython.display import Image  # 주피터 노트북에 이미지 삽입
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211206_181840_1.png")
```





![output_22_0](./img/thesis/papercollect/output_22_0.png)




# 3 Building a business process model

Question 1. Is there a collection of process models which together properly
describe the process in this data. Based on the four categories above, at least
4 models are needed, but any collection of models that together explain the
process well is appreciated.

Preferably, the decision which model explains which purchase item best is based on properties of the item.

The BPMN model was derived from petri net constructed using ProM as in Fig.3.
Since the BPMN model is very complicated as it encompasses all four types of
flows, subprocesses are introduced as in Fig. 4 to enhance the understandability
by making the BPMN model simple and neat. Fig. 5 (a) is a subprocess called
SRM that can be found in 3-way match, before GR. Fig. 5 (b) is a subprocess
called SRM(Transfer followed by order) that can be found in 3-way match, after
GR. Fig. 5 (c) is a subprocess called Vendor Creates Invoice & Record Goods
Receipt (Goods) that can be found in more than two types of flow, and this
subprocess deals with goods. Fig.5 (d) is a subprocess called Vendor Creates
Invoice & Record Goods Receipt (Service) that can be found in more than two
types of flow, and this subprocess deals with services. Fig. 5 (e) is a subprocess
called Record Goods & Invoice receiptthat can be found in more than two types
of flow. In Fig. 5 (c) to Fig. 5 (e), “&” means that any of two activities can have
chance to happen earlier than the other activity.


질문 1. 이 데이터에서 프로세스를 설명하는 프로세스 모델의 모음이 있습니까?
위의 네 가지 범주를 기반으로, 적어도
4개의 모델이 필요하지만, 프로세스를 잘 평가하는 모델 모음이 있다,

바람직하게는, 어떤 모델이 어떤 구매 물품을 가장 잘 설명하는지 결정하는 것은 물품의 속성을 기반으로 합니다.

BPMN 모델은 그림 1과 같이 ProM을 사용하여 구축된 페트리넷에서 파생되었습니다.

3. BPMN 모델은 4가지 유형을 모두 포함하여 매우 복잡하기 때문에,
   이해도를 높이기 위해 그림 4와 같은 하위 프로세스를 도입했습니다.
   BPMN 모델을 간단하고 깔끔하게 만들어줍니다.

그림 5(a)는 다음과 같은 하위 프로세스입니다.
GR 이전의 3way 매치에서 볼 수 있는 SRM으로 불리는 서브프로세스이다.

그림 5(c)는 2가지 이상의 흐름에서 볼 수 있는 영수증(상품)의 Vendor Creates Invoice & Record Goods라는 하위 프로세스입니다.

그림 5(d)는 Vendor Creates invoice & Record Goods Receipt라는 하위 프로세스입니다.




```python
from IPython.display import Image  # 주피터 노트북에 이미지 삽입
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211206_183747_1.png")
```





![output_24_0](./img/thesis/papercollect/output_24_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211206_183747_2.png")
```





![output_25_0](./img/thesis/papercollect/output_25_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211206_183747_3.png")
```





![output_26_0](./img/thesis/papercollect/output_26_0.png)




# Visualizing Trace Variants From Partially Ordered Event Data


Daniel Schuster
1
,2[0000
−0002
−6512
−9580], Lukas Schade
2
, Sebastiaan J. van
Zelst
1
,2[0000
−0003
−0415
−1036], and Wil M. P. van der Aalst
1
,2[0000
−0002
−0955
−6940]
1 Fraunhofer Institute for Applied Information Technology FIT, Germany {daniel.schuster,sebastiaan.van.zelst}@fit.fraunhofer.de 2 RWTH Aachen University, Aachen, Germany
wvdaalst@pads.rwth-aachen.de


출처 - [https://arxiv.org/pdf/2110.02060.pdf](https://arxiv.org/pdf/2110.02060.pdf)

# 개요

Executing operational processes generates event data, which
contain information on the executed process activities.

Process mining
techniques allow to systematically analyze event data to gain insights
that are then used to optimize processes.

Visual analytics for event data
are essential for the application of process mining.

Visualizing unique process executions—also called trace variants, i.e., unique sequences of executed process activities—is a common technique implemented in many
scientific and industrial process mining applications.

Most existing visualizations assume a total order on the executed process activities, i.e.,
these techniques assume that process activities are atomic and were executed at a specific point in time. In reality, however, the executions of
activities are not atomic.

Multiple timestamps are recorded for an executed process activity, e.g., a start-timestamp and a complete-timestamp.

Therefore, the execution of process activities may overlap and, thus, cannot be represented as a total order if more than one timestamp is to be
considered.

In this paper, we present a visualization approach for trace
variants that incorporates start- and complete-timestamps of activities.



운영 프로세스를 실행하면 실행된 프로세스 활동에 대한 정보가 포함된 이벤트 데이터가 생성됩니다. 프로세스 마이닝 기술을 사용하면 이벤트 데이터를 체계적으로 분석하여 프로세스를 최적화하는 데 사용되는 통찰력을 얻을 수 있습니다. 이벤트 데이터에 대한 시각적 분석은 프로세스 마이닝 적용에 필수적입니다. 고유한 프로세스 실행(추적 변형이라고도 함, 즉 실행된 프로세스 활동의 고유한 시퀀스)을 시각화하는 것은 많은 과학 및 산업 프로세스 마이닝 응용 프로그램에서 구현되는 일반적인 기술입니다.

대부분 나와있는 기존 시각화 기능은 실행된 프로세스 활동에 대한 전체 순서만 나타냅니다. 즉, 기존 시각화 기능은 프로세스 활동이 극소적이며 특정 시점에서만 실행되는 것을 보여줍니다.

그러나 현실에서는 프로세스 활동은 극소적이지 않습니다.

그리고 실행된 프로세스 활동에 대해 여러 타임스탬프가 기록됩니다. 예를 들어 시작 타임스탬프 및 완료 타임스탬프가 있습니다.

따라서 하나 이상의 타임스탬프를 고려해서 프로세스 활동의 실행이 겹치지 않게 해야합니다.


이 논문에서는 활동의 시작 및 완료 타임스탬프를 통합하는 트레이스 변형에 대한 시각화 접근 방식을 설명합니다.
