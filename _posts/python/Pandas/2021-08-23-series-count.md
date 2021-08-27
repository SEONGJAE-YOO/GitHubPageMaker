---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Series 데이터 심플 분석(개수, 빈도 등 계산하기)
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
1. Series 함수 활용하여 데이터 분석하기


```python
import numpy as np
import pandas as pd
```

#### **Series size, shape, unique, count, value_counts 함수**
- size : 개수 반환
- shape : 튜플형태로 shape반환
- unique: 유일한 값만 ndarray로 반환
- count : NaN을 제외한 개수를 반환
- mean: NaN을 제외한 평균
- value_counts: NaN을 제외하고 각 값들의 빈도를 반환


```python
s = pd.Series([1, 1, 2, 1, 2, 2, 2, 1, 1, 3, 3, 4, 5, 5, 7, np.NaN])
s
```




    0     1.0
    1     1.0
    2     2.0
    3     1.0
    4     2.0
    5     2.0
    6     2.0
    7     1.0
    8     1.0
    9     3.0
    10    3.0
    11    4.0
    12    5.0
    13    5.0
    14    7.0
    15    NaN
    dtype: float64




```python
len(s)
```




    16




```python
s.size
```




    16




```python
s.shape #1차원 이다.
```




    (16,)




```python
s.unique() #중복된 값 제거!
```




    array([ 1.,  2.,  3.,  4.,  5.,  7., nan])




```python
s.count() #NaN를 뺀 count 값 
```




    15




```python
a = np.array([2, 2, 2, 2, np.NaN])
a.mean()

b = pd.Series(a)
b.mean()
```




    2.0




```python
s.mean()
```




    2.6666666666666665




```python
s
```




    0     1.0
    1     1.0
    2     2.0
    3     1.0
    4     2.0
    5     2.0
    6     2.0
    7     1.0
    8     1.0
    9     3.0
    10    3.0
    11    4.0
    12    5.0
    13    5.0
    14    7.0
    15    NaN
    dtype: float64




```python
s.value_counts()
```




    1.0    5
    2.0    4
    3.0    2
    5.0    2
    7.0    1
    4.0    1
    dtype: int64



index를 활용하여 멀티플한 값에 접근


```python
s[[5, 7, 8, 10]].value_counts()
```




    1.0    2
    3.0    1
    2.0    1
    dtype: int64



#### **head, tail 함수**
- head : 상위 n개 출력 기본 5개
- tail : 하위 n개 출력 기본 5개


```python
s.head(n=7)
```




    0    1.0
    1    1.0
    2    2.0
    3    1.0
    4    2.0
    5    2.0
    6    2.0
    dtype: float64




```python
s.tail()
```




    11    4.0
    12    5.0
    13    5.0
    14    7.0
    15    NaN
    dtype: float64




```python
s
```




    0     1.0
    1     1.0
    2     2.0
    3     1.0
    4     2.0
    5     2.0
    6     2.0
    7     1.0
    8     1.0
    9     3.0
    10    3.0
    11    4.0
    12    5.0
    13    5.0
    14    7.0
    15    NaN
    dtype: float64


