---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Tokenization Basics
tags: [Udemy]  
class: post-template
subclass: 'post tag-Udemy'  
author: SeongJae Yu  
sitemap:
lastmod: 2021-10-29
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---

## 1) Tokenization Basics


```python
s1 = 'Apple is looking at buying U.K. startup for $1 billion !'
s2 = 'Hello all, We are here to help you! email support@udemy.com or visit us at http://www.udemy.com!'
s3 = '10km cab ride almost costs $20 in NYC'
s4 = "Let's watch a movie together."
```

# spacy

spaCy는 파이썬의 자연어처리를 위한 오픈 소스 기반 라이브러리다.

spaCy에서 지원하는 기능들이다. Tokenization과 품사 태깅, 개체 인식 등 기본적인 전처리 기능을 지원하고 있다.
Linguistic Features
Part-of-speech tagging
spacy에서 문장을 tokenize한 뒤, token객체의 유용한 메서드들을 사용할 수 있다.

import spacy

(pip install spacy로 설치)

nlp = spacy.load('en_core_web_sm')

doc = nlp(u'Apple is looking at buying U.K. startup for $1 billion')

for token in doc:

print(token.text, token.lemma_, token.pos_, token.tag_, token.dep_, token.shape_, token.is_alpha, token.is_stop)

- 각 메서드들의 기능은 다음을 의미한다.

Text: The original word text.

Lemma: The base form of the word.

POS: The simple part-of-speech tag.

Tag: The detailed part-of-speech tag.

Dep: Syntactic dependency, i.e. the relation between tokens.

Shape: The word shape — capitalization, punctuation, digits.

is alpha: Is the token an alpha character?

is stop: Is the token part of a stop list, i.e. the most common words of the language?


```python
# !pip install spacy
```


```python
# #https://spacy.io/models  참고
# ! python -m spacy download en_core_web_sm
# ! python -m spacy download en_core_web_md
```


```python
import spacy

nlp = spacy.load(name = 'en_core_web_sm')
```

# en_core_web_sm

English pipeline optimized for CPU. Components: tok2vec, tagger, parser, senter, ner, attribute_ruler, lemmatizer.

SIZE
12 MB

# en_core_web_md

English pipeline optimized for CPU. Components: tok2vec, tagger, parser, senter, ner, attribute_ruler, lemmatizer.

SIZE
43 MB





```python
nlp_1 = spacy.load(name = 'en_core_web_md')
```


```python
import en_core_web_md
```


```python
nlp_1 = en_core_web_md.load()
```


```python
doc1 = nlp(s1)
```


```python
print(s1)
for token in doc1:
  print(token)
```

    Apple is looking at buying U.K. startup for $1 billion !
    Apple
    is
    looking
    at
    buying
    U.K.
    startup
    for
    $
    1
    billion
    !



```python
doc2 = nlp(s2)
print(s2)
for token in doc2:
  print(token)
```

    Hello all, We are here to help you! email support@udemy.com or visit us at http://www.udemy.com!
    Hello
    all
    ,
    We
    are
    here
    to
    help
    you
    !
    email
    support@udemy.com
    or
    visit
    us
    at
    http://www.udemy.com
    !



```python
doc3 = nlp(s3)
print(s3)
for token in doc3:
  print(token)
```

    10km cab ride almost costs $20 in NYC
    10
    km
    cab
    ride
    almost
    costs
    $
    20
    in
    NYC



```python
doc4 = nlp(s4)
print(s4)
for token in doc4:
  print(token)
```

    Let's watch a movie together.
    Let
    's
    watch
    a
    movie
    together
    .


# https://spacy.io/api/tokenizer#_title 참고
# Tokenizer

간단하게 말하면 Tokenization이란 Text를 여러개의 Token으로 나누는 것을 말합니다.

보통 공백, 구두점, 특수문자 등으로 이를 나누는데요. 그 방법에 따라 다양한 Tokenizer가 있습니다.





```python
type(doc4) #A Doc is a sequence of Token
```




    spacy.tokens.doc.Doc




```python
len(doc4) # token 7개로 나누었음
```




    7




```python
doc4[0] #token 첫번째 반환
```




    Let




```python
doc4[2:5]
```




    watch a movie




```python
doc4[-1]
```




    .




```python
doc4[0] ='New' #오류 처리
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-18-c77d23fee774> in <module>
    ----> 1 doc4[0] ='New'
    

    TypeError: 'spacy.tokens.doc.Doc' object does not support item assignment

