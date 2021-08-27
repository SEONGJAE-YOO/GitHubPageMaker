---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Series 데이터 연산하기
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap :
changefreq : daily
priority : 1.0
---
{% include python-table-of-contents.html %}


## 학습목표
1. Series 데이터 연산하기


```python
import numpy as np
import pandas as pd
```

#### index를 기준으로 연산


```python
s1 = pd.Series([1, 2, 3, 4], ['a', 'b', 'c', 'd'])
s2 = pd.Series([6, 3, 2, 1], ['d', 'c', 'b', 'a'])

s1
```




    a    1
    b    2
    c    3
    d    4
    dtype: int64




```python
s2
```




    d    6
    c    3
    b    2
    a    1
    dtype: int64




```python
s1 + s2
```




    a     2
    b     4
    c     6
    d    10
    dtype: int64



#### **산술연산**
- Series의 경우에도 스칼라와의 연산은 각 원소별로 스칼라와의 연산이 적용
- Series와의 연산은 각 인덱스에 맞는 값끼리 연산이 적용
    - 이때, 인덱스의 pair가 맞지 않으면, 결과는 NaN


```python
s1 ** 2
```




    a     1
    b     4
    c     9
    d    16
    dtype: int64




```python
s1 ** s2
```




    a       1
    b       4
    c      27
    d    4096
    dtype: int64




```python
4 ** 6
```




    4096



#### **index pair가 맞지 않는 경우**
- 해당 index에 대해선 NaN 값 생성


```python
s1['k'] = 7
s2['e'] = 9
```


```python
s1
```




    a    1
    b    2
    c    3
    d    4
    k    7
    dtype: int64




```python
s2
```




    d    6
    c    3
    b    2
    a    1
    e    9
    dtype: int64




```python
s1 + s2
```




    a     2.0
    b     4.0
    c     6.0
    d    10.0
    e     NaN
    k     NaN
    dtype: float64


