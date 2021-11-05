---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Natural Language Processing (NLP) in Python with 8 Projects - TfidfVectorizer 
tags: [udemy]  
class: post-template
subclass: 'post tag-udemy'  
author: SeongJae Yu  
sitemap:
lastmod: 2021-11-05
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include udemy.html %}

# TfidfVectorizer은 무엇일까?

# 참고사이트 - [https://chan-lab.tistory.com/27](https://chan-lab.tistory.com/27)


```python
from sklearn.feature_extraction.text import TfidfVectorizer 

text = ['I go to my home my home is very large', 'I went out my home I go to the market', 'I bought a yellow lemon I go back to home'] 

tfidf_vectorizer = TfidfVectorizer()

```


```python
tfidf_vectorizer.fit(text) # 벡터라이저가 단어들을 학습합니다. 
tfidf_vectorizer.vocabulary_ # 벡터라이저가 학습한 단어사전을 출력합니다. 
sorted(tfidf_vectorizer.vocabulary_.items()) # 단어사전을 정렬합니다.

```




    [('back', 0),
     ('bought', 1),
     ('go', 2),
     ('home', 3),
     ('is', 4),
     ('large', 5),
     ('lemon', 6),
     ('market', 7),
     ('my', 8),
     ('out', 9),
     ('the', 10),
     ('to', 11),
     ('very', 12),
     ('went', 13),
     ('yellow', 14)]



min-df는 DF(document-frequency)의 최소 빈도값을 설정해주는 파라미터입니다.


DF는 특정 단어가 나타나는 '문서의 수'를 의미합니다.


'home'이라는 단어가 포함된 문서의 수는 3개이기 때문에 DF = 3 입니다.



```python
min_df_vectorizer = TfidfVectorizer(min_df = 2) 
min_df_vectorizer.fit(text) 
sorted(min_df_vectorizer.vocabulary_.items())

```




    [('go', 0), ('home', 1), ('my', 2), ('to', 3)]



TfidfVectorizer 객체를 생성할 때, 위와 같이 min-df 파라미터를 설정하였습니다.
최소 DF가 2로 설정되었으니, 1인 것들은 모두 탈락하게 됩니다.
(PPT 설명화면에서 회색박스 글자들은 모두 DF가 1인 것들입니다.)


