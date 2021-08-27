---
layout: post
current: post
cover:  assets/built/images/logo-python.png
navigation: True
title: pivot, pivot_table 함수의 이해 및 활용하기
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
1. pivot, pivot_table 함수의 이해


```python
import numpy as np
import pandas as pd
```


```python
df = pd.DataFrame({
    '지역': ['서울', '서울', '경기', '경기', '부산', '서울', '서울', '부산', '경기', '경기', '경기'],
    '요일': ['월요일', '수요일', '월요일', '화요일', '월요일', '목요일', '금요일', '화요일', '수요일', '목요일', '금요일'],
    '강수량': [80, 1000, 200, 200, 100, 50, 100, 200, 100, 50, 100],
    '강수확률': [70, 90, 10, 20, 30, 50, 90, 20, 80, 50, 10]
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
      <td>80</td>
      <td>70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>서울</td>
      <td>수요일</td>
      <td>1000</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>경기</td>
      <td>월요일</td>
      <td>200</td>
      <td>10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>경기</td>
      <td>화요일</td>
      <td>200</td>
      <td>20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>부산</td>
      <td>월요일</td>
      <td>100</td>
      <td>30</td>
    </tr>
    <tr>
      <th>5</th>
      <td>서울</td>
      <td>목요일</td>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>6</th>
      <td>서울</td>
      <td>금요일</td>
      <td>100</td>
      <td>90</td>
    </tr>
    <tr>
      <th>7</th>
      <td>부산</td>
      <td>화요일</td>
      <td>200</td>
      <td>20</td>
    </tr>
    <tr>
      <th>8</th>
      <td>경기</td>
      <td>수요일</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>9</th>
      <td>경기</td>
      <td>목요일</td>
      <td>50</td>
      <td>50</td>
    </tr>
    <tr>
      <th>10</th>
      <td>경기</td>
      <td>금요일</td>
      <td>100</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>



#### pivot
- dataframe의 형태를 변경
- 인덱스, 컬럼, 데이터로 사용할 컬럼을 명시


```python
df.pivot('지역', '요일') #index, column
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
      <td>80.0</td>
      <td>NaN</td>
      <td>90.0</td>
      <td>50.0</td>
      <td>90.0</td>
      <td>70.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.pivot('요일', '지역')
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
      <td>80.0</td>
      <td>10.0</td>
      <td>30.0</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>200.0</td>
      <td>200.0</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>20.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.pivot('요일','지역','강수량')
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>금요일</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>목요일</th>
      <td>50.0</td>
      <td>NaN</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>수요일</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>월요일</th>
      <td>200.0</td>
      <td>100.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>200.0</td>
      <td>200.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



#### pivot_table
- 기능적으로 pivot과 동일
- pivot과의 차이점
  - 중복되는 모호한 값이 있을 경우, aggregation 함수 사용하여 값을 채움
  - 기능은 groupby랑 거의 똑같고 , 연산을 하는것이 pivot_table이다.


```python
pd.pivot_table(df, index='요일', columns='지역', aggfunc=np.mean)
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
      <td>80.0</td>
      <td>10.0</td>
      <td>30.0</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>200.0</td>
      <td>200.0</td>
      <td>NaN</td>
      <td>20.0</td>
      <td>20.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.pivot_table(df, index='요일', columns='지역', aggfunc=np.mean, fill_value=0)
#fill_value=0으로 NaN값을 0으로 대체
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
      <td>100</td>
      <td>0</td>
      <td>100</td>
      <td>10</td>
      <td>0</td>
      <td>90</td>
    </tr>
    <tr>
      <th>목요일</th>
      <td>50</td>
      <td>0</td>
      <td>50</td>
      <td>50</td>
      <td>0</td>
      <td>50</td>
    </tr>
    <tr>
      <th>수요일</th>
      <td>100</td>
      <td>0</td>
      <td>1000</td>
      <td>80</td>
      <td>0</td>
      <td>90</td>
    </tr>
    <tr>
      <th>월요일</th>
      <td>200</td>
      <td>100</td>
      <td>80</td>
      <td>10</td>
      <td>30</td>
      <td>70</td>
    </tr>
    <tr>
      <th>화요일</th>
      <td>200</td>
      <td>200</td>
      <td>0</td>
      <td>20</td>
      <td>20</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>


