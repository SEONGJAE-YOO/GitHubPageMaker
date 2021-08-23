---
layout: post
current: post
cover:  assets/built/images/logo-python.png
navigation: True
title: Series 데이터 Boolean Selection으로 데이터 선택하기
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
1. Series boolean selection 활용하기


```python
import numpy as np
import pandas as pd
```

#### **Boolean selection**
- boolean Series가 []와 함께 사용되면 True 값에 해당하는 값만 새로 반환되는 Series객체에 포함됨
- 다중조건의 경우, &(and), |(or)를 사용하여 연결 가능


```python
s = pd.Series(np.arange(10), np.arange(10)+1) # ( 값,주소 )
s
```




    1     0
    2     1
    3     2
    4     3
    5     4
    6     5
    7     6
    8     7
    9     8
    10    9
    dtype: int32




```python
s > 5
```




    1     False
    2     False
    3     False
    4     False
    5     False
    6     False
    7      True
    8      True
    9      True
    10     True
    dtype: bool




```python
s[s>5]
```




    7     6
    8     7
    9     8
    10    9
    dtype: int32




```python
s[s % 2 == 0]
```




    1    0
    3    2
    5    4
    7    6
    9    8
    dtype: int32




```python
s
```




    1     0
    2     1
    3     2
    4     3
    5     4
    6     5
    7     6
    8     7
    9     8
    10    9
    dtype: int32




```python
s.index > 5
```




    array([False, False, False, False, False,  True,  True,  True,  True,
            True])




```python
s[s.index > 5]
```




    6     5
    7     6
    8     7
    9     8
    10    9
    dtype: int32




```python
(s > 5) & (s < 8)
```




    1     False
    2     False
    3     False
    4     False
    5     False
    6     False
    7      True
    8      True
    9     False
    10    False
    dtype: bool




```python
s[(s > 5) & (s < 8)]
```




    7    6
    8    7
    dtype: int32




```python
(s >= 7).sum()
```




    3




```python
(s[s>=7]).sum() # 7+8+9 = 24
```




    24


