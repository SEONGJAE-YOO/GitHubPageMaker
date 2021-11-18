---
layout: page
current: page
cover:  assets/built/images/book.png
navigation: True
title: JSON Support for the XES Event Log Standard - SUPPORTING XES USING JSON
tags: [thesis]  
class: post-template
subclass: 'post tag-thesis'  
author: SeongJae Yu    
sitemap:
lastmod: 2021-11-18
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include JSONSupportfortheXESEventLogStandard.html %}

# JSON Support for the XES Event Log Standard -  SUPPORTING XES USING JSON



The XES standard defines a grammar for a tag-based language whose aim is to provide designers of information systems with a unified and extensible methodology for capturing systems behaviors by means of event logs and event streams.

But, not all information systems support XES format and would be beneficial to extend the XES semantic to JSON format.

An XML schema shown in Figure 1 describes the structure of an event log.

XES 표준은 이벤트 로그들 및 이벤트 스트림들을 통해 시스템 동작들을 차지하기 위한 통합되고 확장 가능한 방법론을 정보 시스템 설계자들에게 제공하는 것을 목표로 하는 태그 기반 언어의 문법을 정의합니다.

그러나 모든 정보 시스템이 XES 형식을 지원하는 것은 아니며 XES 의미론적인 형식을 JSON 형식으로 확장하는 것이 좋습니다.

그림 1에 표시된 XML 스키마는 이벤트 로그의 구조를 설명합니다.

* methodology : 방법론
* semantic : 의미론적인


```python
from IPython.display import Image  # 주피터 노트북에 이미지 삽입
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211117_182932_1.png")
```





![output_2_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_2_0.png)




OpenXES is a standard implementation of XES.

OpenXES stores the event log in main memory using the standard classes provided by the Java collections framework.

This simplifies the development and works well with small event logs typically used as examples for research purposes.

However, when using OpenXES for large and complex real-life event logs (e.g., 10 million events with 3 attributes each), the
available main memory on a typical workstation (e.g., 4 GB) is insufficient to load the event log.

XESLite can handle large event logs and overcome the drawback of memory issue.

DB-XES is a database schema which resembles the standard XES structure.

OpenXES는 XES의 일반적인 구현입니다.

OpenXES는 Java 컬렉션 프레임워크에서 제공하는 표준 클래스를 사용하여 메인 메모리에 이벤트 로그를 저장합니다.

이것은 개발을 단순화하고 일반적으로 연구 목적의 예로 사용되는 작은 이벤트 로그들와 잘 작동합니다.

그러나 크고 복잡한 실제 이벤트 로그들(예: 각각 3개의 속성이 있는 천만 개의 이벤트)에 OpenXES를 사용할 때,
일반적인 워크스테이션에서 사용 가능한 주 메모리(예: 4GB)는 이벤트 로그를 로딩하기에 충분하지 않습니다.

XESLite는 대용량 이벤트 로그들을 처리하고 메모리 문제의 단점을 극복합니다.

DB-XES는 일반적인 XES 구조와 유사한 데이터베이스 스키마입니다.

JSON is an open standard lightweight file format commonly used for data interchange.

It uses human-readable text to store and transmit data objects.

Hence, the motivation to define a JSON event log format and create a plugin lies in the fact that we can easily use the JSON logs.

We have used the 4 design principles defined for the XES format, namely, (i) Simplicity, (ii) Flexibility, (iii) Extensibility and (iv) Expressivity.

This helped us to make design decisions with respect to defining the JXES standard format as well as evaluate the implementations
and make suggestions on the optimal parser

JSON은 데이터 교환에 일반적으로 사용되는 개방형 표준 경량 파일 형식입니다.

데이터 객체들을 저장하고 전송하기 위해서 사람이 읽을 수 있는 텍스트를 사용합니다.

따라서 JSON 이벤트 로그 형식을 정의하고 플러그인을 생성하게 된 동기는 JSON 로그를 쉽게 사용할 수 있다는 사실에 있습니다.

XES 형식에 대해 정의된 4가지 디자인 원칙, 즉 (i) 단순성, (ii) 유연성, (iii) 확장성 및 (iv) 표현성을 사용했습니다.

이는 JXES 표준 형식 정의와 관련하여 설계 결정을 내리고 구현방법을 평가하고 최적의 파서에 대해 제안을 하는데 도움이 되었습니다.


For defining the JSON standard format, we have taken into account the XES meta-model shown in Figure 1 which is represented by the basic structure (log, trace and event), Attributes, Nested Attributes, Global Attributes, Event classifiers and Extensions.

There are different types of primitive attributes.

The String and Date attributes are stored as JSON strings.

JSON numbers represent floats and integers.

Boolean values are stored as JSON Boolean.

The List values is represented as an array of JSON Objects.

Lastly, the Container is stored as JSON object.

A log object contains 0 or more trace objects, which are stored in the traces array of JSON objects.

Each trace describes the execution of one specific instance, or case, of the logged process.

Every trace contains an arbitrary number of events objects.

In addition every trace has its own attributes stored in the attrs object.

Below is an example representation of the basic structure

JSON 일반적인 형식을 정의하기 위해, 기본 구조(로그, trace 및 이벤트), 속성, 중첩 속성, 전역 속성, 이벤트 분류자 및 확장으로 표현되는 그림 1에서 보여진 XES 메타 모델을 고려하였습니다.

다양한 유형들의 기본 속성이 있습니다.

String 및 Date 속성은 JSON 문자열로 저장됩니다.

JSON 숫자는 부동 소수점과 정수를 나타냅니다.

부울 값은 JSON 부울로 저장됩니다.

목록 값은 JSON 개체의 배열로 표현됩니다.

마지막으로 Container는 JSON 객체로 저장됩니다.

로그 개체에는 JSON 개체의 trace 배열에 저장되는 0개 이상의 trace 개체가 포함됩니다.

각 trace는 기록된 프로세스의 case 또는 하나의 특정 인스턴스의 실행을 설명합니다.

모든 trace에는 임의의 수의 이벤트 개체가 포함됩니다.

또한 모든 trace에는 attrs 개체에 저장된 고유한 속성이 있습니다.

아래는 기본 구조의 예시 표현입니다.



```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211117_212833_1.png")
```





![output_6_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_6_0.png)




Because the string has the same power as the ID, the ID data type is not supported in JXES.

Below is an example for the representation of every attribute-type.

문자열은 ID와 동일한 권한을 갖기 때문에, ID 데이터 유형은 JXES에서 지원되지 않습니다.

아래는 모든 속성 유형의 표현에 대한 예입니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_111144_1.png")
```





![output_8_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_8_0.png)




To represent nested attributes in JXES the container with two keys with the names value and nested-attrs is reserved.

Which means that every container with any of the keys value or nested-attrs is reserved by JXES and can not be used by the user.

Every other container is allowed.

Below is an example for a nested attribute container.

JXES에서 중첩된 속성을 나타내기 위해 이름 값과 nested-attrs가 있는 두 개의 키가 있는 컨테이너가 자리잡고 있습니다.

즉, 키 값 또는 중첩 속성이 있는 모든 컨테이너는 JXES의해 자리잡고 있고 사용자에 의해 사용될 수 없습니다.

다른 모든 컨테이너는 허용됩니다.

아래는 중첩된 속성 컨테이너의 예입니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_112303_1.png")
```





![output_10_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_10_0.png)




Global attributes are attributes that are understood to be available and properly defined for each element on their respective level throughout the document.

The log object holds two lists of global attributes for the trace level and for the event level.

This means, a global attribute on the event level must be available for every event in every trace.

In JXES, we have defined elements for the trace level and the event level.

They are both stored in under the element global-attrs as nested elements.

Below is an example for a global attribute.

전역 속성은 문서 전체에서 해당 수준의 각 요소에 대해 적절하게 정의되었고 사용 가능하도록 이해되는 속성입니다.

로그 개체에는 trace level 및 이벤트 level에 대한 두 개의 전역 속성 목록이 있습니다.

즉, 이벤트 level의 전역 속성은 모든 trace의 모든 이벤트에 사용될 수 있어야 합니다.

JXES에서는 trace level 및 이벤트 level에 대한 요소들을 정의하였습니다.

둘 다 전역 속성 요소 아래에 중첩 요소로 저장됩니다.

아래는 전역 속성의 예입니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_115000_1.png")
```





![output_12_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_12_0.png)




Event Classifiers assigns to each event an identity, which makes it comparable to other events (via their assigned identity).

The JXES format makes event classification configurable and flexible, by introducing the concept of event classifiers.

The classifier name is stored in the key and the classifier keys are stored as an array of strings.

An example of event classifiers can be found below.

이벤트 분류자는 각 이벤트에 ID를 할당하여 다른 이벤트와 비교할 수 있도록 합니다(할당된 ID를 통해).

JXES 형식은 이벤트 분류기의 개념을 도입하여 이벤트 분류를 설정 가능하고 유연하게 만듭니다.

분류자 이름은 키에 저장되고 분류자 키는 문자열 배열로 저장됩니다.

이벤트 분류기의 예는 아래에서 찾을 수 있습니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_120251_1.png")
```





![output_14_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_14_0.png)




Extensions is a set of attributes on any levels of the XES log hierarchy (log, trace, event, and meta for nested attributes).

Extensions have many possible uses. One important use is to introduce a set of commonly understood attributes which are vital for a specific perspective or dimension of event log analysis (and which may even not have been foreseen at the time of designing the XES standard).

They are stored in an array.

Every object in this array represents an extension.

The name, concept and uri are stored as key value pairs.

See the following example of extension definitions.

확장은 XES 로그 체계의 모든 수준에 있는 속성 집합입니다(중첩 속성의 경우 로그, trace, 이벤트 및 메타).

확장에는 많은 용도가 있습니다. 한 가지 중요한 용도는 이벤트 로그 분석의 특정 관점이나 차원에 필수적인 일반적으로 이해되는 속성 세트를 도입하는 것입니다(XES 표준을 설계할 때 예측하지 못했을 수도 있음).

그것들은 배열에 저장됩니다.

이 배열의 모든 개체는 확장을 나타냅니다.

이름, 개념 및 uri는 키 값 쌍으로 저장됩니다.

확장 정의의 다음 예를 참조하십시오.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_125648_1.png")
```





![output_16_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_16_0.png)




III. IMPLEMENTATION

The basic idea is to enable usage of JSON format of event logs in the ProM tool.

To achieve this we did a market research of the top and best performing Java JSON parsers.

III. 구현

기본 아이디어는 ProM 도구에서 이벤트 로그의 JSON 형식 사용을 활성화하는 것입니다.

이를 달성하기 위해 우리는 최고 성능의 Java JSON 파서에 대한 시장 조사를 수행했습니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_130123_1.png")
```





![output_18_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_18_0.png)




The plugin to import and export the JSON file consists of 4 different parser implementations of import as well as export.

The parsers that have been implemented are Jackson, Jsoninter, GSON and simple JSON.

When the user clicks on ”Import”in ProM tool and chooses a JSON file, the import options related to JXES are displayed where the user can choose from one of the parsers to import as shown in Figure 3.

When the user clicks on ”Export to disk” option, the console to save is displayed with Export parser options as shown in Figure 4.

JSON 파일을 가져오고 내보내는 플러그인은 가져오기 및 내보내기의 4가지 다른 파서 구현으로 구성됩니다.

구현된 파서들은 Jackson, Jsoninter, GSON 및 단순 JSON입니다.

사용자가 ProM 도구에서 "가져오기"를 클릭하고 JSON 파일을 선택하면, 그림 3과 같이 사용자가 가져올 파서 중 하나를 선택할 수 있는 JXES와 관련된 가져오기 옵션이 표시됩니다.

사용자가 "디스크로 내보내기" 옵션을 클릭하면, 그림 4와 같이 파서 내보내기 옵션과 함께 저장할 콘솔이 표시됩니다.




```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_130948_1.png")
```





![output_20_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_20_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_131108_1.png")
```





![output_21_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_21_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_131155_1.png")
```





![output_22_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_22_0.png)




IV. EVALUATION

We used 2 real-life event logs and 2 artificial event logs for our analysis.

Table I highlights some of the key characteristics of the real-life event logs.

The Level D2 log contains all standard attribute extensions like lifecycle, cost, concept and time extensions of an event log.

The Flag X2 log is a Level D2 extended with attributes from non-standard XES extensions and/or attributes without an extension.

IV. 평가

우리는 분석을 위해 2개의 실제 이벤트 로그와 2개의 인공 이벤트 로그를 사용했습니다.

표 I은 실제 이벤트 로그의 몇 가지 주요 특성들을 강조합니다.

레벨 D2 로그에는 이벤트 로그의 수명 주기, 비용, 개념 및 시간 확장과 같은 모든 표준 속성 확장이 포함됩니다.

플래그 X2 로그는 비표준 XES 확장의 속성 및/또는 확장이 없는 속성으로 확장된 레벨 D2입니다.


The evaluation of parsers was done alongside to the existing XES Naive and XES zipped implementations in ProM.

The machine used to run the evaluation is equipped with a 4-core Intel i7 processor and 8 GB of RAM.

파서 평가는 ProM에서 기존 XES Naive 및 XES 압축 구현과 함께 수행되었습니다.

평가를 실행하는 데 사용된 컴퓨터에는 4코어 Intel i7 프로세서와 8GB RAM이 장착되어 있습니다.

The criteria considered for evaluation are (1) speed (2)memory usage and (3) size.

To test the speed and memory of the different parsers, the file is imported/exported thrice and the average of the 3 runs is recorded in the tables.

1) Speed: The best result for JSON parsers is highlighted in green.

The unit of time specified in Table II and Table III is in milliseconds

평가 기준은 (1) 속도 (2) 메모리 사용량 (3) 크기입니다.

다른 파서의 속도와 메모리를 테스트하기 위해서, 파일을 세 번 가져오거나 내보내고 3회 실행의 평균을 테이블에 기록합니다.

1) 속도: JSON 파서에 대한 최상의 결과는 녹색으로 강조 표시됩니다.

표 II 및 표 III에 지정된 시간 단위는 밀리초입니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_132642_1.png")
```





![output_26_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_26_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_132723_1.png")
```





![output_27_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_27_0.png)




2) Memory: The memory consumption for different parsers and the XES implementation is computed using difference of the JAVA Runtime memory methods totalMemory and freeMemory.

It is noticeable that the Jackson JSON parser uses significantly less memory than the other parsers.

And also that Simple JSON uses little less memory when it comes to average sized files.

The unit of memory given in Table IV and Table V is MBs.

2) 메모리: 다른 파서 및 XES 구현에 대한 메모리 소비는 JAVA 런타임 메모리 메서드 totalMemory 및 freeMemory의 차이를 사용하여 계산됩니다.

Jackson JSON 파서는 다른 파서보다 훨씬 적은 메모리를 사용합니다.

또한 Simple JSON은 평균 크기의 파일과 관련하여 메모리를 거의 사용하지 않습니다.

표 IV 및 표 V에 제공된 메모리 단위는 MB(메가바이트)입니다.

It is clear from the results of the export plugin that JXES is up to 4x faster than XES.

One reason is the reduced syntax that gets written when exporting JSON as compared to XML.

내보내기 플러그인의 결과를 보면 JXES가 XES보다 최대 4배 빠릅니다.

한 가지 이유는 XML과 비교하여 JSON을 내보낼 때 작성되는 축소된 구문 때문입니다.


It is also clear that the Jsoninter parser achieves better results than all others in terms of exporting speed.

The speed improvement in Jsoninter can be attributed to its Dynamic Class Shadowing tri-tree feature.

It is noticeable that the Jackson JSON Parser uses significantly less memory than the other parsers.

This performance can be attributed to the incremental parsing/generation feature which reads and writes JSON content as discrete events.

And also that Simple JSON uses little less Memory when it comes to average sized files.

In addition the Jsoninter JSON parsers uses the most memory in all cases.

또한 Jsoninter 파서가 내보내기 속도 면에서 다른 모든 파서보다 더 나은 결과를 달성한다는 것도 분명합니다.

Jsoninter의 속도 향상은 Dynamic Class Shadowing tri-tree 특징의 결과로 볼 수 있습니다.

Jackson JSON 파서는 다른 파서보다 훨씬 적은 메모리를 사용합니다.

이 성능은 JSON 콘텐츠를 개별 이벤트로 읽고 쓰는 증분 구문 분석/생성 특징의 결과로 볼 수 있습니다.

또한 Simple JSON은 평균 크기의 파일과 관련하여 메모리를 거의 사용하지 않습니다.

또한 Jsoninter JSON 파서는 모든 경우에 가장 많은 메모리를 사용합니다.


3) Size: Table VI provides the size in MBs for the files stored.

The size improvement in JXES is obvious because the type is not specified and tag names are not written twice and the markup is not repeated.

We noted that there was no loss of information during the conversion from XES to JXES and vice versa.

The only difference noted was the log version information in the header of XES file.

We observed that the performance of the XES Naive importer is surprisingly good when compared with the JSON format.

This can be attributed to the fact that XES format has the datatype specified in the tag whereas in JSON we need to parse it completely to determine the datatype.

3) 크기: 테이블 VI는 저장된 파일의 크기(MB:메가바이트)를 제공합니다.

JXES의 크기 개선은 유형이 지정되지 않고 태그 이름이 두 번 작성되지 않고 마크업이 반복되지 않기 때문에 명백합니다.

우리는 XES에서 JXES로 또는 그 반대로 변환하는 동안 정보 손실이 없다는 점에 주목했습니다.

기록된 유일한 차이점은 XES 파일의 헤더에 있는 로그 버전 정보였습니다.

XES Naive 임포터의 성능이 JSON 형식과 비교할 때 놀라울 정도로 좋다는 것을 목격했습니다.

이는 XES 형식이 태그에 명시된 데이터 유형을 갖는 반면 JSON에서는 데이터 유형을 결정하기 위해 완전히 구문 분석해야 한다는 사실에 기인할 수 있습니다.

* parser란 compiler의 일부로 컴파일러나 인터프리터에서 원시 프로그램을 읽어 들여 그 문장의 구조를 알아내는 parsing(구문 분석)을 행하는 프로그램


V. ACCESS AND DEMONSTRATION

The JXES import and export plugins have been implemented with 4 different JSON parsers.

The code is available in the SVN repository.

The sample logs for JXES format can be found under the tests/testfiles/ directory.

The tutorial video of the parsers implemented as a ProM plugin is available at https://youtu.be/sZ6UnTfSsFI.

Figure 3 and Figure 4 show the different parser options to import and export JSON event logs respectively.

To run the import plugin, we will require the event log in JSON format shown in Figure 2.

The application can also be run by importing any of the event log in CSV or XES format and then converting it into the JSON format

V. 접근 및 시연

JXES 가져오기 및 내보내기 플러그인은 4개의 다른 JSON 구문 분석기로 구현되었습니다.

코드는 SVN 저장소에서 사용할 수 있습니다.

JXES 형식에 대한 샘플 로그는 tests/testfiles/디렉토리에서 찾을 수 있습니다.

ProM 플러그인으로 구현된 파서의 튜토리얼 비디오는 https://youtu.be/sZ6UnTfSsFI 에서 볼 수 있습니다.

그림 3과 그림 4는 각각 JSON 이벤트 로그를 가져오고 내보내는 다양한 파서 옵션을 보여줍니다.

가져오기 플러그인을 실행하려면, 그림 2와 같은 JSON 형식의 이벤트 로그가 필요합니다.

CSV 또는 XES 형식의 이벤트 로그를 가져온 다음 JSON 형식으로 변환하여 애플리케이션을 실행할 수도 있습니다.


```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_140217_1.png")
```





![output_33_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_33_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/논문/data/20211118_140217_2.png")
```





![output_34_0](./img/thesis/JSONSupportfortheXESEventLogStandard/output_34_0.png)




VI. CONCLUSION

We have introduced JXES, which is a JSON format of event log which adheres to the XES principles.

In this paper, we defined the JSON format of event log as defined by the IEEE XES standard.

After defining the standard, we have provided 4 different implementations of import and export options for JSON event logs with different parsers.

VI. 결론

XES 원칙을 준수하는 JSON 형식의 이벤트 로그인 JXES를 소개하였습니다.

본 논문에서는 IEEE XES 표준에서 정의한 JSON 형식의 이벤트 로그를 정의하였다.

표준을 정의한 후 다양한 파서를 사용하여 JSON 이벤트 로그에 대한 가져오기 및 내보내기 옵션의 4가지 구현을 제공했습니다.

Table VII shows the authors recommendation for parser choice in case of import and export considering speed and memory criteria.

It would be interesting to evaluate more parsers (e.g.,JSONP, fastJSON and ProtoBuf)

We hope that the JXES standard defined by this paper will be helpful and serve as a guideline for generating event logs in JSON format.

We also hope that the JXES standard defined in this paper will be useful for many tools like Disco, Celonis, PM4Py, etc., to enable support for JSON event logs.

We also hope that the plugin implemented in ProM will be useful for many with JSON event logs.


표 VII는 속도 및 메모리 기준을 고려하여 가져오기 및 내보내기의 경우 파서 선택에 대한 작성자 권장 사항을 보여줍니다.

더 많은 파서를 평가하는 것은 흥미로울 것입니다(예: JSONP, fastJSON 및 ProtoBuf)

본 논문에서 정의한 JXES 표준이 JSON 형식의 이벤트 로그를 생성하는 데 도움이 되고 지침이 되기를 바랍니다.

또한 이 문서에서 정의한 JXES 표준이 Disco, Celonis, PM4Py 등과 같은 많은 도구에서 JSON 이벤트 로그를 지원하는 데 유용하기를 바랍니다.

또한 ProM에 구현된 플러그인이 JSON 이벤트 로그를 사용하는 많은 사람들에게 유용하기를 바랍니다.
