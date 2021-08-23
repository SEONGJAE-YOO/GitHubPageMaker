---
layout: post
current: post
cover:  assets/built/images/logo-python.png
navigation: True
title: Series 데이터 생성하기(index, value 활용)
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
1. pandas Series 데이터 생성하기


```python
import numpy as np
import pandas as pd
```

#### Series
- pandas의 기본 객체 중 하나
- numpy의 ndarray를 기반으로 인덱싱을 기능을 추가하여 1차원 배열을 나타냄
- index를 지정하지 않을 시, 기본적으로 ndarray와 같이 0-based 인덱스 생성, 지정할 경우 명시적으로 지정된 index를 사용
- 같은 타입의 0개 이상의 데이터를 가질 수 있음

* data로만 생성하기
- index는 기본적으로 0부터 자동적으로 생성


```python
s1 = pd.Series([1, 2, 3])
s1
```




    0    1
    1    2
    2    3
    dtype: int64




```python
s2 = pd.Series(['a', 'b', 'c'])
s2
```




    0    a
    1    b
    2    c
    dtype: object




```python
s3 = pd.Series(np.arange(200))
s3
```




    0        0
    1        1
    2        2
    3        3
    4        4
          ... 
    195    195
    196    196
    197    197
    198    198
    199    199
    Length: 200, dtype: int32



* data, index함께 명시하기


```python
s4 = pd.Series([1, 2, 3], [100, 200, 300])
s4
```




    100    1
    200    2
    300    3
    dtype: int64




```python
s5 = pd.Series([1, 2, 3], ['a', 'm', 'k'])
s5
```




    a    1
    m    2
    k    3
    dtype: int64



* data, index, data type 함께 명시하기


```python
s6 = pd.Series(np.arange(5), np.arange(100, 105), dtype=np.int16)
s6
```




    100    0
    101    1
    102    2
    103    3
    104    4
    dtype: int16



#### 인덱스 활용하기


```python
s6.index
```




    Int64Index([100, 101, 102, 103, 104], dtype='int64')




```python
s6.values
```




    array([0, 1, 2, 3, 4], dtype=int16)



1. 인덱스를 통한 데이터 접근


```python
s6[104]
```




    4



2. 인덱스를 통한 데이터 업데이트


```python
s6[104] = 70
s6
```




    100     0
    101     1
    102     2
    103     3
    104    70
    dtype: int16




```python
s6[105] = 90
s6[200] = 80 # 인덱스를 통해 값을 넣을 수도 있다.
s6
```




    100     0
    101     1
    102     2
    103     3
    104    70
    105    90
    200    80
    dtype: int64



3. 인덱스 재사용하기


```python
s7 = pd.Series(np.arange(7), s6.index) #s6 index 그대로 사용한다
s7
```




    100    0
    101    1
    102    2
    103    3
    104    4
    105    5
    200    6
    dtype: int32


