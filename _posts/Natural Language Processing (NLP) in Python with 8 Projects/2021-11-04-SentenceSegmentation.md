---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Natural Language Processing (NLP) in Python with 8 Projects - Sentence Segmentation
tags: [udemy]  
class: post-template
subclass: 'post tag-udemy'  
author: SeongJae Yu  
sitemap:
lastmod: 2021-11-04
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include udemy.html %}

## Sentence Segmentation

A Doc object’s sentences are available via the Doc.sents property. To view a Doc’s sentences, you can iterate over the Doc.sents, a generator that yields Span objects. You can check whether a Doc has sentence boundaries by calling Doc.has_annotation with the attribute name "SENT_START".

Doc 개체의 문장은 Doc.sents 속성을 통해 사용할 수 있습니다. Doc의 문장을 보려면 Span 개체를 생성하는 생성기인 Doc.sents를 반복할 수 있습니다. 속성 이름이 "SENT_START"인 Doc.has_annotation을 호출하여 문서에 문장 경계가 있는지 확인할 수 있습니다.


```python
# 예제1 
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("This is a sentence. This is another sentence.")
assert doc.has_annotation("SENT_START")
for sent in doc.sents:
    print(sent.text)
```

    This is a sentence.
    This is another sentence.



```python
#예제2
```


```python
s1 = "This is a sentence. This is second sentence. This is last sentence."
s2 = "This is a sentence; This is second sentence; This is last sentence."
```


```python
import spacy
nlp = spacy.load(name='en_core_web_sm')
```


```python
doc1 = nlp(s1)
```

#  한문장 안에 .으로 끝나는 문장을 여러개 출력해준다


```python
for sent in doc1.sents:
  print(sent.text)
```

    This is a sentence.
    This is second sentence.
    This is last sentence.



```python
s3 = "This is a sentence. This is second U.K. sentence. This is last sentence."
```


```python
doc3 = nlp(s3)
for sent in doc3.sents:
  print(sent.text)
```

    This is a sentence.
    This is second U.K. sentence.
    This is last sentence.



```python
doc2 = nlp(s2)
for sent in doc2.sents:
  print(sent.text)
```

    This is a sentence; This is second sentence; This is last sentence.



```python
s2
```




    'This is a sentence; This is second sentence; This is last sentence.'



# 'This is a sentence; This is second sentence; This is last sentence.' 문장을 어떻게 여러개의 문장들로 나눌수 있을까?

# 참고사이트-[https://johnnn.tech/q/using-pos-and-punct-tokens-in-custom-sentence-boundaries-in-spacy/](https://johnnn.tech/q/using-pos-and-punct-tokens-in-custom-sentence-boundaries-in-spacy/)


```python
from spacy.language import Language
```


```python

custom_EOS = ['.', ',', '!', '!',';']
custom_conj = ['then', 'so']
```


```python
@Language.component("set_custom_boundaries")
def set_custom_boundaries(doc):
    for token in doc[:-1]:
        if token.text in custom_EOS:
            doc[token.i + 1].is_sent_start = True
        if token.text in custom_conj:
            doc[token.i].is_sent_start = True
    return doc
```


```python
def set_sentence_breaks(doc):
    for token in doc:
        if token == "SCONJ": #SCONJ: subordinating conjunction
            doc[token.i].is_sent_start = True

```


```python
def main():
    text = "In the add user use case, we need to consider speed and reliability, so use of a relational DB would be better than using SQLite. Though it may take extra effort to convert @Bot"

    nlp = spacy.load("en_core_web_sm")
    nlp.add_pipe("set_custom_boundaries", before="parser")
    doc = nlp(text)
    # for token in doc:
    #     print(token.pos_)

    print("Sentences:", [sent.text for sent in doc.sents])


if __name__ == "__main__":
    main()

```

    Sentences: ['In the add user use case,', 'we need to consider speed and reliability,', 'so use of a relational DB would be better than using SQLite.', 'Though it may take extra effort to convert @Bot']


- 참고 사이트에 나와있는 방식대로 적용하여 ['This is a sentence;', 'This is second sentence;', 'This is last sentence.'] 문장들로 분리했음


```python
def main():
    text = s2
    nlp = spacy.load("en_core_web_sm")
    nlp.add_pipe("set_custom_boundaries", before="parser")
    doc = nlp(text)
    # for token in doc:
    #     print(token.pos_)

    print("Sentences:", [sent.text for sent in doc.sents])


if __name__ == "__main__":
    main()

```

    Sentences: ['This is a sentence;', 'This is second sentence;', 'This is last sentence.']
    

