---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 데이터 기반으로 의사결정하기-푸쉬 노티피케이션 타임
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
1. 푸쉬 노티피케이션 타임 의사 결정 하기

#### 푸쉬 노티피케이션 이란?

push notification은 단적으로 설명한다면 서버에서 발생한 Event를 특정 클라이언트에게 Event 발생 사실을 통지하는 기술입니다. 주변에서 볼 수 있는 가장 흔히 볼 수 있는 사례가 SNS application의 메시지 수신 알림입니다.




```python
import numpy as np
import pandas as pd
# seaborn
import seaborn as sns
COLORS = sns.color_palette()

%matplotlib inline
```


```python
def plot_bar(df, xlabel, ylabel, title, figsize=(20, 10), color=COLORS[-1], rotation=45):
    plot = df.plot(kind='bar', color=color, figsize=figsize)
    plot.set_xlabel(xlabel, fontsize=10)
    plot.set_ylabel(ylabel, fontsize=10)
    plot.set_title(title, fontsize=12)
    plot.set_xticklabels(labels=df.index, rotation=rotation)
```


```python
dtypes = {
    'UnitPrice': np.float32,
    'CustomerID': np.int32,
    'Quantity': np.int32
}
retail = pd.read_csv('./OnlineRetailClean.csv', dtype=dtypes)
retail['InvoiceDate'] = pd.to_datetime(retail['InvoiceDate'], infer_datetime_format=True)
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



#### 쿠폰 발송을 할때, push를 언제 보내는게 좋을까?
- 고객에게 쿠폰 발송을 한다고 기획하고, 회의를 한다고 가정해보겠습니다.
- A: 쿠폰을 언제보내는게 좋을까요?
- B: 아침에 출퇴근 시간에 보내는게 좋을까요?
- C: 점심 먹고 졸린데 그때 보내보죠?
- D: 흠 자기전에 스마트폰 많이 하던데 그때는 어떨까요?
- A: 그러면 평균 시간을 내볼까요?
- K: 아 **데이터**를 확인해보는게 맞지 않을까요? 언제 고객이 주로 주문을 하는지?


- 위에서 처럼 실제로 회의를 하다보면 의사결정이 본인/주변의 경험에 의해서 이뤄지는 것을 많이 볼 수 있습니다.
- 주문이 이뤄지는 시간을 고려하지 않고 막무가내로 보낸다면 아무 의미가 없고, 추후 같은 이벤트 발생시에도 판단 근거가 없게 됨
- 현상태에서는 가장 많이 주문이 일어나는 시점에서 하는 것이 가장 직관적인 판단
  - 1. 데이터로 파악
  - 2. 가설 제시
  - 3. 가설 검증
  - 4. 1-3 반복
- 시간(hour, minute)과 주로 관련되기 때문에 역시 InvoiceDate가 중요한 feature



```python
order_by_hour = retail.set_index('InvoiceDate').groupby(lambda date: date.hour).count()
order_by_hour
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
      <th>UnitPrice</th>
      <th>CustomerID</th>
      <th>Country</th>
      <th>CheckoutPrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>379</td>
      <td>379</td>
      <td>379</td>
      <td>379</td>
      <td>379</td>
      <td>379</td>
      <td>379</td>
      <td>379</td>
      <td>379</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
      <td>8690</td>
    </tr>
    <tr>
      <th>9</th>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
      <td>21944</td>
    </tr>
    <tr>
      <th>10</th>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
      <td>37997</td>
    </tr>
    <tr>
      <th>11</th>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
      <td>49084</td>
    </tr>
    <tr>
      <th>12</th>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
      <td>72065</td>
    </tr>
    <tr>
      <th>13</th>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
      <td>64026</td>
    </tr>
    <tr>
      <th>14</th>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
      <td>54118</td>
    </tr>
    <tr>
      <th>15</th>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
      <td>45369</td>
    </tr>
    <tr>
      <th>16</th>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
      <td>24089</td>
    </tr>
    <tr>
      <th>17</th>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
      <td>13071</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
      <td>2928</td>
    </tr>
    <tr>
      <th>19</th>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
      <td>3321</td>
    </tr>
    <tr>
      <th>20</th>
      <td>802</td>
      <td>802</td>
      <td>802</td>
      <td>802</td>
      <td>802</td>
      <td>802</td>
      <td>802</td>
      <td>802</td>
      <td>802</td>
    </tr>
  </tbody>
</table>
</div>




```python
order_by_hour = retail.set_index('InvoiceDate').groupby(lambda date: date.hour).count()['CustomerID']
order_by_hour
```




    6         1
    7       379
    8      8690
    9     21944
    10    37997
    11    49084
    12    72065
    13    64026
    14    54118
    15    45369
    16    24089
    17    13071
    18     2928
    19     3321
    20      802
    Name: CustomerID, dtype: int64




```python
plot_bar(order_by_hour, 'hour', '# orders', 'Order by hour')
```



![output_7_0](./img/pushnoti/output_7_0.png)




```python
def half_an_hour(date):
    minute = ':00'
    if date.minute > 30:
        minute = ':30'
    hour = str(date.hour)
    if date.hour < 10:
        hour = '0' + hour
    
    return hour + minute
```


```python
order_by_hour_half = retail.set_index('InvoiceDate').groupby(half_an_hour).count()['CustomerID']
order_by_hour_half
```




    06:00        1
    07:30      379
    08:00     3145
    08:30     5545
    09:00     9364
    09:30    12580
    10:00    16950
    10:30    21047
    11:00    18925
    11:30    30159
    12:00    37174
    12:30    34891
    13:00    31131
    13:30    32895
    14:00    26958
    14:30    27160
    15:00    24227
    15:30    21142
    16:00    14316
    16:30     9773
    17:00     8889
    17:30     4182
    18:00     1715
    18:30     1213
    19:00     1534
    19:30     1787
    20:00      802
    Name: CustomerID, dtype: int64




```python
order_by_hour_half / order_by_hour_half.sum()
```




    06:00    0.000003
    07:30    0.000953
    08:00    0.007904
    08:30    0.013936
    09:00    0.023534
    09:30    0.031617
    10:00    0.042600
    10:30    0.052897
    11:00    0.047564
    11:30    0.075798
    12:00    0.093429
    12:30    0.087691
    13:00    0.078241
    13:30    0.082675
    14:00    0.067753
    14:30    0.068261
    15:00    0.060890
    15:30    0.053136
    16:00    0.035980
    16:30    0.024562
    17:00    0.022341
    17:30    0.010511
    18:00    0.004310
    18:30    0.003049
    19:00    0.003855
    19:30    0.004491
    20:00    0.002016
    Name: CustomerID, dtype: float64




```python
plot_bar(order_by_hour_half, 'half an hour', '# orders', 'order by half an hour')
```



![output_11_0](./img/pushnoti/output_11_0.png)



#### 개인화된 push notification
- 아마존을 필두로, 개인화(personalization)하여 맞춤으로 사용자마다 최적의 솔루션을 찾는것이 트렌드가 됨
- 사용자별로 소비의 패턴이 다를 수 있기 때문에, 가장 많이 구매한 시간대를 찾아서 해당 시간대에 쿠폰을 발송!

#### 사용자별 각 시간별 주문 량 계산하기


```python
order_count_by_hour = retail.set_index('InvoiceDate').groupby(['CustomerID', lambda date: date.hour]).count()['StockCode']
order_count_by_hour #12346 ID 는 10 시에 1건 , 12347  ID는 8 시에 22건 
```




    CustomerID    
    12346       10     1
    12347       8     22
                10    24
                12    47
                13    18
                      ..
    18283       15     1
                16    56
                19    87
    18287       9      3
                10    67
    Name: StockCode, Length: 11205, dtype: int64




```python
order_count_by_hour.loc[12347] #2시에 가장 많은 주문을 함
```




    8     22
    10    24
    12    47
    13    18
    14    60
    15    11
    Name: StockCode, dtype: int64



#### 사용자별 최대 주문 시간 계산하기
- 가장 많은 주문량을 보인 시간을 계산


```python
order_count_by_hour.groupby('CustomerID').idxmax() #idxmax 함수는 최댓값을 가진 index를 반환한다
```




    CustomerID
    12346    (12346, 10)
    12347    (12347, 14)
    12348    (12348, 19)
    12349     (12349, 9)
    12350    (12350, 16)
                ...     
    18280     (18280, 9)
    18281    (18281, 10)
    18282    (18282, 13)
    18283    (18283, 14)
    18287    (18287, 10)
    Name: StockCode, Length: 4338, dtype: object




```python
idx = order_count_by_hour.groupby('CustomerID').idxmax() #idxmax 함수는 최댓값을 가진 index를 반환한다
```

#### 해당 시간 indexing


```python
result = order_count_by_hour.loc[idx]
result
```




    CustomerID    
    12346       10      1
    12347       14     60
    12348       19     17
    12349       9      73
    12350       16     17
                     ... 
    18280       9      10
    18281       10      7
    18282       13      7
    18283       14    201
    18287       10     67
    Name: StockCode, Length: 4338, dtype: int64




```python
result.reset_index()
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
      <th>CustomerID</th>
      <th>level_1</th>
      <th>StockCode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12346</td>
      <td>10</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12347</td>
      <td>14</td>
      <td>60</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12348</td>
      <td>19</td>
      <td>17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12349</td>
      <td>9</td>
      <td>73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12350</td>
      <td>16</td>
      <td>17</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4333</th>
      <td>18280</td>
      <td>9</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4334</th>
      <td>18281</td>
      <td>10</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4335</th>
      <td>18282</td>
      <td>13</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4336</th>
      <td>18283</td>
      <td>14</td>
      <td>201</td>
    </tr>
    <tr>
      <th>4337</th>
      <td>18287</td>
      <td>10</td>
      <td>67</td>
    </tr>
  </tbody>
</table>
<p>4338 rows × 3 columns</p>
</div>




```python
result.reset_index().groupby('level_1').groups #시간대별로 그룹화하기
```




    {7: [73, 269, 319, 344, 375, 893, 1667, 2317], 8: [46, 58, 87, 126, 172, 179, 187, 260, 278, 279, 282, 292, 306, 347, 399, 429, 496, 503, 526, 533, 549, 552, 651, 671, 747, 755, 784, 792, 800, 803, 806, 821, 838, 877, 883, 920, 944, 947, 951, 954, 1008, 1093, 1106, 1120, 1138, 1172, 1173, 1217, 1251, 1397, 1422, 1424, 1436, 1472, 1512, 1616, 1621, 1666, 1668, 1678, 1687, 1734, 1759, 1761, 1774, 1791, 1815, 1827, 1846, 1859, 1895, 1900, 1903, 1996, 2018, 2023, 2054, 2085, 2108, 2117, 2167, 2172, 2253, 2380, 2383, 2403, 2404, 2417, 2427, 2462, 2464, 2643, 2749, 2776, 2781, 2896, 2936, 2949, 3021, 3130, ...], 9: [3, 9, 26, 30, 33, 35, 37, 48, 60, 66, 75, 84, 86, 90, 100, 106, 107, 121, 127, 135, 138, 142, 144, 146, 154, 159, 181, 199, 230, 240, 264, 265, 267, 277, 280, 286, 294, 298, 328, 333, 336, 342, 343, 352, 362, 366, 385, 402, 421, 459, 470, 475, 478, 482, 483, 509, 517, 519, 574, 603, 615, 630, 636, 642, 644, 691, 701, 706, 707, 746, 749, 752, 764, 770, 781, 783, 818, 825, 829, 844, 859, 874, 887, 925, 934, 950, 969, 981, 992, 998, 1003, 1004, 1016, 1032, 1038, 1045, 1050, 1053, 1063, 1082, ...], 10: [0, 11, 21, 27, 28, 41, 42, 45, 49, 51, 55, 61, 77, 93, 94, 103, 104, 105, 110, 113, 122, 132, 137, 140, 147, 150, 155, 156, 165, 168, 169, 174, 178, 182, 186, 195, 205, 206, 208, 216, 217, 222, 231, 233, 242, 251, 252, 255, 263, 275, 276, 287, 288, 290, 293, 301, 310, 314, 322, 331, 337, 339, 341, 348, 359, 360, 361, 363, 364, 365, 379, 381, 407, 437, 439, 441, 443, 450, 464, 465, 468, 471, 481, 499, 500, 511, 516, 529, 541, 553, 560, 563, 570, 578, 584, 586, 590, 591, 595, 596, ...], 11: [29, 32, 34, 57, 99, 102, 111, 124, 139, 148, 163, 171, 176, 188, 207, 220, 223, 228, 234, 246, 253, 254, 256, 266, 272, 311, 313, 315, 324, 326, 330, 346, 349, 355, 356, 380, 393, 400, 419, 423, 424, 427, 430, 431, 449, 458, 462, 485, 487, 515, 521, 528, 542, 545, 550, 559, 567, 569, 575, 605, 616, 635, 648, 650, 654, 658, 664, 677, 678, 680, 692, 693, 694, 702, 712, 729, 744, 748, 763, 765, 771, 778, 793, 798, 812, 819, 824, 828, 831, 837, 843, 846, 851, 856, 866, 868, 869, 873, 875, 903, ...], 12: [12, 20, 22, 36, 50, 62, 64, 67, 72, 74, 81, 116, 120, 123, 145, 151, 158, 160, 164, 189, 191, 193, 200, 203, 209, 226, 237, 238, 241, 243, 244, 245, 249, 259, 270, 271, 284, 297, 305, 308, 317, 327, 332, 335, 350, 357, 367, 371, 376, 377, 388, 390, 391, 397, 398, 403, 404, 414, 415, 418, 428, 432, 435, 436, 440, 451, 460, 473, 477, 488, 489, 490, 492, 495, 504, 510, 525, 540, 565, 568, 577, 582, 585, 594, 598, 599, 611, 612, 613, 622, 624, 625, 631, 634, 643, 649, 653, 655, 666, 675, ...], 13: [7, 8, 14, 16, 18, 23, 43, 44, 52, 59, 70, 71, 76, 82, 83, 97, 98, 108, 112, 114, 115, 119, 143, 149, 166, 167, 183, 190, 198, 201, 202, 204, 212, 213, 225, 227, 232, 236, 239, 257, 258, 262, 300, 303, 312, 329, 340, 351, 353, 368, 369, 372, 374, 382, 383, 384, 394, 396, 406, 416, 417, 422, 438, 445, 448, 452, 455, 456, 466, 474, 493, 505, 506, 512, 534, 535, 537, 548, 551, 556, 561, 581, 601, 609, 610, 614, 617, 623, 632, 639, 647, 659, 660, 668, 669, 676, 681, 684, 685, 687, ...], 14: [1, 5, 25, 31, 38, 40, 54, 56, 69, 78, 79, 85, 88, 95, 96, 101, 109, 118, 125, 129, 130, 131, 141, 152, 162, 173, 175, 177, 196, 197, 215, 219, 221, 247, 273, 281, 291, 295, 296, 318, 325, 334, 354, 358, 389, 395, 401, 405, 408, 412, 413, 425, 433, 457, 461, 463, 480, 486, 491, 494, 501, 507, 520, 522, 524, 530, 538, 539, 555, 557, 562, 572, 573, 579, 583, 588, 589, 618, 626, 627, 640, 641, 645, 646, 661, 663, 665, 696, 697, 699, 720, 725, 726, 735, 745, 760, 761, 799, 801, 809, ...], 15: [13, 15, 17, 24, 65, 68, 91, 92, 117, 134, 136, 161, 170, 180, 184, 194, 211, 214, 218, 229, 235, 250, 268, 274, 285, 299, 304, 307, 309, 338, 345, 373, 378, 386, 392, 409, 410, 411, 434, 444, 446, 467, 476, 479, 497, 498, 502, 513, 514, 527, 531, 532, 536, 544, 564, 566, 576, 592, 600, 602, 607, 619, 620, 621, 629, 638, 674, 689, 705, 714, 734, 739, 740, 777, 787, 789, 791, 796, 804, 814, 823, 827, 832, 855, 857, 861, 882, 888, 900, 902, 935, 938, 941, 952, 953, 962, 972, 977, 979, 982, ...], 16: [4, 10, 19, 39, 53, 128, 133, 157, 192, 210, 224, 248, 302, 316, 323, 370, 387, 420, 442, 447, 454, 469, 472, 484, 518, 523, 543, 546, 554, 558, 580, 587, 604, 628, 657, 662, 672, 682, 704, 788, 794, 833, 834, 847, 850, 908, 930, 940, 964, 970, 999, 1029, 1036, 1048, 1067, 1096, 1107, 1115, 1116, 1144, 1174, 1177, 1189, 1219, 1224, 1239, 1270, 1273, 1279, 1288, 1314, 1322, 1331, 1343, 1355, 1359, 1366, 1377, 1380, 1468, 1473, 1477, 1484, 1488, 1490, 1500, 1506, 1526, 1564, 1566, 1574, 1585, 1638, 1676, 1692, 1772, 1799, 1820, 1834, 1848, ...], 17: [6, 63, 89, 153, 185, 261, 283, 289, 321, 426, 508, 547, 571, 593, 652, 670, 703, 719, 722, 754, 836, 845, 907, 936, 1019, 1088, 1140, 1188, 1240, 1296, 1379, 1489, 1540, 1578, 1588, 1590, 1603, 1628, 1640, 1642, 1679, 1739, 1742, 1889, 1906, 1940, 2058, 2156, 2169, 2274, 2279, 2340, 2374, 2408, 2443, 2515, 2566, 2568, 2583, 2594, 2602, 2621, 2644, 2661, 2853, 2856, 2886, 2928, 2948, 2978, 2995, 3004, 3069, 3102, 3141, 3173, 3185, 3226, 3230, 3277, 3323, 3401, 3404, 3425, 3443, 3461, 3462, 3466, 3512, 3543, 3559, 3598, 3639, 3659, 3677, 3690, 3716, 3727, 3736, 3739, ...], 18: [80, 320, 453, 637, 767, 862, 879, 1128, 1326, 1378, 1498, 1519, 1624, 1652, 1758, 1768, 1844, 2879, 3198, 3467, 3511, 3537, 3767, 3802, 3820, 3837, 4072, 4077, 4079, 4273], 19: [2, 47, 667, 1589, 1591, 1639, 1730, 1776, 1928, 2044, 2448, 2548, 2876, 3002, 3047, 3261, 3274, 3479, 3556, 3652, 3685, 3789, 3812, 4324], 20: [1646, 1943, 3804, 3838, 4050, 4110]}



- 시간대별로 어떤 고객이 가장많은 주문을 했는지 확인가능

