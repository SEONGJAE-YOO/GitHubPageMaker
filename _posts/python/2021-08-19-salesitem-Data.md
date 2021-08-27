---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 매출,가장 많이 팔린 아이템 확인하기 
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-08-19
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}

### 학습목표
1. 아이템별 지표 확인하기
2. 시간별 지역별 판매 지표 확인하기


```python
import numpy as np
import pandas as pd
# seaborn
import seaborn as sns
COLORS = sns.color_palette()

%matplotlib inline
```

#### 데이터 로딩
1. 정제된 데이터 사용(retail.csv)


```python
dtypes = {
    'UnitPrice': np.float32,
    'CustomerID': np.int32,
    'Quantity': np.int32
}
retail = pd.read_csv('./OnlineRetailClean.csv', dtype=dtypes)
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
      <td>12/1/2010 8:26</td>
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
      <td>12/1/2010 8:26</td>
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
      <td>12/1/2010 8:26</td>
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
      <td>12/1/2010 8:26</td>
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
      <td>12/1/2010 8:26</td>
      <td>3.39</td>
      <td>17850</td>
      <td>United Kingdom</td>
      <td>20.34</td>
    </tr>
  </tbody>
</table>
</div>



#### 날짜 타입 데이터 변환
- 문자열로 로딩하는 것보다 date/datetime 타입으로 로딩하는 것이 분석에 용이


```python
retail['InvoiceDate'] = pd.to_datetime(retail['InvoiceDate'], infer_datetime_format=True)# infer_datetime_format=True 날짜시간 포맷 추정해서 파싱하기
retail.info() #5   InvoiceDate    397884 non-null  datetime64[ns]   바꿔짐
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 397884 entries, 0 to 397883
    Data columns (total 10 columns):
     #   Column         Non-Null Count   Dtype         
    ---  ------         --------------   -----         
     0   Unnamed: 0     397884 non-null  int64         
     1   InvoiceNo      397884 non-null  int64         
     2   StockCode      397884 non-null  object        
     3   Description    397884 non-null  object        
     4   Quantity       397884 non-null  int32         
     5   InvoiceDate    397884 non-null  datetime64[ns]
     6   UnitPrice      397884 non-null  float32       
     7   CustomerID     397884 non-null  int32         
     8   Country        397884 non-null  object        
     9   CheckoutPrice  397884 non-null  float64       
    dtypes: datetime64[ns](1), float32(1), float64(1), int32(2), int64(2), object(3)
    memory usage: 25.8+ MB


#### 해당 기간 동안의 매출
- 전체 매출
- 국가별 매출
- 월별 매출
- 요일별 매출
- 시간별 매출

#### 전체 매출


```python
total_revenue = retail['CheckoutPrice'].sum()
total_revenue
```




    8911407.904



#### 국가별 매출


```python
rev_by_countries = retail.groupby('Country').sum()['CheckoutPrice'].sort_values()
rev_by_countries
```




    Country
    Saudi Arabia            1.459200e+02
    Bahrain                 5.484000e+02
    Czech Republic          8.267400e+02
    RSA                     1.002310e+03
    Brazil                  1.143600e+03
    European Community      1.300250e+03
    Lithuania               1.661060e+03
    Lebanon                 1.693880e+03
    United Arab Emirates    1.902280e+03
    Unspecified             2.667070e+03
    Malta                   2.725590e+03
    USA                     3.580390e+03
    Canada                  3.666380e+03
    Iceland                 4.310000e+03
    Greece                  4.760520e+03
    Israel                  7.221690e+03
    Poland                  7.334650e+03
    Austria                 1.019868e+04
    Cyprus                  1.359038e+04
    Italy                   1.748324e+04
    Denmark                 1.895534e+04
    Channel Islands         2.045044e+04
    Singapore               2.127929e+04
    Finland                 2.254608e+04
    Portugal                3.343989e+04
    Norway                  3.616544e+04
    Japan                   3.741637e+04
    Sweden                  3.837833e+04
    Belgium                 4.119634e+04
    Switzerland             5.644395e+04
    Spain                   6.157711e+04
    Australia               1.385213e+05
    France                  2.090240e+05
    Germany                 2.288671e+05
    EIRE                    2.655459e+05
    Netherlands             2.854463e+05
    United Kingdom          7.308392e+06
    Name: CheckoutPrice, dtype: float64




```python
plot = rev_by_countries.plot(kind='bar', color=COLORS[-1], figsize=(20, 10))
plot.set_xlabel('Country', fontsize=11)
plot.set_ylabel('Revenue', fontsize=11)
plot.set_title('Revenue by Country', fontsize=13)
plot.set_xticklabels(labels=rev_by_countries.index, rotation=45)
```




    [Text(0, 0, 'Saudi Arabia'),
     Text(1, 0, 'Bahrain'),
     Text(2, 0, 'Czech Republic'),
     Text(3, 0, 'RSA'),
     Text(4, 0, 'Brazil'),
     Text(5, 0, 'European Community'),
     Text(6, 0, 'Lithuania'),
     Text(7, 0, 'Lebanon'),
     Text(8, 0, 'United Arab Emirates'),
     Text(9, 0, 'Unspecified'),
     Text(10, 0, 'Malta'),
     Text(11, 0, 'USA'),
     Text(12, 0, 'Canada'),
     Text(13, 0, 'Iceland'),
     Text(14, 0, 'Greece'),
     Text(15, 0, 'Israel'),
     Text(16, 0, 'Poland'),
     Text(17, 0, 'Austria'),
     Text(18, 0, 'Cyprus'),
     Text(19, 0, 'Italy'),
     Text(20, 0, 'Denmark'),
     Text(21, 0, 'Channel Islands'),
     Text(22, 0, 'Singapore'),
     Text(23, 0, 'Finland'),
     Text(24, 0, 'Portugal'),
     Text(25, 0, 'Norway'),
     Text(26, 0, 'Japan'),
     Text(27, 0, 'Sweden'),
     Text(28, 0, 'Belgium'),
     Text(29, 0, 'Switzerland'),
     Text(30, 0, 'Spain'),
     Text(31, 0, 'Australia'),
     Text(32, 0, 'France'),
     Text(33, 0, 'Germany'),
     Text(34, 0, 'EIRE'),
     Text(35, 0, 'Netherlands'),
     Text(36, 0, 'United Kingdom')]





![output_11_1](./img/salesitem/output_11_1.png)




```python
rev_by_countries / total_revenue #비율 확인할 수 있다.
```




    Country
    Saudi Arabia            0.000016
    Bahrain                 0.000062
    Czech Republic          0.000093
    RSA                     0.000112
    Brazil                  0.000128
    European Community      0.000146
    Lithuania               0.000186
    Lebanon                 0.000190
    United Arab Emirates    0.000213
    Unspecified             0.000299
    Malta                   0.000306
    USA                     0.000402
    Canada                  0.000411
    Iceland                 0.000484
    Greece                  0.000534
    Israel                  0.000810
    Poland                  0.000823
    Austria                 0.001144
    Cyprus                  0.001525
    Italy                   0.001962
    Denmark                 0.002127
    Channel Islands         0.002295
    Singapore               0.002388
    Finland                 0.002530
    Portugal                0.003752
    Norway                  0.004058
    Japan                   0.004199
    Sweden                  0.004307
    Belgium                 0.004623
    Switzerland             0.006334
    Spain                   0.006910
    Australia               0.015544
    France                  0.023456
    Germany                 0.025682
    EIRE                    0.029798
    Netherlands             0.032032
    United Kingdom          0.820116
    Name: CheckoutPrice, dtype: float64



#### 그래프 유틸 함수


```python
def plot_bar(df, xlabel, ylabel, title, color=COLORS[0], figsize=(20, 10), rotation=45):
    plot = df.plot(kind='bar', color=color, figsize=figsize)
    plot.set_xlabel(xlabel, fontsize=11)
    plot.set_ylabel(ylabel, fontsize=11)
    plot.set_title(title, fontsize=13)
    plot.set_xticklabels(labels=df.index, rotation=rotation)
                   
plot_bar(rev_by_countries, 'Country', 'Revenue', 'Revenue by Country')
```



![output_14_0](./img/salesitem/output_14_0.png)



#### 월별 매출


```python
retail['InvoiceDate'].sort_values(ascending=False)
```




    397883   2011-12-09 12:50:00
    397876   2011-12-09 12:50:00
    397870   2011-12-09 12:50:00
    397871   2011-12-09 12:50:00
    397872   2011-12-09 12:50:00
                     ...        
    3        2010-12-01 08:26:00
    1        2010-12-01 08:26:00
    5        2010-12-01 08:26:00
    6        2010-12-01 08:26:00
    0        2010-12-01 08:26:00
    Name: InvoiceDate, Length: 397884, dtype: datetime64[ns]




```python
def extract_month(date):
    month = str(date.month)
    if date.month < 10:
        month = '0' + month
    return str(date.year) + month 
```


```python
rev_by_month = retail.set_index('InvoiceDate').groupby(extract_month).sum()['CheckoutPrice']
rev_by_month

```




    201012     572713.890
    201101     569445.040
    201102     447137.350
    201103     595500.760
    201104     469200.361
    201105     678594.560
    201106     661213.690
    201107     600091.011
    201108     645343.900
    201109     952838.382
    201110    1039318.790
    201111    1161817.380
    201112     518192.790
    Name: CheckoutPrice, dtype: float64




```python
plot_bar(rev_by_month, 'Month', 'Revenue', 'Revenue by Month')
```



![output_19_0](./img/salesitem/output_19_0.png)



#### 요일별 매출


```python
rev_by_dow = retail.set_index('InvoiceDate').groupby(lambda date:date.dayofweek).sum()['CheckoutPrice']
rev_by_dow
```




    0    1367146.411
    1    1700634.631
    2    1588336.170
    3    1976859.070
    4    1485917.401
    6     792514.221
    Name: CheckoutPrice, dtype: float64




```python
DAY_OF_WEEK = np.array(['Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat', 'Sun'])
rev_by_dow.index = DAY_OF_WEEK[rev_by_dow.index]
plot_bar(rev_by_dow, 'DOW', 'Revenue', 'Revenue by DOW')
```



![output_22_0](./img/salesitem/output_22_0.png)



#### 시간별 매출


```python
rev_by_hour = retail.set_index('InvoiceDate').groupby(lambda date:date.hour).sum()['CheckoutPrice']
plot_bar(rev_by_hour, 'hour', 'revenue', 'revenue by hour')
```



![output_24_0](./img/salesitem/output_24_0.png)



#### 매출 데이터로부터 insight
- 전체 매출의 82%가 UK에서 발생
- 11년도의 가장 많은 주문이 발생한 달 11월(12월의 전체 데이터가 반영이 되진 않았음)
- 11, 12월의 판매량이 압도(블랙프라이데이, 사이버먼데이, 크리스마스 휴일)
- 일주일중 목요일까지는 성장세를 보이다가, 이후로 하락(토요일에는 주문X)
- 7시를 시작으로 주문이 시작되어 12시까지 증가세, 15시까지 하락을, 15시 이후 부터 급락)

#### 제품별 metrics
- Top 10 판매 제품
- Top 10 매출 제품


```python
top_selling = retail.groupby('StockCode').sum()['Quantity'].sort_values(ascending=False)[:3]
top_selling
```




    StockCode
    23843    80995
    23166    77916
    84077    54415
    Name: Quantity, dtype: int32




```python
top_revenue = retail.groupby('StockCode').sum()['CheckoutPrice'].sort_values(ascending=False)[:10]
top_revenue
```




    StockCode
    23843     168469.60
    22423     142592.95
    85123A    100603.50
    85099B     85220.78
    23166      81416.73
    POST       77803.96
    47566      68844.33
    84879      56580.34
    M          53779.93
    23084      51346.20
    Name: CheckoutPrice, dtype: float64



#### top 3 아이템의 월별 판매량 추이


```python
retail.set_index('InvoiceDate').groupby(['StockCode', extract_month]).sum()[['Quantity', 'CheckoutPrice']].loc[top_selling.index]
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
      <th>Quantity</th>
      <th>CheckoutPrice</th>
    </tr>
    <tr>
      <th>StockCode</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>23843</th>
      <th>201112</th>
      <td>80995</td>
      <td>168469.60</td>
    </tr>
    <tr>
      <th rowspan="9" valign="top">23166</th>
      <th>201101</th>
      <td>74215</td>
      <td>77183.60</td>
    </tr>
    <tr>
      <th>201105</th>
      <td>792</td>
      <td>869.04</td>
    </tr>
    <tr>
      <th>201106</th>
      <td>391</td>
      <td>458.51</td>
    </tr>
    <tr>
      <th>201107</th>
      <td>718</td>
      <td>826.94</td>
    </tr>
    <tr>
      <th>201108</th>
      <td>405</td>
      <td>486.09</td>
    </tr>
    <tr>
      <th>201109</th>
      <td>342</td>
      <td>397.26</td>
    </tr>
    <tr>
      <th>201110</th>
      <td>235</td>
      <td>283.67</td>
    </tr>
    <tr>
      <th>201111</th>
      <td>631</td>
      <td>708.11</td>
    </tr>
    <tr>
      <th>201112</th>
      <td>187</td>
      <td>203.51</td>
    </tr>
    <tr>
      <th rowspan="13" valign="top">84077</th>
      <th>201012</th>
      <td>5139</td>
      <td>1150.47</td>
    </tr>
    <tr>
      <th>201101</th>
      <td>1488</td>
      <td>385.44</td>
    </tr>
    <tr>
      <th>201102</th>
      <td>3457</td>
      <td>795.17</td>
    </tr>
    <tr>
      <th>201103</th>
      <td>3888</td>
      <td>943.20</td>
    </tr>
    <tr>
      <th>201104</th>
      <td>10224</td>
      <td>2281.44</td>
    </tr>
    <tr>
      <th>201105</th>
      <td>4944</td>
      <td>1249.44</td>
    </tr>
    <tr>
      <th>201106</th>
      <td>1920</td>
      <td>533.76</td>
    </tr>
    <tr>
      <th>201107</th>
      <td>3600</td>
      <td>982.56</td>
    </tr>
    <tr>
      <th>201108</th>
      <td>2256</td>
      <td>654.24</td>
    </tr>
    <tr>
      <th>201109</th>
      <td>3462</td>
      <td>985.70</td>
    </tr>
    <tr>
      <th>201110</th>
      <td>8174</td>
      <td>1953.98</td>
    </tr>
    <tr>
      <th>201111</th>
      <td>4500</td>
      <td>1294.20</td>
    </tr>
    <tr>
      <th>201112</th>
      <td>1363</td>
      <td>376.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
monthly_top3 = retail.set_index('InvoiceDate').groupby(['StockCode', extract_month]).sum()[['Quantity', 'CheckoutPrice']].loc[top_selling.index]
```


```python
plot_bar(monthly_top3['CheckoutPrice'], 'Product/Month', 'Revenue', 'Revenue of top 3 items')
```



![output_32_0](./img/salesitem/output_32_0.png)
    

