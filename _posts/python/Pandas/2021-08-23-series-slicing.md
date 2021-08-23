---
layout: post
current: post
cover:  assets/built/images/logo-python.png
navigation: True
title: Series 데이터 변경 - 슬라이싱하기
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
1. pandas Series 이해하기


```python
import numpy as np
import pandas as pd
```

#### **Series 값 변경**
- 추가 및 업데이트: 인덱스를 이용
- 삭제: drop함수 이용



```python
s = pd.Series(np.arange(100, 105), ['a', 'b', 'c', 'd', 'e'])
s
```




    a    100
    b    101
    c    102
    d    103
    e    104
    dtype: int32




```python
s['a'] = 200
s
```




    a    200
    b    101
    c    102
    d    103
    e    104
    dtype: int32




```python
s['k'] = 300
s
```




    a    200
    b    101
    c    102
    d    103
    e    104
    k    300
    dtype: int64




```python
s.drop('k', inplace=True) #inplace=True으로 결과 값 고정시킴
```


```python
s
```




    a    200
    b    101
    c    102
    d    103
    e    104
    dtype: int64




```python
s[['a', 'b']] = [300, 900]
s
```




    a    300
    b    900
    c    102
    d    103
    e    104
    dtype: int64



#### **Slicing**
- 리스트, ndarray와 동일하게 적용


```python
s1 = pd.Series(np.arange(100, 105))
s1
```




    0    100
    1    101
    2    102
    3    103
    4    104
    dtype: int32




```python
s1[1:3]
```




    1    101
    2    102
    dtype: int32




```python
s2 = pd.Series(np.arange(100, 105), ['a', 'c', 'b', 'd', 'e'])
s2
```




    a    100
    c    101
    b    102
    d    103
    e    104
    dtype: int32




```python
s2[1:3]
```




    c    101
    b    102
    dtype: int32




```python
s2['c':'d'] #문자열로 이루어진 경우 마지막 까지 포함시킴
```




    c    101
    b    102
    d    103
    dtype: int32


