---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: DataFrame 데이터 생성하기
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
1. 수치해석 라이브러리인 numpy의 이해 및 사용
2. 데이터 분석 라이브러이인 pandas의 이해 및 사용

#### DataFrame 생성하기
- 일반적으로 분석을 위한 데이터는 다른 데이터 소스(database, 외부 파일)을 통해 dataframe을 생성
- 여기서는 실습을 통해, dummy 데이터를 생성하는 방법을 다룰 예정


```python
import pandas as pd
```

#### dictionary로 부터 생성하기
- dict의 key -> column


```python
data = {'a' : 100, 'b' : 200, 'c' : 300}
pd.DataFrame(data, index=['x', 'y', 'z'])
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>x</th>
      <td>100</td>
      <td>200</td>
      <td>300</td>
    </tr>
    <tr>
      <th>y</th>
      <td>100</td>
      <td>200</td>
      <td>300</td>
    </tr>
    <tr>
      <th>z</th>
      <td>100</td>
      <td>200</td>
      <td>300</td>
    </tr>
  </tbody>
</table>
</div>




```python
data = {'a' : [1, 2, 3], 'b' : [4, 5, 6], 'c' : [10, 11, 12]}
pd.DataFrame(data, index=[0, 1, 2])
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>4</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>5</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>6</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>



#### Series로 부터 생성하기
- 각 Series의 인덱스 -> column


```python
a = pd.Series([100, 200, 300], ['a', 'b', 'c'])
b = pd.Series([101, 201, 301], ['a', 'b', 'c'])
c = pd.Series([110, 210, 310], ['a', 'b', 'c'])

pd.DataFrame([a, b, c], index=[100, 101, 102])
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>100</td>
      <td>200</td>
      <td>300</td>
    </tr>
    <tr>
      <th>101</th>
      <td>101</td>
      <td>201</td>
      <td>301</td>
    </tr>
    <tr>
      <th>102</th>
      <td>110</td>
      <td>210</td>
      <td>310</td>
    </tr>
  </tbody>
</table>
</div>




```python
a = pd.Series([100, 200, 300], ['a', 'b', 'd'])
b = pd.Series([101, 201, 301], ['a', 'b', 'k'])
c = pd.Series([110, 210, 310], ['a', 'b', 'c'])

pd.DataFrame([a, b, c], index=[100, 101, 102])
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
      <th>a</th>
      <th>b</th>
      <th>d</th>
      <th>k</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>100.0</td>
      <td>200.0</td>
      <td>300.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>101</th>
      <td>101.0</td>
      <td>201.0</td>
      <td>NaN</td>
      <td>301.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>102</th>
      <td>110.0</td>
      <td>210.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>310.0</td>
    </tr>
  </tbody>
</table>
</div>


