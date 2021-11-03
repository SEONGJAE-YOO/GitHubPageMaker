---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Natural Language Processing (NLP) in Python with 8 Projects - Named Entity Recognition
tags: [udemy]  
class: post-template
subclass: 'post tag-udemy'  
author: SeongJae Yu  
sitemap:  
lastmod: 2021-11-03
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include udemy.html %}

## Named Entity Recognition

spaCy features an extremely fast statistical entity recognition system, that assigns labels to contiguous spans of tokens. The default trained pipelines can identify a variety of named and numeric entities, including companies, locations, organizations and products. You can add arbitrary classes to the entity recognition system, and update the model with new examples.

spaCy는 연속적인 토큰 범위에 레이블을 할당하는 매우 빠른 통계적 개체 인식 시스템을 특징으로 합니다. 기본 훈련된 파이프라인은 회사, 위치, 조직 및 제품을 포함하여 다양한 이름 및 숫자 엔터티를 식별할 수 있습니다. 엔티티 인식 시스템에 임의의 클래스를 추가하고 새 예제로 모델을 업데이트할 수 있습니다.

# Named Entity Recognition NER using spaCy | NLP | Part 4

# 참고 사이트 -[https://towardsdatascience.com/named-entity-recognition-ner-using-spacy-nlp-part-4-28da2ece57c6](https://towardsdatascience.com/named-entity-recognition-ner-using-spacy-nlp-part-4-28da2ece57c6)


Named Entity Recognition is the most important, or I would say, the starting step in Information Retrieval. Information Retrieval is the technique to extract important and useful information from unstructured raw text documents. Named Entity Recognition NER works by locating and identifying the named entities present in unstructured text into the standard categories such as person names, locations, organizations, time expressions, quantities, monetary values, percentage, codes etc. Spacy comes with an extremely fast statistical entity recognition system that assigns labels to contiguous spans of tokens.

Named Entity Recognition은 정보 검색의 시작 단계 중 가장 중요합니다. 정보 검색은 구조화되지 않은 원시 텍스트 문서에서 중요하고 유용한 정보를 추출하는 기술입니다. Named Entity Recognition NER는 사람 이름, 위치, 조직, 시간 표현, 수량, 금전적 가치, 백분율, 코드 등과 같은 표준 범주로 구조화되지 않은 텍스트에 있는 명명된 엔터티를 찾아 식별하는 방식으로 작동합니다. Spacy는 연속적인 토큰 범위에 레이블을 할당하는 매우 빠른 통계 개체와 함께 제공됩니다.

Spacy provides an option to add arbitrary classes to entity recognition systems and update the model to even include the new examples apart from already defined entities within the model.
Spacy has the ‘ner’ pipeline component that identifies token spans fitting a predetermined set of named entities. These are available as the ‘ents’ property of a Doc object.

Spacy는 엔티티 인식 시스템에 임의의 클래스를 추가하고 모델 내에서 이미 정의된 엔티티와 별개로 새 예제를 포함하도록 모델을 업데이트하는 옵션을 제공합니다.
Spacy에는 미리 결정된 명명된 엔터티 집합에 맞는 토큰 범위를 식별하는 'ner' 파이프라인 구성 요소가 있습니다. 이들은 Doc 객체의 'ents' 속성으로 사용할 수 있습니다.


```python
s1 = "Apple is looking at buying U.K. startup for $1 billion"
s2 = "San Francisco considers banning sidewalk delivery robots"
s3 = "facebook is hiring a new vice president in U.S."
```


```python
import spacy
nlp = spacy.load(name='en_core_web_sm')
```


```python
doc1 = nlp(s1)
doc1.ents
```




    (Apple, U.K., $1 billion)



# doc.ents property가 무엇일까?

# 참고사이트 - [https://www.tutorialspoint.com/spacy/spacy_doc_ents.htm](https://www.tutorialspoint.com/spacy/spacy_doc_ents.htm)

This doc property is used for the named entities in the document. If the entity recognizer has been applied, this property will return a tuple of named entity span objects.

이 doc 속성은 문서의 명명된 엔터티에 사용됩니다. 엔터티 인식기가 적용된 경우 이 속성은 명명된 엔터티 범위 개체의 튜플을 반환합니다.




```python
doc1 = nlp(s1)
for ent in doc1.ents:
  print(ent.text, ent.label_, str(spacy.explain(ent.label_)))
```

    Apple ORG Companies, agencies, institutions, etc.
    U.K. GPE Countries, cities, states
    $1 billion MONEY Monetary values, including unit



```python
doc2 = nlp(s2)
for ent in doc2.ents:
  print(ent.text, ent.label_, str(spacy.explain(ent.label_)))
```

    San Francisco GPE Countries, cities, states



```python
print(s3)
doc3 = nlp(s3)  # facebook은 name entity 가 아니다
for ent in doc3.ents:
  print(ent.text, ent.label_, str(spacy.explain(ent.label_)))
```

    facebook is hiring a new vice president in U.S.
    U.S. GPE Countries, cities, states


# facebook을 name entity로 추가하고 싶을때 방법


```python
ORG = doc3.vocab.strings['ORG']
```


```python
from spacy.tokens import Span
new_ent = Span(doc3, 0, 1, label = ORG)
```

# span 클래스란 무엇일까?

# 참고사이트- [https://spacy.io/api/span](https://spacy.io/api/span)

A slice from a Doc object.




```python
doc3.ents = list(doc3.ents) + [new_ent]
```


```python
doc3.ents
```




    (facebook, U.S.)




```python
print(s3)
for ent in doc3.ents:
  print(ent.text, ent.label_, str(spacy.explain(ent.label_)))
```

    facebook is hiring a new vice president in U.S.
    facebook ORG Companies, agencies, institutions, etc.
    U.S. GPE Countries, cities, states



```python
from spacy import displacy
```


```python
displacy.render(docs = doc1, style= 'ent', jupyter=True)
```


<span class="tex2jax_ignore"><div class="entities" style="line-height: 2.5; direction: ltr">
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
Apple
<span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
is looking at buying
<mark class="entity" style="background: #feca74; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
U.K.
<span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">GPE</span>
</mark>
startup for
<mark class="entity" style="background: #e4e7d2; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
$1 billion
<span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">MONEY</span>
</mark>
</div></span>



```python
displacy.render(docs = doc1, style= 'ent',options={'ents': ['ORG']}, jupyter=True)
```


<span class="tex2jax_ignore"><div class="entities" style="line-height: 2.5; direction: ltr">
<mark class="entity" style="background: #7aecec; padding: 0.45em 0.6em; margin: 0 0.25em; line-height: 1; border-radius: 0.35em;">
Apple
<span style="font-size: 0.8em; font-weight: bold; line-height: 1; border-radius: 0.35em; vertical-align: middle; margin-left: 0.5rem">ORG</span>
</mark>
is looking at buying U.K. startup for $1 billion</div></span>

