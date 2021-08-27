---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: stack, unstack 함수의 이해 및 활용하기
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-08-23
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}

## 학습목표
1. stack, unstack 함수 이해하기


```python
import numpy as np
import pandas as pd
```


```python
df = pd.DataFrame({
    '지역': ['서울', '서울', '서울', '경기', '경기', '부산', '서울', '서울', '부산', '경기', '경기', '경기'],
    '요일': ['월요일', '화요일', '수요일', '월요일', '화요일', '월요일', '목요일', '금요일', '화요일', '수요일', '목요일', '금요일'],
    '강수량': [100, 80, 1000, 200, 200, 100, 50, 100, 200, 100, 50, 100],
    '강수확률': [80, 70, 90, 10, 20, 30, 50, 90, 20, 80, 50, 10]
                  })

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>지역</th>
      <th>요일</th>
      <th>강수량</th>
      <th>강수확률</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서울</td>
      <td>월요일</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>서울</td>
      <td>화요일</td>
      <td>80</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2</th>
      <td>서울</td>
      <td>수요일</td>
      <td>1000</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>경기</td>
      <td>월요일</td>
      <td>200</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>경기</td>
      <td>화요일</td>
      <td>200</td>
      <td>20</td>
    </tr>
    <tr>
      <th>5</th>
      <td>부산</td>
      <td>월요일</td>
      <td>100</td>
      <td>30</td>
    </tr>
    <tr>
      <th>6</th>
      <td>서울</td>
      <td>목요일</td>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>7</th>
      <td>서울</td>
      <td>금요일</td>
      <td>100</td>
      <td>90</td>
    </tr>
    <tr>
      <th>8</th>
      <td>부산</td>
      <td>화요일</td>
      <td>200</td>
      <td>20</td>
    </tr>
    <tr>
      <th>9</th>
      <td>경기</td>
      <td>수요일</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>10</th>
      <td>경기</td>
      <td>목요일</td>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>11</th>
      <td>경기</td>
      <td>금요일</td>
      <td>100</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>



####  stack & unstack
- stack : 컬럼 레벨에서 인덱스 레벨로 dataframe 변경
- 즉, 데이터를 쌓아올리는 개념으로 이해하면 쉬움
- unstack : 인덱스 레벨에서 컬럼 레벨로 dataframe 변경
- stack의 반대 operation

- 둘은 역의 관계에 있음


```python
new_df = df.set_index(['지역', '요일'])
new_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>강수량</th>
      <th>강수확률</th>
    </tr>
    <tr>
      <th>지역</th>
      <th>요일</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">서울</th>
      <th>월요일</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>80</td>
      <td>70</td>
    </tr>
    <tr>
      <th>수요일</th>
      <td>1000</td>
      <td>90</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">경기</th>
      <th>월요일</th>
      <td>200</td>
      <td>10</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>200</td>
      <td>20</td>
    </tr>
    <tr>
      <th>부산</th>
      <th>월요일</th>
      <td>100</td>
      <td>30</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">서울</th>
      <th>목요일</th>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>금요일</th>
      <td>100</td>
      <td>90</td>
    </tr>
    <tr>
      <th>부산</th>
      <th>화요일</th>
      <td>200</td>
      <td>20</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">경기</th>
      <th>수요일</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>목요일</th>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>금요일</th>
      <td>100</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df.index
```




    MultiIndex([('서울', '월요일'),
                ('서울', '화요일'),
                ('서울', '수요일'),
                ('경기', '월요일'),
                ('경기', '화요일'),
                ('부산', '월요일'),
                ('서울', '목요일'),
                ('서울', '금요일'),
                ('부산', '화요일'),
                ('경기', '수요일'),
                ('경기', '목요일'),
                ('경기', '금요일')],
               names=['지역', '요일'])




```python
# 첫번째 레벨의 인덱스를 컬럼으로 이동
new_df.unstack(0) 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">강수량</th>
      <th colspan="3" halign="left">강수확률</th>
    </tr>
    <tr>
      <th>지역</th>
      <th>경기</th>
      <th>부산</th>
      <th>서울</th>
      <th>경기</th>
      <th>부산</th>
      <th>서울</th>
    </tr>
    <tr>
      <th>요일</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>금요일</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>100.0</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>목요일</th>
      <td>50.0</td>
      <td>NaN</td>
      <td>50.0</td>
      <td>50.0</td>
      <td>NaN</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>수요일</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>1000.0</td>
      <td>80.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>월요일</th>
      <td>200.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>10.0</td>
      <td>30.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>200.0</td>
      <td>200.0</td>
      <td>80.0</td>
      <td>20.0</td>
      <td>20.0</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 두번째 레벨의 인덱스를 컬럼으로 이동
new_df.unstack(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="5" halign="left">강수량</th>
      <th colspan="5" halign="left">강수확률</th>
    </tr>
    <tr>
      <th>요일</th>
      <th>금요일</th>
      <th>목요일</th>
      <th>수요일</th>
      <th>월요일</th>
      <th>화요일</th>
      <th>금요일</th>
      <th>목요일</th>
      <th>수요일</th>
      <th>월요일</th>
      <th>화요일</th>
    </tr>
    <tr>
      <th>지역</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>경기</th>
      <td>100.0</td>
      <td>50.0</td>
      <td>100.0</td>
      <td>200.0</td>
      <td>200.0</td>
      <td>10.0</td>
      <td>50.0</td>
      <td>80.0</td>
      <td>10.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>100.0</td>
      <td>200.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>100.0</td>
      <td>50.0</td>
      <td>1000.0</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>90.0</td>
      <td>50.0</td>
      <td>90.0</td>
      <td>80.0</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
from IPython.display import Image
Image("stack.png")
```





![output_8_0](./img/stack/output_8_0.png)





```python
# 첫번째 레벨의 컬럼을 인덱스로 이동
new_df.unstack(0).stack(0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>지역</th>
      <th>경기</th>
      <th>부산</th>
      <th>서울</th>
    </tr>
    <tr>
      <th>요일</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">금요일</th>
      <th>강수량</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>강수확률</th>
      <td>10.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">목요일</th>
      <th>강수량</th>
      <td>50.0</td>
      <td>NaN</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>강수확률</th>
      <td>50.0</td>
      <td>NaN</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">수요일</th>
      <th>강수량</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>강수확률</th>
      <td>80.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">월요일</th>
      <th>강수량</th>
      <td>200.0</td>
      <td>100.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>강수확률</th>
      <td>10.0</td>
      <td>30.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">화요일</th>
      <th>강수량</th>
      <td>200.0</td>
      <td>200.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th>강수확률</th>
      <td>20.0</td>
      <td>20.0</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df.unstack(0).stack(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>강수량</th>
      <th>강수확률</th>
    </tr>
    <tr>
      <th>요일</th>
      <th>지역</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">금요일</th>
      <th>경기</th>
      <td>100.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>100.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">목요일</th>
      <th>경기</th>
      <td>50.0</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>50.0</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">수요일</th>
      <th>경기</th>
      <td>100.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>1000.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">월요일</th>
      <th>경기</th>
      <td>200.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>100.0</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>100.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">화요일</th>
      <th>경기</th>
      <td>200.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>부산</th>
      <td>200.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>서울</th>
      <td>80.0</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df.stack
```




    <bound method DataFrame.stack of          강수량  강수확률
    지역 요일             
    서울 월요일   100    80
       화요일    80    70
       수요일  1000    90
    경기 월요일   200    10
       화요일   200    20
    부산 월요일   100    30
    서울 목요일    50    50
       금요일   100    90
    부산 화요일   200    20
    경기 수요일   100    80
       목요일    50    50
       금요일   100    10>


