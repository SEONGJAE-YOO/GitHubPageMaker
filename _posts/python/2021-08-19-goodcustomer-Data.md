---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 우수고객 선별하기(가장 소비를 많이 한 고객),고객 코호트 분석
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap :
changefreq : daily
priority : 1.0
---
{% include python-table-of-contents.html %}

### 학습목표
1. 소비 우수고객 찾기
2. 고객 retention


```python
from datetime import datetime
import numpy as np
import pandas as pd
import seaborn as sns
from matplotlib import pyplot as plt

%matplotlib inline
```

- seaborn은 matplotlib 처럼 그래프를 그리는 기능이다(matplotlib으로 그래프 그리는 꿀팁이 궁금하다면?). matplotlip으로도 대부분의 시각화는 가능하지만 아래와 같은 이유들로 seaborn을 더 선호하는 추세이다.

1. seaborn에서만 제공되는 통계 기반 plot
2. 특별하게 꾸미지 않아도 깔끔하게 구현되는 기본 color
3. 더 아름답게 그래프 구현이 가능한 palette 기능
4. pandas 데이터프레임과 높은 호환성
   : hue 옵션으로 bar 구분이 가능하며, xtick, ytick, xlabel, ylabel, legend 등이 추가적인 코딩 작업없이 자동으로 세팅된다.


```python
dtypes = {
    'UnitPrice': np.float32,
    'CustomerID': np.int32,
    'Quantity': np.int32
}
retail = pd.read_csv('./OnlineRetailClean.csv', dtype=dtypes)
retail['InvoiceDate'] = pd.to_datetime(retail['InvoiceDate'], infer_datetime_format=True) #      infer_datetime_format=True 날짜시간 포맷 추정해서 파싱하기
retail.head()
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
      <th>Unnamed: 0</th>
      <th>InvoiceNo</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>UnitPrice</th>
      <th>CustomerID</th>
      <th>Country</th>
      <th>CheckoutPrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>536365</td>
      <td>85123A</td>
      <td>WHITE HANGING HEART T-LIGHT HOLDER</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>2.55</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>15.30</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>536365</td>
      <td>71053</td>
      <td>WHITE METAL LANTERN</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>536365</td>
      <td>84406B</td>
      <td>CREAM CUPID HEARTS COAT HANGER</td>
      <td>8</td>
      <td>2010-12-01 08:26:00</td>
      <td>2.75</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>22.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>536365</td>
      <td>84029G</td>
      <td>KNITTED UNION FLAG HOT WATER BOTTLE</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>536365</td>
      <td>84029E</td>
      <td>RED WOOLLY HOTTIE WHITE HEART.</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
    </tr>
  </tbody>
</table>
</div>



#### 우수 고객 확인
- 구매 횟수 기준
- 지불 금액 기준


```python
retail.groupby('CustomerID').count()['Quantity'].sort_values(ascending=False)
```




    CustomerID
    17841    7847
    14911    5675
    14096    5111
    12748    4595
    14606    2700
             ... 
    17846       1
    13017       1
    13099       1
    13106       1
    12346       1
    Name: Quantity, Length: 4338, dtype: int64




```python
retail.groupby('CustomerID').sum()['CheckoutPrice'].sort_values(ascending=False)
```




    CustomerID
    14646    280206.02
    18102    259657.30
    17450    194550.79
    16446    168472.50
    14911    143825.06
               ...    
    16878        13.30
    17956        12.75
    16454         6.90
    14792         6.20
    16738         3.75
    Name: CheckoutPrice, Length: 4338, dtype: float64



#### 사용자 retention 분석
- 월간 사용자 cohort를 바탕으로 월별 재구매율(retention) 분석하기
- heatmap으로 한눈에 재구매율을 파악 가능
  -![goodcustomer](./img/goodcustomer/goodcustomer.png)

#### 사용자 기준으로 최초 구매한 월(month) 연산하기
- Month : 구매월(일(day)을 무시)
- MonthStarted: 사용자가 최초 구매한 달


```python
def get_month_as_datetime(date):
    return datetime(date.year, date.month, 1) #년,월,일 

retail['Month'] = retail['InvoiceDate'].apply(get_month_as_datetime)  # Month 컬럼 생성됨

retail.head()
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
      <th>Unnamed: 0</th>
      <th>InvoiceNo</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>UnitPrice</th>
      <th>CustomerID</th>
      <th>Country</th>
      <th>CheckoutPrice</th>
      <th>Month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>536365</td>
      <td>85123A</td>
      <td>WHITE HANGING HEART T-LIGHT HOLDER</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>2.55</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>15.30</td>
      <td>2010-12-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>536365</td>
      <td>71053</td>
      <td>WHITE METAL LANTERN</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
      <td>2010-12-01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>536365</td>
      <td>84406B</td>
      <td>CREAM CUPID HEARTS COAT HANGER</td>
      <td>8</td>
      <td>2010-12-01 08:26:00</td>
      <td>2.75</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>22.00</td>
      <td>2010-12-01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>536365</td>
      <td>84029G</td>
      <td>KNITTED UNION FLAG HOT WATER BOTTLE</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
      <td>2010-12-01</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>536365</td>
      <td>84029E</td>
      <td>RED WOOLLY HOTTIE WHITE HEART.</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
      <td>2010-12-01</td>
    </tr>
  </tbody>
</table>
</div>




```python
retail.groupby('CustomerID')['Month']
```




    <pandas.core.groupby.generic.SeriesGroupBy object at 0x000001F01E170610>




```python
month_group = retail.groupby('CustomerID')['Month']
retail['MonthStarted'] = month_group.transform(np.min)

retail.tail() #최초로 이용한 달 검색
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
      <th>Unnamed: 0</th>
      <th>InvoiceNo</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>UnitPrice</th>
      <th>CustomerID</th>
      <th>Country</th>
      <th>CheckoutPrice</th>
      <th>Month</th>
      <th>MonthStarted</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>397879</th>
      <td>541904</td>
      <td>581587</td>
      <td>22613</td>
      <td>PACK OF 20 SPACEBOY NAPKINS</td>
      <td>12</td>
      <td>2011-12-09 12:50:00</td>
      <td>0.85</td>
      <td>12680</td>
      <td>France</td>
      <td>10.20</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
    </tr>
    <tr>
      <th>397880</th>
      <td>541905</td>
      <td>581587</td>
      <td>22899</td>
      <td>CHILDREN'S APRON DOLLY GIRL</td>
      <td>6</td>
      <td>2011-12-09 12:50:00</td>
      <td>2.10</td>
      <td>12680</td>
      <td>France</td>
      <td>12.60</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
    </tr>
    <tr>
      <th>397881</th>
      <td>541906</td>
      <td>581587</td>
      <td>23254</td>
      <td>CHILDRENS CUTLERY DOLLY GIRL</td>
      <td>4</td>
      <td>2011-12-09 12:50:00</td>
      <td>4.15</td>
      <td>12680</td>
      <td>France</td>
      <td>16.60</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
    </tr>
    <tr>
      <th>397882</th>
      <td>541907</td>
      <td>581587</td>
      <td>23255</td>
      <td>CHILDRENS CUTLERY CIRCUS PARADE</td>
      <td>4</td>
      <td>2011-12-09 12:50:00</td>
      <td>4.15</td>
      <td>12680</td>
      <td>France</td>
      <td>16.60</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
    </tr>
    <tr>
      <th>397883</th>
      <td>541908</td>
      <td>581587</td>
      <td>22138</td>
      <td>BAKING SET 9 PIECE RETROSPOT</td>
      <td>3</td>
      <td>2011-12-09 12:50:00</td>
      <td>4.95</td>
      <td>12680</td>
      <td>France</td>
      <td>14.85</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
    </tr>
  </tbody>
</table>
</div>



#### 기준이 되는 월과 실제 구매 월의 차이 계산하기
- 각 구매가 최초 구매로 부터 얼마의 월이 지났는지 연산
- MonthPassed : 최초 구매월로부터의 월 차이


```python
retail['MonthPassed'] = (retail['Month'].dt.year - retail['MonthStarted'].dt.year) * 12 + \
    (retail['Month'].dt.month - retail['MonthStarted'].dt.month)
```


```python
retail.tail()
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
      <th>Unnamed: 0</th>
      <th>InvoiceNo</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>UnitPrice</th>
      <th>CustomerID</th>
      <th>Country</th>
      <th>CheckoutPrice</th>
      <th>Month</th>
      <th>MonthStarted</th>
      <th>MonthPassed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>397879</th>
      <td>541904</td>
      <td>581587</td>
      <td>22613</td>
      <td>PACK OF 20 SPACEBOY NAPKINS</td>
      <td>12</td>
      <td>2011-12-09 12:50:00</td>
      <td>0.85</td>
      <td>12680</td>
      <td>France</td>
      <td>10.20</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
      <td>4</td>
    </tr>
    <tr>
      <th>397880</th>
      <td>541905</td>
      <td>581587</td>
      <td>22899</td>
      <td>CHILDREN'S APRON DOLLY GIRL</td>
      <td>6</td>
      <td>2011-12-09 12:50:00</td>
      <td>2.10</td>
      <td>12680</td>
      <td>France</td>
      <td>12.60</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
      <td>4</td>
    </tr>
    <tr>
      <th>397881</th>
      <td>541906</td>
      <td>581587</td>
      <td>23254</td>
      <td>CHILDRENS CUTLERY DOLLY GIRL</td>
      <td>4</td>
      <td>2011-12-09 12:50:00</td>
      <td>4.15</td>
      <td>12680</td>
      <td>France</td>
      <td>16.60</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
      <td>4</td>
    </tr>
    <tr>
      <th>397882</th>
      <td>541907</td>
      <td>581587</td>
      <td>23255</td>
      <td>CHILDRENS CUTLERY CIRCUS PARADE</td>
      <td>4</td>
      <td>2011-12-09 12:50:00</td>
      <td>4.15</td>
      <td>12680</td>
      <td>France</td>
      <td>16.60</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
      <td>4</td>
    </tr>
    <tr>
      <th>397883</th>
      <td>541908</td>
      <td>581587</td>
      <td>22138</td>
      <td>BAKING SET 9 PIECE RETROSPOT</td>
      <td>3</td>
      <td>2011-12-09 12:50:00</td>
      <td>4.95</td>
      <td>12680</td>
      <td>France</td>
      <td>14.85</td>
      <td>2011-12-01</td>
      <td>2011-08-01</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



#### 기준 월, MonthPassed를 기준으로 고객 카운팅
- 기준이 되는 월과 그 월로부터 지난 기간의 고객 수를 계산


```python
def get_unique_no(x):
    return len(np.unique(x))

cohort_group = retail.groupby(['MonthStarted', 'MonthPassed'])
cohort_df = cohort_group['CustomerID'].apply(get_unique_no).reset_index() #reset_index 함수로 index 없애기
cohort_df.head()
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
      <th>MonthStarted</th>
      <th>MonthPassed</th>
      <th>CustomerID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010-12-01</td>
      <td>0</td>
      <td>885</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010-12-01</td>
      <td>1</td>
      <td>324</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010-12-01</td>
      <td>2</td>
      <td>286</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2010-12-01</td>
      <td>3</td>
      <td>340</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2010-12-01</td>
      <td>4</td>
      <td>321</td>
    </tr>
  </tbody>
</table>
</div>



#### 테이블 피벗
- pivot 함수를 이용하여 index는 MonthStarted, columns을 MonthPassed로 변경하여 테이블 변경
- 첫번째 column을 기준으로 100분위 연산


```python
cohort_df = cohort_df.pivot(index='MonthStarted', columns='MonthPassed')
cohort_df.head()
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
      <th colspan="13" halign="left">CustomerID</th>
    </tr>
    <tr>
      <th>MonthPassed</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
    </tr>
    <tr>
      <th>MonthStarted</th>
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
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010-12-01</th>
      <td>885.0</td>
      <td>324.0</td>
      <td>286.0</td>
      <td>340.0</td>
      <td>321.0</td>
      <td>352.0</td>
      <td>321.0</td>
      <td>309.0</td>
      <td>313.0</td>
      <td>350.0</td>
      <td>331.0</td>
      <td>445.0</td>
      <td>235.0</td>
    </tr>
    <tr>
      <th>2011-01-01</th>
      <td>417.0</td>
      <td>92.0</td>
      <td>111.0</td>
      <td>96.0</td>
      <td>134.0</td>
      <td>120.0</td>
      <td>103.0</td>
      <td>101.0</td>
      <td>125.0</td>
      <td>136.0</td>
      <td>152.0</td>
      <td>49.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-02-01</th>
      <td>380.0</td>
      <td>71.0</td>
      <td>71.0</td>
      <td>108.0</td>
      <td>103.0</td>
      <td>94.0</td>
      <td>96.0</td>
      <td>106.0</td>
      <td>94.0</td>
      <td>116.0</td>
      <td>26.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-03-01</th>
      <td>452.0</td>
      <td>68.0</td>
      <td>114.0</td>
      <td>90.0</td>
      <td>101.0</td>
      <td>76.0</td>
      <td>121.0</td>
      <td>104.0</td>
      <td>126.0</td>
      <td>39.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-04-01</th>
      <td>300.0</td>
      <td>64.0</td>
      <td>61.0</td>
      <td>63.0</td>
      <td>59.0</td>
      <td>68.0</td>
      <td>65.0</td>
      <td>78.0</td>
      <td>22.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
customer_cohort = cohort_df.div(cohort_df.iloc[:, 0], axis=0) * 100
#df.iloc[ ]는 row와 column의 이름을 그대로 쓰는 것이 아니라 각 row와 column의 인덱스 값으로 인덱싱하는 방법이다.
customer_cohort.head()
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
      <th colspan="13" halign="left">CustomerID</th>
    </tr>
    <tr>
      <th>MonthPassed</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
    </tr>
    <tr>
      <th>MonthStarted</th>
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
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010-12-01</th>
      <td>100.0</td>
      <td>36.610169</td>
      <td>32.316384</td>
      <td>38.418079</td>
      <td>36.271186</td>
      <td>39.774011</td>
      <td>36.271186</td>
      <td>34.915254</td>
      <td>35.367232</td>
      <td>39.548023</td>
      <td>37.401130</td>
      <td>50.282486</td>
      <td>26.553672</td>
    </tr>
    <tr>
      <th>2011-01-01</th>
      <td>100.0</td>
      <td>22.062350</td>
      <td>26.618705</td>
      <td>23.021583</td>
      <td>32.134293</td>
      <td>28.776978</td>
      <td>24.700240</td>
      <td>24.220624</td>
      <td>29.976019</td>
      <td>32.613909</td>
      <td>36.450839</td>
      <td>11.750600</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-02-01</th>
      <td>100.0</td>
      <td>18.684211</td>
      <td>18.684211</td>
      <td>28.421053</td>
      <td>27.105263</td>
      <td>24.736842</td>
      <td>25.263158</td>
      <td>27.894737</td>
      <td>24.736842</td>
      <td>30.526316</td>
      <td>6.842105</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-03-01</th>
      <td>100.0</td>
      <td>15.044248</td>
      <td>25.221239</td>
      <td>19.911504</td>
      <td>22.345133</td>
      <td>16.814159</td>
      <td>26.769912</td>
      <td>23.008850</td>
      <td>27.876106</td>
      <td>8.628319</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-04-01</th>
      <td>100.0</td>
      <td>21.333333</td>
      <td>20.333333</td>
      <td>21.000000</td>
      <td>19.666667</td>
      <td>22.666667</td>
      <td>21.666667</td>
      <td>26.000000</td>
      <td>7.333333</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
customer_cohort = customer_cohort.round(decimals=2)

customer_cohort
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
      <th colspan="13" halign="left">CustomerID</th>
    </tr>
    <tr>
      <th>MonthPassed</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
    </tr>
    <tr>
      <th>MonthStarted</th>
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
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2010-12-01</th>
      <td>100.0</td>
      <td>36.61</td>
      <td>32.32</td>
      <td>38.42</td>
      <td>36.27</td>
      <td>39.77</td>
      <td>36.27</td>
      <td>34.92</td>
      <td>35.37</td>
      <td>39.55</td>
      <td>37.40</td>
      <td>50.28</td>
      <td>26.55</td>
    </tr>
    <tr>
      <th>2011-01-01</th>
      <td>100.0</td>
      <td>22.06</td>
      <td>26.62</td>
      <td>23.02</td>
      <td>32.13</td>
      <td>28.78</td>
      <td>24.70</td>
      <td>24.22</td>
      <td>29.98</td>
      <td>32.61</td>
      <td>36.45</td>
      <td>11.75</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-02-01</th>
      <td>100.0</td>
      <td>18.68</td>
      <td>18.68</td>
      <td>28.42</td>
      <td>27.11</td>
      <td>24.74</td>
      <td>25.26</td>
      <td>27.89</td>
      <td>24.74</td>
      <td>30.53</td>
      <td>6.84</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-03-01</th>
      <td>100.0</td>
      <td>15.04</td>
      <td>25.22</td>
      <td>19.91</td>
      <td>22.35</td>
      <td>16.81</td>
      <td>26.77</td>
      <td>23.01</td>
      <td>27.88</td>
      <td>8.63</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-04-01</th>
      <td>100.0</td>
      <td>21.33</td>
      <td>20.33</td>
      <td>21.00</td>
      <td>19.67</td>
      <td>22.67</td>
      <td>21.67</td>
      <td>26.00</td>
      <td>7.33</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-05-01</th>
      <td>100.0</td>
      <td>19.01</td>
      <td>17.25</td>
      <td>17.25</td>
      <td>20.77</td>
      <td>23.24</td>
      <td>26.41</td>
      <td>9.51</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-06-01</th>
      <td>100.0</td>
      <td>17.36</td>
      <td>15.70</td>
      <td>26.45</td>
      <td>23.14</td>
      <td>33.47</td>
      <td>9.50</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-07-01</th>
      <td>100.0</td>
      <td>18.09</td>
      <td>20.74</td>
      <td>22.34</td>
      <td>27.13</td>
      <td>11.17</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-08-01</th>
      <td>100.0</td>
      <td>20.71</td>
      <td>24.85</td>
      <td>24.26</td>
      <td>12.43</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-09-01</th>
      <td>100.0</td>
      <td>23.41</td>
      <td>30.10</td>
      <td>11.37</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-10-01</th>
      <td>100.0</td>
      <td>24.02</td>
      <td>11.45</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-11-01</th>
      <td>100.0</td>
      <td>11.15</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2011-12-01</th>
      <td>100.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



#### heatmap 출력하기
- seaborn의 heatmap 함수로 visualization!


```python
xticks = np.arange(0, 13)
yticks = ['2010/12', '2011/01', '2011/02', '2011/03', '2011/04', '2011/05', '2011/06', '2011/07', '2011/08', '2011/09', '2011/10', '2011/11', '2011/12']

plt.figure(figsize = (15, 8))
sns.heatmap(customer_cohort, 
            annot=True, 
            xticklabels=xticks,
            yticklabels=yticks, 
            fmt='.1f')

```




    <AxesSubplot:xlabel='None-MonthPassed', ylabel='MonthStarted'>





![output_22_1](./img/goodcustomer/output_22_1.png)
    

