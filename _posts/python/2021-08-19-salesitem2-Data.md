---
date: 2021-08-19 19:23:00
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 응용-매출,가장 많이 팔린 아이템 확인하기
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
# 그래프를 노트북 안에 그리기 위해 설정
```


```python
# matplotlib 한글 폰트 출력코드
import matplotlib.pyplot as plt
import matplotlib
matplotlib.font_manager._rebuild()

from matplotlib import style
from matplotlib import font_manager , rc
import platform
platform.platform() #'Windows-10-10.0.19041-SP0' / 시스템 운영체제 확인 가능

try :
    if platform.sys() == 'windows':
        # 윈도우인 경우
        font_name = fontmanager.FontProperties(fname="c:/windows/Fonts/나눔고딕.ttf").get_name()
        rc('font', family=font_name)
    else:
        #max 인 경우
        rc('font', family='AppleGothic')
except:
    pass
matplotlib.rcParams['axes.unicode_minus'] = False
# rcParams 폰트 크기를 지정하기 
# 그래프에서 마이너스 기호가 표시되도록 하는 설정입니다. 

```

    Matplotlib is building the font cache; this may take a moment.



```python
plt.rcParams["font.family"] = 'serif'
print (plt.rcParams['font.family'] ) #현재 설정 된 폰트 ['sans-serif']이다 

```

    ['serif']



```python
!conda install fonts-nanum*
```

    Collecting package metadata (current_repodata.json): ...working... failed

    
    CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://repo.anaconda.com/pkgs/main/win-64/current_repodata.json>
    Elapsed: -
    
    An HTTP error occurred when trying to retrieve this URL.
    HTTP errors are often intermittent, and a simple retry will get you on your way.
    
    If your current network has https://www.anaconda.com blocked, please file
    a support request with your network engineering team.
    
    'https://repo.anaconda.com/pkgs/main/win-64'








```python
#I was able to get rid of the RuntimeWarning by declaring the font usage with:
#plt.rcParams["font.serif"] = "nanumGothic"
# 참조 -https://github.com/matplotlib/matplotlib/issues/17007
import matplotlib.pyplot as plt
plt.rcParams["font.serif"]= "nanumGothic"
fig, ax = plt.subplots()

ax.set_title('my font')
ax.set_xlabel(r'my font $\alpha\beta\gamma$')
ax.set_yticks([-1,0,1])
plt.show()
```



![output_5_0](./img/salesitem2/output_5_0.png)



#### 데이터 로딩
1. 정제된 데이터 사용(OnlineRetailCleanhangle.csv)


```python
dtypes = {
    '상품 가격': np.float32,
    '고객 아이디': np.int32,
    '주문 수량': np.int32
}
# 한글 깨짐 해결 방안
#[거의 해결] 해결책 (3) - Excel에서 인코딩 옵션 변경
# 파일을 우선 Excel에서 열어줍니다.
# 파일 - 다른 이름으로 저장에서 - CSV UTF-8 (쉼표로 분리) 로 변경하여 저장합니다.

retail = pd.read_csv('./OnlineRetailCleanhangle.csv', 
                     dtype=dtypes , 
                     encoding = 'utf-8')
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
      <th>주문 번호</th>
      <th>아이템 아이디</th>
      <th>상품 설명</th>
      <th>주문 수량</th>
      <th>주문 시각</th>
      <th>상품 가격</th>
      <th>고객 아이디</th>
      <th>고객 거주 지역</th>
      <th>총 주문 가격</th>
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
retail['주문 시각'] = pd.to_datetime(retail['주문 시각'], infer_datetime_format=True)# infer_datetime_format=True 날짜시간 포맷 추정해서 파싱하기
retail.info() #5  주문 시각       397884 non-null  datetime64[ns]  바꿔짐
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 397884 entries, 0 to 397883
    Data columns (total 10 columns):
     #   Column      Non-Null Count   Dtype         
    ---  ------      --------------   -----         
     0   Unnamed: 0  397884 non-null  int64         
     1   주문 번호       397884 non-null  int64         
     2   아이템 아이디     397884 non-null  object        
     3   상품 설명       397884 non-null  object        
     4   주문 수량       397884 non-null  int32         
     5   주문 시각       397884 non-null  datetime64[ns]
     6   상품 가격       397884 non-null  float32       
     7   고객 아이디      397884 non-null  int32         
     8   고객 거주 지역    397884 non-null  object        
     9   총 주문 가격     397884 non-null  float64       
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
total_revenue = retail['총 주문 가격'].sum()
total_revenue
```




    8911407.904



#### 국가별 매출


```python
rev_by_countries = retail.groupby('고객 거주 지역').sum()['총 주문 가격'].sort_values(ascending=False) # 내림차순 : ascending=False
rev_by_countries
```




    고객 거주 지역
    United Kingdom          7.308392e+06
    Netherlands             2.854463e+05
    EIRE                    2.655459e+05
    Germany                 2.288671e+05
    France                  2.090240e+05
    Australia               1.385213e+05
    Spain                   6.157711e+04
    Switzerland             5.644395e+04
    Belgium                 4.119634e+04
    Sweden                  3.837833e+04
    Japan                   3.741637e+04
    Norway                  3.616544e+04
    Portugal                3.343989e+04
    Finland                 2.254608e+04
    Singapore               2.127929e+04
    Channel Islands         2.045044e+04
    Denmark                 1.895534e+04
    Italy                   1.748324e+04
    Cyprus                  1.359038e+04
    Austria                 1.019868e+04
    Poland                  7.334650e+03
    Israel                  7.221690e+03
    Greece                  4.760520e+03
    Iceland                 4.310000e+03
    Canada                  3.666380e+03
    USA                     3.580390e+03
    Malta                   2.725590e+03
    Unspecified             2.667070e+03
    United Arab Emirates    1.902280e+03
    Lebanon                 1.693880e+03
    Lithuania               1.661060e+03
    European Community      1.300250e+03
    Brazil                  1.143600e+03
    RSA                     1.002310e+03
    Czech Republic          8.267400e+02
    Bahrain                 5.484000e+02
    Saudi Arabia            1.459200e+02
    Name: 총 주문 가격, dtype: float64




```python
plot = rev_by_countries.plot(kind='bar', color=COLORS[-1], figsize=(20, 10))
plot.set_xlabel('고객 거주 지역', fontsize=25)
plot.set_ylabel('매출', fontsize=25)
plot.set_title('국가별 매출', fontsize=23)
plot.set_xticklabels(labels=rev_by_countries.index, rotation=75, fontsize=13)
```




    [Text(0, 0, 'United Kingdom'),
     Text(1, 0, 'Netherlands'),
     Text(2, 0, 'EIRE'),
     Text(3, 0, 'Germany'),
     Text(4, 0, 'France'),
     Text(5, 0, 'Australia'),
     Text(6, 0, 'Spain'),
     Text(7, 0, 'Switzerland'),
     Text(8, 0, 'Belgium'),
     Text(9, 0, 'Sweden'),
     Text(10, 0, 'Japan'),
     Text(11, 0, 'Norway'),
     Text(12, 0, 'Portugal'),
     Text(13, 0, 'Finland'),
     Text(14, 0, 'Singapore'),
     Text(15, 0, 'Channel Islands'),
     Text(16, 0, 'Denmark'),
     Text(17, 0, 'Italy'),
     Text(18, 0, 'Cyprus'),
     Text(19, 0, 'Austria'),
     Text(20, 0, 'Poland'),
     Text(21, 0, 'Israel'),
     Text(22, 0, 'Greece'),
     Text(23, 0, 'Iceland'),
     Text(24, 0, 'Canada'),
     Text(25, 0, 'USA'),
     Text(26, 0, 'Malta'),
     Text(27, 0, 'Unspecified'),
     Text(28, 0, 'United Arab Emirates'),
     Text(29, 0, 'Lebanon'),
     Text(30, 0, 'Lithuania'),
     Text(31, 0, 'European Community'),
     Text(32, 0, 'Brazil'),
     Text(33, 0, 'RSA'),
     Text(34, 0, 'Czech Republic'),
     Text(35, 0, 'Bahrain'),
     Text(36, 0, 'Saudi Arabia')]





![output_15_1](./img/salesitem2/output_15_1.png)




```python
rev_by_countries / total_revenue #비율 확인할 수 있다.
```




    고객 거주 지역
    United Kingdom          0.820116
    Netherlands             0.032032
    EIRE                    0.029798
    Germany                 0.025682
    France                  0.023456
    Australia               0.015544
    Spain                   0.006910
    Switzerland             0.006334
    Belgium                 0.004623
    Sweden                  0.004307
    Japan                   0.004199
    Norway                  0.004058
    Portugal                0.003752
    Finland                 0.002530
    Singapore               0.002388
    Channel Islands         0.002295
    Denmark                 0.002127
    Italy                   0.001962
    Cyprus                  0.001525
    Austria                 0.001144
    Poland                  0.000823
    Israel                  0.000810
    Greece                  0.000534
    Iceland                 0.000484
    Canada                  0.000411
    USA                     0.000402
    Malta                   0.000306
    Unspecified             0.000299
    United Arab Emirates    0.000213
    Lebanon                 0.000190
    Lithuania               0.000186
    European Community      0.000146
    Brazil                  0.000128
    RSA                     0.000112
    Czech Republic          0.000093
    Bahrain                 0.000062
    Saudi Arabia            0.000016
    Name: 총 주문 가격, dtype: float64



#### 그래프 유틸 함수


```python
def plot_bar(df, xlabel, ylabel, title, color=COLORS[-7], figsize=(20, 10), rotation=45):
    plot = df.plot(kind='bar', color=color, figsize=figsize)
    plot.set_xlabel(xlabel, fontsize=25)
    plot.set_ylabel(ylabel, fontsize=25)
    plot.set_title(title, fontsize=30)
    plot.set_xticklabels(labels=df.index, rotation=75 , fontsize=13)
                   
plot_bar(rev_by_countries, '고객 거주 지역', '매출', '국가별 매출')
```



![output_18_0](./img/salesitem2/output_18_0.png)




```python
grouped = retail.groupby('고객 거주 지역')
grouped.first()
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
      <th>주문 번호</th>
      <th>아이템 아이디</th>
      <th>상품 설명</th>
      <th>주문 수량</th>
      <th>주문 시각</th>
      <th>상품 가격</th>
      <th>고객 아이디</th>
      <th>총 주문 가격</th>
    </tr>
    <tr>
      <th>고객 거주 지역</th>
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
      <th>Australia</th>
      <td>197</td>
      <td>536389</td>
      <td>22941</td>
      <td>CHRISTMAS LIGHTS 10 REINDEER</td>
      <td>6</td>
      <td>2010-12-01 10:03:00</td>
      <td>8.50</td>
      <td>12431</td>
      <td>51.00</td>
    </tr>
    <tr>
      <th>Austria</th>
      <td>34293</td>
      <td>539330</td>
      <td>37449</td>
      <td>CERAMIC CAKE STAND + HANGING CAKES</td>
      <td>8</td>
      <td>2010-12-17 09:38:00</td>
      <td>8.50</td>
      <td>12370</td>
      <td>68.00</td>
    </tr>
    <tr>
      <th>Bahrain</th>
      <td>181140</td>
      <td>552449</td>
      <td>22693</td>
      <td>GROW A FLYTRAP OR SUNFLOWER IN TIN</td>
      <td>24</td>
      <td>2011-05-09 13:49:00</td>
      <td>1.25</td>
      <td>12355</td>
      <td>30.00</td>
    </tr>
    <tr>
      <th>Belgium</th>
      <td>7279</td>
      <td>537026</td>
      <td>84375</td>
      <td>SET OF 20 KIDS COOKIE CUTTERS</td>
      <td>12</td>
      <td>2010-12-03 16:35:00</td>
      <td>2.10</td>
      <td>12395</td>
      <td>25.20</td>
    </tr>
    <tr>
      <th>Brazil</th>
      <td>157299</td>
      <td>550201</td>
      <td>22423</td>
      <td>REGENCY CAKESTAND 3 TIER</td>
      <td>16</td>
      <td>2011-04-15 10:25:00</td>
      <td>10.95</td>
      <td>12769</td>
      <td>175.20</td>
    </tr>
    <tr>
      <th>Canada</th>
      <td>119191</td>
      <td>546533</td>
      <td>20886</td>
      <td>BOX OF 9 PEBBLE CANDLES</td>
      <td>12</td>
      <td>2011-03-14 13:53:00</td>
      <td>1.95</td>
      <td>15388</td>
      <td>23.40</td>
    </tr>
    <tr>
      <th>Channel Islands</th>
      <td>20000</td>
      <td>538002</td>
      <td>22690</td>
      <td>DOORMAT HOME SWEET HOME BLUE</td>
      <td>2</td>
      <td>2010-12-09 11:48:00</td>
      <td>7.95</td>
      <td>14932</td>
      <td>15.90</td>
    </tr>
    <tr>
      <th>Cyprus</th>
      <td>29732</td>
      <td>538826</td>
      <td>85123A</td>
      <td>WHITE HANGING HEART T-LIGHT HOLDER</td>
      <td>64</td>
      <td>2010-12-14 12:58:00</td>
      <td>2.55</td>
      <td>12370</td>
      <td>163.20</td>
    </tr>
    <tr>
      <th>Czech Republic</th>
      <td>103598</td>
      <td>545072</td>
      <td>22930</td>
      <td>BAKING MOULD HEART MILK CHOCOLATE</td>
      <td>18</td>
      <td>2011-02-28 08:43:00</td>
      <td>2.55</td>
      <td>12781</td>
      <td>45.90</td>
    </tr>
    <tr>
      <th>Denmark</th>
      <td>20017</td>
      <td>538003</td>
      <td>22847</td>
      <td>BREAD BIN DINER STYLE IVORY</td>
      <td>8</td>
      <td>2010-12-09 12:05:00</td>
      <td>14.95</td>
      <td>12429</td>
      <td>119.60</td>
    </tr>
    <tr>
      <th>EIRE</th>
      <td>1404</td>
      <td>536540</td>
      <td>22968</td>
      <td>ROSE COTTAGE KEEPSAKE BOX</td>
      <td>4</td>
      <td>2010-12-01 14:05:00</td>
      <td>9.95</td>
      <td>14911</td>
      <td>39.80</td>
    </tr>
    <tr>
      <th>European Community</th>
      <td>168149</td>
      <td>551013</td>
      <td>22839</td>
      <td>3 TIER CAKE TIN GREEN AND CREAM</td>
      <td>1</td>
      <td>2011-04-26 10:54:00</td>
      <td>14.95</td>
      <td>15108</td>
      <td>14.95</td>
    </tr>
    <tr>
      <th>Finland</th>
      <td>34083</td>
      <td>539318</td>
      <td>84992</td>
      <td>72 SWEETHEART FAIRY CAKE CASES</td>
      <td>72</td>
      <td>2010-12-16 19:09:00</td>
      <td>0.55</td>
      <td>12348</td>
      <td>39.60</td>
    </tr>
    <tr>
      <th>France</th>
      <td>26</td>
      <td>536370</td>
      <td>22728</td>
      <td>ALARM CLOCK BAKELIKE PINK</td>
      <td>24</td>
      <td>2010-12-01 08:45:00</td>
      <td>3.75</td>
      <td>12583</td>
      <td>90.00</td>
    </tr>
    <tr>
      <th>Germany</th>
      <td>1109</td>
      <td>536527</td>
      <td>22809</td>
      <td>SET OF 6 T-LIGHTS SANTA</td>
      <td>6</td>
      <td>2010-12-01 13:04:00</td>
      <td>2.95</td>
      <td>12662</td>
      <td>17.70</td>
    </tr>
    <tr>
      <th>Greece</th>
      <td>69007</td>
      <td>541932</td>
      <td>22699</td>
      <td>ROSES REGENCY TEACUP AND SAUCER</td>
      <td>24</td>
      <td>2011-01-24 11:39:00</td>
      <td>2.55</td>
      <td>14439</td>
      <td>61.20</td>
    </tr>
    <tr>
      <th>Iceland</th>
      <td>14938</td>
      <td>537626</td>
      <td>85116</td>
      <td>BLACK CANDELABRA T-LIGHT HOLDER</td>
      <td>12</td>
      <td>2010-12-07 14:57:00</td>
      <td>2.10</td>
      <td>12347</td>
      <td>25.20</td>
    </tr>
    <tr>
      <th>Israel</th>
      <td>91580</td>
      <td>544108</td>
      <td>22245</td>
      <td>HOOK, 1 HANGER ,MAGIC GARDEN</td>
      <td>24</td>
      <td>2011-02-16 10:53:00</td>
      <td>0.85</td>
      <td>12653</td>
      <td>20.40</td>
    </tr>
    <tr>
      <th>Italy</th>
      <td>7214</td>
      <td>537022</td>
      <td>22791</td>
      <td>T-LIGHT GLASS FLUTED ANTIQUE</td>
      <td>12</td>
      <td>2010-12-03 15:45:00</td>
      <td>1.25</td>
      <td>12725</td>
      <td>15.00</td>
    </tr>
    <tr>
      <th>Japan</th>
      <td>9783</td>
      <td>537218</td>
      <td>85016</td>
      <td>SET OF 6 VINTAGE NOTELETS KIT</td>
      <td>6</td>
      <td>2010-12-05 15:46:00</td>
      <td>2.55</td>
      <td>12763</td>
      <td>15.30</td>
    </tr>
    <tr>
      <th>Lebanon</th>
      <td>72985</td>
      <td>542276</td>
      <td>82551</td>
      <td>LAUNDRY 15C METAL SIGN</td>
      <td>12</td>
      <td>2011-01-27 10:19:00</td>
      <td>1.45</td>
      <td>12764</td>
      <td>17.40</td>
    </tr>
    <tr>
      <th>Lithuania</th>
      <td>7986</td>
      <td>537081</td>
      <td>22409</td>
      <td>MONEY BOX BISCUITS DESIGN</td>
      <td>12</td>
      <td>2010-12-05 12:00:00</td>
      <td>1.25</td>
      <td>15332</td>
      <td>15.00</td>
    </tr>
    <tr>
      <th>Malta</th>
      <td>217684</td>
      <td>555931</td>
      <td>22784</td>
      <td>LANTERN CREAM GAZEBO</td>
      <td>3</td>
      <td>2011-06-08 08:31:00</td>
      <td>4.95</td>
      <td>17828</td>
      <td>14.85</td>
    </tr>
    <tr>
      <th>Netherlands</th>
      <td>385</td>
      <td>536403</td>
      <td>22867</td>
      <td>HAND WARMER BIRD DESIGN</td>
      <td>96</td>
      <td>2010-12-01 11:27:00</td>
      <td>1.85</td>
      <td>12791</td>
      <td>177.60</td>
    </tr>
    <tr>
      <th>Norway</th>
      <td>1236</td>
      <td>536532</td>
      <td>84692</td>
      <td>BOX OF 24 COCKTAIL PARASOLS</td>
      <td>50</td>
      <td>2010-12-01 13:24:00</td>
      <td>0.42</td>
      <td>12433</td>
      <td>21.00</td>
    </tr>
    <tr>
      <th>Poland</th>
      <td>6608</td>
      <td>536971</td>
      <td>21733</td>
      <td>RED HANGING HEART T-LIGHT HOLDER</td>
      <td>32</td>
      <td>2010-12-03 13:40:00</td>
      <td>2.55</td>
      <td>12779</td>
      <td>81.60</td>
    </tr>
    <tr>
      <th>Portugal</th>
      <td>7134</td>
      <td>536990</td>
      <td>21992</td>
      <td>VINTAGE PAISLEY STATIONERY SET</td>
      <td>6</td>
      <td>2010-12-03 15:14:00</td>
      <td>2.95</td>
      <td>12793</td>
      <td>17.70</td>
    </tr>
    <tr>
      <th>RSA</th>
      <td>395472</td>
      <td>571035</td>
      <td>21238</td>
      <td>RED RETROSPOT CUP</td>
      <td>8</td>
      <td>2011-10-13 12:50:00</td>
      <td>0.85</td>
      <td>12446</td>
      <td>6.80</td>
    </tr>
    <tr>
      <th>Saudi Arabia</th>
      <td>100810</td>
      <td>544838</td>
      <td>22915</td>
      <td>ASSORTED BOTTLE TOP  MAGNETS</td>
      <td>12</td>
      <td>2011-02-24 10:34:00</td>
      <td>0.42</td>
      <td>12565</td>
      <td>5.04</td>
    </tr>
    <tr>
      <th>Singapore</th>
      <td>70758</td>
      <td>542102</td>
      <td>21519</td>
      <td>GIN &amp; TONIC DIET GREETING CARD</td>
      <td>72</td>
      <td>2011-01-25 13:26:00</td>
      <td>0.36</td>
      <td>12744</td>
      <td>25.92</td>
    </tr>
    <tr>
      <th>Spain</th>
      <td>6421</td>
      <td>536944</td>
      <td>22383</td>
      <td>LUNCH BAG SUKI  DESIGN</td>
      <td>70</td>
      <td>2010-12-03 12:20:00</td>
      <td>1.65</td>
      <td>12557</td>
      <td>115.50</td>
    </tr>
    <tr>
      <th>Sweden</th>
      <td>30079</td>
      <td>538848</td>
      <td>85232B</td>
      <td>SET OF 3 BABUSHKA STACKING TINS</td>
      <td>240</td>
      <td>2010-12-14 13:28:00</td>
      <td>4.95</td>
      <td>17404</td>
      <td>1188.00</td>
    </tr>
    <tr>
      <th>Switzerland</th>
      <td>5320</td>
      <td>536858</td>
      <td>22326</td>
      <td>ROUND SNACK BOXES SET OF4 WOODLAND</td>
      <td>30</td>
      <td>2010-12-03 10:36:00</td>
      <td>2.95</td>
      <td>13520</td>
      <td>88.50</td>
    </tr>
    <tr>
      <th>USA</th>
      <td>164464</td>
      <td>550644</td>
      <td>22722</td>
      <td>SET OF 6 SPICE TINS PANTRY DESIGN</td>
      <td>7</td>
      <td>2011-04-19 16:19:00</td>
      <td>3.95</td>
      <td>12733</td>
      <td>27.65</td>
    </tr>
    <tr>
      <th>United Arab Emirates</th>
      <td>89570</td>
      <td>543911</td>
      <td>21485</td>
      <td>RETROSPOT HEART HOT WATER BOTTLE</td>
      <td>6</td>
      <td>2011-02-14 12:46:00</td>
      <td>4.95</td>
      <td>17829</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>United Kingdom</th>
      <td>0</td>
      <td>536365</td>
      <td>85123A</td>
      <td>WHITE HANGING HEART T-LIGHT HOLDER</td>
      <td>6</td>
      <td>2010-12-01 08:26:00</td>
      <td>2.55</td>
      <td>17850</td>
      <td>15.30</td>
    </tr>
    <tr>
      <th>Unspecified</th>
      <td>152712</td>
      <td>549687</td>
      <td>20685</td>
      <td>DOORMAT RED RETROSPOT</td>
      <td>2</td>
      <td>2011-04-11 13:29:00</td>
      <td>7.95</td>
      <td>12363</td>
      <td>15.90</td>
    </tr>
  </tbody>
</table>
</div>




```python
grouped_sr = grouped.size()
grouped_sr
```




    고객 거주 지역
    Australia                 1182
    Austria                    398
    Bahrain                     17
    Belgium                   2031
    Brazil                      32
    Canada                     151
    Channel Islands            748
    Cyprus                     614
    Czech Republic              25
    Denmark                    380
    EIRE                      7236
    European Community          60
    Finland                    685
    France                    8341
    Germany                   9040
    Greece                     145
    Iceland                    182
    Israel                     248
    Italy                      758
    Japan                      321
    Lebanon                     45
    Lithuania                   35
    Malta                      112
    Netherlands               2359
    Norway                    1071
    Poland                     330
    Portugal                  1462
    RSA                         57
    Saudi Arabia                 9
    Singapore                  222
    Spain                     2484
    Sweden                     451
    Switzerland               1841
    USA                        179
    United Arab Emirates        68
    United Kingdom          354321
    Unspecified                244
    dtype: int64




```python
grouped_sr.index 
```




    Index(['Australia', 'Austria', 'Bahrain', 'Belgium', 'Brazil', 'Canada',
           'Channel Islands', 'Cyprus', 'Czech Republic', 'Denmark', 'EIRE',
           'European Community', 'Finland', 'France', 'Germany', 'Greece',
           'Iceland', 'Israel', 'Italy', 'Japan', 'Lebanon', 'Lithuania', 'Malta',
           'Netherlands', 'Norway', 'Poland', 'Portugal', 'RSA', 'Saudi Arabia',
           'Singapore', 'Spain', 'Sweden', 'Switzerland', 'USA',
           'United Arab Emirates', 'United Kingdom', 'Unspecified'],
          dtype='object', name='고객 거주 지역')




```python
grouped_sr.plot(kind='pie', 
                figsize=(15,25),
               textprops={'size':10}
               )

plt.title('국가별 매출', size=50)

plt.legend(grouped_sr.index, loc='best')
plt.show()
```



![output_22_0](./img/salesitem2/output_22_0.png)



- grouped_sr.index 가 많다보니 글씨가 잘 안보인다.


```python
grouped_sr.plot.pie(figsize=(17,25),
                    shadow = True,
                    textprops = {'fontsize' : 14},
                    autopct='%1.0f%%') # 파이 조각의 전체 대비 백분율 

plt.title('국가별 매출', size=50)

plt.legend(grouped_sr.index, loc='best')
plt.show()
```



![output_24_0](./img/salesitem2/output_24_0.png)



#### 월별 매출


```python
retail['주문 시각'].sort_values(ascending=False)
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
    Name: 주문 시각, Length: 397884, dtype: datetime64[ns]




```python
def extract_month(date):
    month = str(date.month)
    if date.month < 10:
        month = '0' + month
    return str(date.year) + month 
```


```python
rev_by_month = retail.set_index('주문 시각').groupby(extract_month).sum()['총 주문 가격']
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
    Name: 총 주문 가격, dtype: float64




```python
plot_bar(rev_by_month, '월별', '매출', '월별 매출')
```



![output_29_0](./img/salesitem2/output_29_0.png)



#### 요일별 매출


```python
rev_by_dow = retail.set_index('주문 시각').groupby(lambda date:date.dayofweek).sum()['총 주문 가격']
rev_by_dow  ## dayofweek - [Monday 0 ~ Sunday 6]
```




    0    1367146.411
    1    1700634.631
    2    1588336.170
    3    1976859.070
    4    1485917.401
    6     792514.221
    Name: 총 주문 가격, dtype: float64




```python
DAY_OF_WEEK = np.array(['월요일', '화요일', '수요일', '목요일', '금요일', '토요일', '일요일'])
rev_by_dow.index = DAY_OF_WEEK[rev_by_dow.index]
plot_bar(rev_by_dow, '요일', '매출', '요일 별 매출')
```



![output_32_0](./img/salesitem2/output_32_0.png)



#### 시간별 매출


```python
rev_by_hour = retail.set_index('주문 시각').groupby(lambda date:date.hour).sum()['총 주문 가격']
plot_bar(rev_by_hour, '시간', '매출', '시간별 매출')
```



![output_34_0](./img/salesitem2/output_34_0.png)



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
top_selling = retail.groupby('아이템 아이디').sum()['주문 수량'].sort_values(ascending=False)[:3]
top_selling
```




    아이템 아이디
    23843    80995
    23166    77916
    84077    54415
    Name: 주문 수량, dtype: int32




```python
top_revenue = retail.groupby('아이템 아이디').sum()['총 주문 가격'].sort_values(ascending=False)[:10]
top_revenue
```




    아이템 아이디
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
    Name: 총 주문 가격, dtype: float64



#### top 3 아이템의 월별 판매량 추이


```python
retail.set_index('주문 시각').groupby(['아이템 아이디', extract_month]).sum()[['주문 수량', '총 주문 가격']].loc[top_selling.index]
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
      <th>주문 수량</th>
      <th>총 주문 가격</th>
    </tr>
    <tr>
      <th>아이템 아이디</th>
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
monthly_top3 = retail.set_index('주문 시각').groupby(['아이템 아이디', extract_month]).sum()[['주문 수량', '총 주문 가격']].loc[top_selling.index]
```


```python
plot_bar(monthly_top3['총 주문 가격'], '아이템/월별', '매출', '탑 3 아이템 매출')
```



![output_42_0](./img/salesitem2/output_42_0.png)
    

