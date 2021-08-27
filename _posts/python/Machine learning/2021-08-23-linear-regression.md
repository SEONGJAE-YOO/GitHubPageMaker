---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 단순선형회귀분석 실습 - 단순선형회귀 적합 및 해석
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


```python
import os
import pandas as pd 
import numpy as np
import statsmodels.api as sm
```


```python
# 현재경로 확인
os.getcwd()
```




    'C:\\Users\\MyCom\\jupyter-tutorial\\머신러닝과 데이터분석 A-Z 올인원 패키지 Online\\Machine learning의 개념과 종류'




```python
# 데이터 불러오기
boston = pd.read_csv("Boston_house.csv")

```


```python
boston.head()
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
      <th>AGE</th>
      <th>B</th>
      <th>RM</th>
      <th>CRIM</th>
      <th>DIS</th>
      <th>INDUS</th>
      <th>LSTAT</th>
      <th>NOX</th>
      <th>PTRATIO</th>
      <th>RAD</th>
      <th>ZN</th>
      <th>TAX</th>
      <th>CHAS</th>
      <th>Target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>65.2</td>
      <td>396.90</td>
      <td>6.575</td>
      <td>0.00632</td>
      <td>4.0900</td>
      <td>2.31</td>
      <td>4.98</td>
      <td>0.538</td>
      <td>15.3</td>
      <td>1</td>
      <td>18.0</td>
      <td>296</td>
      <td>0</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>78.9</td>
      <td>396.90</td>
      <td>6.421</td>
      <td>0.02731</td>
      <td>4.9671</td>
      <td>7.07</td>
      <td>9.14</td>
      <td>0.469</td>
      <td>17.8</td>
      <td>2</td>
      <td>0.0</td>
      <td>242</td>
      <td>0</td>
      <td>21.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>61.1</td>
      <td>392.83</td>
      <td>7.185</td>
      <td>0.02729</td>
      <td>4.9671</td>
      <td>7.07</td>
      <td>4.03</td>
      <td>0.469</td>
      <td>17.8</td>
      <td>2</td>
      <td>0.0</td>
      <td>242</td>
      <td>0</td>
      <td>34.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>45.8</td>
      <td>394.63</td>
      <td>6.998</td>
      <td>0.03237</td>
      <td>6.0622</td>
      <td>2.18</td>
      <td>2.94</td>
      <td>0.458</td>
      <td>18.7</td>
      <td>3</td>
      <td>0.0</td>
      <td>222</td>
      <td>0</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>54.2</td>
      <td>396.90</td>
      <td>7.147</td>
      <td>0.06905</td>
      <td>6.0622</td>
      <td>2.18</td>
      <td>5.33</td>
      <td>0.458</td>
      <td>18.7</td>
      <td>3</td>
      <td>0.0</td>
      <td>222</td>
      <td>0</td>
      <td>36.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# target 제외한 데이터만 뽑기
boston_data = boston.drop(['Target'],axis=1)
# boston_data
```


```python
boston_data.describe()
# data 통계 뽑아보기
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
      <th>AGE</th>
      <th>B</th>
      <th>RM</th>
      <th>CRIM</th>
      <th>DIS</th>
      <th>INDUS</th>
      <th>LSTAT</th>
      <th>NOX</th>
      <th>PTRATIO</th>
      <th>RAD</th>
      <th>ZN</th>
      <th>TAX</th>
      <th>CHAS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>68.574901</td>
      <td>356.674032</td>
      <td>6.284634</td>
      <td>3.613524</td>
      <td>3.795043</td>
      <td>11.136779</td>
      <td>12.653063</td>
      <td>0.554695</td>
      <td>18.455534</td>
      <td>9.549407</td>
      <td>11.363636</td>
      <td>408.237154</td>
      <td>0.069170</td>
    </tr>
    <tr>
      <th>std</th>
      <td>28.148861</td>
      <td>91.294864</td>
      <td>0.702617</td>
      <td>8.601545</td>
      <td>2.105710</td>
      <td>6.860353</td>
      <td>7.141062</td>
      <td>0.115878</td>
      <td>2.164946</td>
      <td>8.707259</td>
      <td>23.322453</td>
      <td>168.537116</td>
      <td>0.253994</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2.900000</td>
      <td>0.320000</td>
      <td>3.561000</td>
      <td>0.006320</td>
      <td>1.129600</td>
      <td>0.460000</td>
      <td>1.730000</td>
      <td>0.385000</td>
      <td>12.600000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>187.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>45.025000</td>
      <td>375.377500</td>
      <td>5.885500</td>
      <td>0.082045</td>
      <td>2.100175</td>
      <td>5.190000</td>
      <td>6.950000</td>
      <td>0.449000</td>
      <td>17.400000</td>
      <td>4.000000</td>
      <td>0.000000</td>
      <td>279.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>77.500000</td>
      <td>391.440000</td>
      <td>6.208500</td>
      <td>0.256510</td>
      <td>3.207450</td>
      <td>9.690000</td>
      <td>11.360000</td>
      <td>0.538000</td>
      <td>19.050000</td>
      <td>5.000000</td>
      <td>0.000000</td>
      <td>330.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>94.075000</td>
      <td>396.225000</td>
      <td>6.623500</td>
      <td>3.677083</td>
      <td>5.188425</td>
      <td>18.100000</td>
      <td>16.955000</td>
      <td>0.624000</td>
      <td>20.200000</td>
      <td>24.000000</td>
      <td>12.500000</td>
      <td>666.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>100.000000</td>
      <td>396.900000</td>
      <td>8.780000</td>
      <td>88.976200</td>
      <td>12.126500</td>
      <td>27.740000</td>
      <td>37.970000</td>
      <td>0.871000</td>
      <td>22.000000</td>
      <td>24.000000</td>
      <td>100.000000</td>
      <td>711.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''
타겟 데이터
1978 보스턴 주택 가격
506개 타운의 주택 가격 중앙값 (단위 1,000 달러)

특징 데이터
CRIM: 범죄율
INDUS: 비소매상업지역 면적 비율
NOX: 일산화질소 농도
RM: 주택당 방 수
LSTAT: 인구 중 하위 계층 비율
B: 인구 중 흑인 비율
PTRATIO: 학생/교사 비율
ZN: 25,000 평방피트를 초과 거주지역 비율
CHAS: 찰스강의 경계에 위치한 경우는 1, 아니면 0
AGE: 1940년 이전에 건축된 주택의 비율
RAD: 방사형 고속도로까지의 거리
DIS: 직업센터의 거리
TAX: 재산세율'''
```




    '\n타겟 데이터\n1978 보스턴 주택 가격\n506개 타운의 주택 가격 중앙값 (단위 1,000 달러)\n\n특징 데이터\nCRIM: 범죄율\nINDUS: 비소매상업지역 면적 비율\nNOX: 일산화질소 농도\nRM: 주택당 방 수\nLSTAT: 인구 중 하위 계층 비율\nB: 인구 중 흑인 비율\nPTRATIO: 학생/교사 비율\nZN: 25,000 평방피트를 초과 거주지역 비율\nCHAS: 찰스강의 경계에 위치한 경우는 1, 아니면 0\nAGE: 1940년 이전에 건축된 주택의 비율\nRAD: 방사형 고속도로까지의 거리\nDIS: 직업센터의 거리\nTAX: 재산세율'



# crim/rm/lstat 세게의 변수로 각각 단순 선형 회귀 분석하기


```python
target = boston[['Target']]
# boston_target
crim=boston[['CRIM']]
rm=boston[['RM']]
lstat=boston['LSTAT']
```

## target ~ crim 선형회귀분석


```python
crim1 = sm.add_constant(crim, has_constant='add')
```


```python
model1 = sm.OLS(target,crim1)
fitted_model1=model1.fit()

```


```python
fitted_model1.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>         <td>Target</td>      <th>  R-squared:         </th> <td>   0.151</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.149</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   89.49</td>
</tr>
<tr>
  <th>Date:</th>             <td>Wed, 12 May 2021</td> <th>  Prob (F-statistic):</th> <td>1.17e-19</td>
</tr>
<tr>
  <th>Time:</th>                 <td>15:52:25</td>     <th>  Log-Likelihood:    </th> <td> -1798.9</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>   506</td>      <th>  AIC:               </th> <td>   3602.</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>   504</td>      <th>  BIC:               </th> <td>   3610.</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
    <td></td>       <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th> <td>   24.0331</td> <td>    0.409</td> <td>   58.740</td> <td> 0.000</td> <td>   23.229</td> <td>   24.837</td>
</tr>
<tr>
  <th>CRIM</th>  <td>   -0.4152</td> <td>    0.044</td> <td>   -9.460</td> <td> 0.000</td> <td>   -0.501</td> <td>   -0.329</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>139.832</td> <th>  Durbin-Watson:     </th> <td>   0.713</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td> 295.404</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 1.490</td>  <th>  Prob(JB):          </th> <td>7.14e-65</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 5.264</td>  <th>  Cond. No.          </th> <td>    10.1</td>
</tr>
</table><br/><br/>Notes:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.



## y_hat=beta0 + beta1 * X 계산해보기



```python
(np.dot(crim1,fitted_model1.params))
```




    array([ 24.03048217,  24.02176733,  24.02177563,  24.01966646,
            24.00443729,  24.02071274,  23.99644902,  23.97309042,
            23.94540138,  23.96250722,  23.93973403,  23.98433377,
            23.99416963,  23.77163594,  23.76823138,  23.77261995,
            23.59552468,  23.70751396,  23.69982879,  23.73176107,
            23.51337514,  23.67934745,  23.52139661,  23.62271965,
            23.72160552,  23.68412214,  23.75413567,  23.63627976,
            23.71216824,  23.61689868,  23.56360486,  23.4706396 ,
            23.45682622,  23.55492323,  23.36347899,  24.00646341,
            23.99265003,  23.99983283,  23.96042712,  24.02163447,
            24.01915993,  23.98019433,  23.97435675,  23.96694145,
            23.98216648,  23.96193426,  23.95490093,  23.9379155 ,
            23.92770182,  23.94185981,  23.99626634,  24.01509937,
            24.01085198,  24.01242555,  24.02745959,  24.02766303,
            24.02457401,  24.02716065,  23.96898004,  23.99022532,
            23.97110996,  23.96181385,  23.98732314,  23.9805846 ,
            24.02500581,  24.01822575,  24.01492499,  24.00907081,
            23.97683128,  23.97989539,  23.99646148,  23.96719057,
            23.99505814,  23.95198215,  24.00032275,  23.99361327,
            23.99095191,  23.99695556,  24.00966453,  23.99828417,
            24.0160294 ,  24.01458038,  24.01791436,  24.01836277,
            24.0121017 ,  24.00929501,  24.0115661 ,  24.00341592,
            24.0096064 ,  24.01109279,  24.01365866,  24.01678089,
            24.01565573,  24.02116945,  24.0152779 ,  23.98243635,
            23.98534268,  23.98293873,  23.99911455,  24.00462412,
            23.97138399,  23.98564162,  23.93812725,  23.94524776,
            23.97514561,  23.97804364,  23.9620256 ,  23.97864567,
            23.97995351,  23.92364956,  23.98829469,  23.99123839,
            23.98191736,  23.94088411,  23.97402045,  23.96196747,
            23.97847544,  23.97042075,  23.97889063,  23.97300323,
            24.0044622 ,  24.00335779,  23.99449763,  23.97066986,
            23.99221408,  23.96293071,  23.87228222,  23.92550961,
            23.8979908 ,  23.66721974,  23.89191657,  23.53780908,
            23.78812315,  23.89616812,  23.62780988,  23.80152134,
            23.89914918,  23.88682218,  23.92939164,  23.80702676,
            23.91232732,  23.35691068,  22.6542385 ,  22.33190553,
            22.87898515,  23.04522734,  23.13835037,  23.04967818,
            23.06530179,  22.89798841,  23.34530196,  23.41184866,
            23.56536111,  23.14078753,  23.4460894 ,  22.56540439,
            23.01726842,  23.52508765,  23.47557206,  23.44145172,
            23.50437796,  23.42553333,  23.2717427 ,  23.40242384,
            23.1021001 ,  22.8190898 ,  23.19849483,  23.28564742,
            23.07800246,  23.01608513,  23.53179713,  23.07239739,
            23.9753366 ,  23.99500001,  23.99803505,  24.00543789,
            24.00395151,  24.0105821 ,  24.00552924,  24.00910818,
            24.00575344,  24.00450787,  23.9953114 ,  23.99155393,
            23.99861217,  24.00799962,  24.00984721,  24.00040994,
            23.98087939,  23.99835475,  23.99545672,  24.00441237,
            23.99713409,  24.02402596,  24.02713159,  24.0273724 ,
            24.01645289,  24.0137334 ,  24.0174618 ,  24.02002768,
            24.02572409,  24.01880287,  24.02406748,  24.018533  ,
            24.024765  ,  23.97646592,  23.93774112,  23.92848238,
            23.97669427,  23.85220362,  23.96067208,  23.87708597,
            23.942931  ,  23.97476364,  23.91288783,  23.9508902 ,
            24.0141735 ,  24.00398888,  23.98714876,  23.98567068,
            23.88443069,  23.86382895,  23.77421012,  23.77788871,
            23.90218422,  23.81432996,  23.87444536,  23.86189001,
            23.90930059,  23.84968341,  23.81014899,  23.84088968,
            23.79425136,  23.89548305,  23.8471383 ,  23.89590655,
            23.81696642,  23.82059933,  23.99887789,  23.99469277,
            23.98606927,  23.98904618,  23.99038309,  23.98014035,
            23.94754376,  23.95366782,  23.89201206,  23.95149222,
            23.96485304,  23.95391693,  23.97485498,  23.94421809,
            23.99897338,  23.87992587,  24.01309815,  24.01837522,
            24.02672055,  23.77920071,  23.75762327,  23.76047148,
            23.80885775,  23.81134474,  23.8171491 ,  23.69046625,
            23.80472246,  23.71688895,  23.70689117,  23.79298503,
            23.80869583,  23.99546918,  23.90889785,  23.96579968,
            23.98552537,  23.94098376,  24.00967283,  23.9932313 ,
            23.9896399 ,  24.00766747,  23.99998229,  23.94575844,
            24.01825067,  24.01772337,  24.00765916,  24.02687417,
            24.02934455,  24.02855569,  24.02494769,  24.01703416,
            24.01404894,  24.01526545,  24.01856621,  24.00036427,
            24.01809705,  23.9987907 ,  23.99906472,  23.97941377,
            24.01080215,  23.97455189,  24.00625997,  24.01001744,
            24.01476722,  24.01842089,  23.99463464,  23.99158715,
            24.01020843,  24.0103579 ,  24.00195445,  24.01262899,
            23.82842567,  23.88803869,  22.9388805 ,  23.70493563,
            23.92445503,  23.92126222,  23.87981792,  23.92783053,
            23.90096356,  23.93129321,  23.86619138,  23.83569565,
            23.96352028,  23.95771177,  23.88731626,  23.91522535,
            23.89148892,  23.95344777,  23.90710838,  23.93303286,
            24.00563303,  24.00518878,  24.01423993,  24.01225117,
            24.01871568,  24.01200205,  24.01758636,  24.01666049,
            24.0188776 ,  24.02048024,  24.01937998,  24.01028316,
            24.00756782,  24.02770455,  24.02273472,  24.02254789,
            24.02044702,  24.0201813 ,  24.00752215,  24.02534212,
            24.02687417,  24.02106981,  24.00731871,  24.00009855,
            24.00302979,  24.02601057,  24.01524884,  23.98885104,
            20.30346852,  22.43474816,  21.87338184,  22.26385169,
            22.14734515,  22.44008751,  22.50594499,  22.2800109 ,
            22.5906189 ,  22.14155324,  22.49816848,  18.4188202 ,
            21.99941285,  21.6789856 ,  21.31827659,  20.19994497,
            20.60062435,  19.42113105,  16.35283338,  15.8915985 ,
            17.68567721,  19.95448863,  14.21460344,  16.61502604,
           -12.90894703,  17.44220963,  20.21874479,  20.71470618,
            15.69405096,  17.05301026,  13.90503757,  14.65100995,
            18.08189329,  20.64858298,  21.14248918,  21.83548327,
            19.22607466,  20.44388587,  18.4862471 ,  20.41399632,
            21.5950881 ,  20.84775806,   8.10981167,  19.91585102,
            13.63420895,  18.12237434,  20.04906067,  13.73568146,
             6.79058608,  -4.16694965,  15.43194134,  19.07112564,
            20.95908303,  18.03846438,   2.80201916,  18.19939214,
            16.22296186,  12.13549661,   5.0397702 ,  16.52455607,
            19.53485167,  13.26282125,  -6.49753724,  19.12875405,
            19.42972549,  21.11739508,  19.03081067,  21.10584033,
            20.38270343,  17.44806381,  18.9481878 ,   8.39625145,
            20.97435373,  20.15568984,  20.50725636,  19.85533704,
            21.35759926,  21.71590017,  18.25639776,  19.3994166 ,
            18.04573021,  17.73168029,  18.35409203,  20.13420789,
            14.87770384,  19.99572118,  21.68048444,  19.89509566,
            18.71771568,  19.60227857,  21.42236064,  19.91240494,
            20.1597587 ,  20.90837999,  21.24397414,  21.77399775,
            21.91971708,  20.60857939,  20.08313949,  22.05996835,
            22.09465335,  20.62830508,  20.81445565,  21.20932651,
            22.03515658,  22.49976281,  21.27004809,  21.61622129,
            20.77829672,  22.71961021,  22.46577118,  22.19701851,
            17.56622696,  18.60445177,  22.22753085,  22.3563976 ,
            22.55142493,  22.10376262,  20.68842049,  21.3787449 ,
            22.0105441 ,  17.79553655,  19.78446406,  18.08189329,
            21.61503384,  21.66312533,  21.65358426,  22.8629422 ,
            23.04554703,  22.50783411,  21.66994691,  22.025383  ,
            23.97047057,  23.95697273,  23.9469708 ,  23.98920395,
            23.98688719,  23.96114955,  23.91703143,  23.95879127,
            23.91286707,  23.92167741,  23.93382587,  23.95927289,
            23.93994578,  24.00710281,  24.01431051,  24.00787921,
            23.98760547,  24.013422  ])




```python
len(np.dot(crim1,fitted_model1.params))
```




    506




```python
pred1=fitted_model1.predict(crim1)
```


```python
pred1-np.dot(crim1,fitted_model1.params)
```




    0      0.0
    1      0.0
    2      0.0
    3      0.0
    4      0.0
    5      0.0
    6      0.0
    7      0.0
    8      0.0
    9      0.0
    10     0.0
    11     0.0
    12     0.0
    13     0.0
    14     0.0
    15     0.0
    16     0.0
    17     0.0
    18     0.0
    19     0.0
    20     0.0
    21     0.0
    22     0.0
    23     0.0
    24     0.0
    25     0.0
    26     0.0
    27     0.0
    28     0.0
    29     0.0
          ... 
    476    0.0
    477    0.0
    478    0.0
    479    0.0
    480    0.0
    481    0.0
    482    0.0
    483    0.0
    484    0.0
    485    0.0
    486    0.0
    487    0.0
    488    0.0
    489    0.0
    490    0.0
    491    0.0
    492    0.0
    493    0.0
    494    0.0
    495    0.0
    496    0.0
    497    0.0
    498    0.0
    499    0.0
    500    0.0
    501    0.0
    502    0.0
    503    0.0
    504    0.0
    505    0.0
    Length: 506, dtype: float64



## 적합시킨 직선 시각화


```python
import matplotlib.pyplot as plt
plt.yticks(fontname = "Arial") #
plt.scatter(crim,target,label="data")
plt.plot(crim,pred1,label="result")
plt.legend()
plt.show()
```



![output_19_0](./img/Linearregression/output_19_0.png)




```python

plt.scatter(target,pred1)
plt.xlabel("real_value")
plt.ylabel("pred_value")
plt.show()
```



![output_20_0](./img/Linearregression/output_20_0.png)




```python
fitted_model1.resid.plot()
plt.xlabel("residual_number")
plt.show()
```



![output_21_0](./img/Linearregression/output_21_0.png)




```python
##잔차의 합계산해보기

sum(fitted_model1.resid)
```




    -2.717825964282383e-13



## 위와 동일하게 rm변수와 lstat 변수로 각각 단순선형회귀분석 적합시켜보기



```python
rm1 = sm.add_constant(rm, has_constant='add')
lstat1 = sm.add_constant(lstat, has_constant='add')
```


```python
model2 = sm.OLS(target,rm1)
fitted_model2=model2.fit()
model3 = sm.OLS(target,lstat1)
fitted_model3=model3.fit()
```


```python
fitted_model2.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>         <td>Target</td>      <th>  R-squared:         </th> <td>   0.484</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.483</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   471.8</td>
</tr>
<tr>
  <th>Date:</th>             <td>Mon, 12 Aug 2019</td> <th>  Prob (F-statistic):</th> <td>2.49e-74</td>
</tr>
<tr>
  <th>Time:</th>                 <td>16:00:59</td>     <th>  Log-Likelihood:    </th> <td> -1673.1</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>   506</td>      <th>  AIC:               </th> <td>   3350.</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>   504</td>      <th>  BIC:               </th> <td>   3359.</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
    <td></td>       <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th> <td>  -34.6706</td> <td>    2.650</td> <td>  -13.084</td> <td> 0.000</td> <td>  -39.877</td> <td>  -29.465</td>
</tr>
<tr>
  <th>RM</th>    <td>    9.1021</td> <td>    0.419</td> <td>   21.722</td> <td> 0.000</td> <td>    8.279</td> <td>    9.925</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>102.585</td> <th>  Durbin-Watson:     </th> <td>   0.684</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td> 612.449</td> 
</tr>
<tr>
  <th>Skew:</th>          <td> 0.726</td>  <th>  Prob(JB):          </th> <td>1.02e-133</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 8.190</td>  <th>  Cond. No.          </th> <td>    58.4</td> 
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.




```python
fitted_model3.summary()
```




<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>         <td>Target</td>      <th>  R-squared:         </th> <td>   0.544</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.543</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   601.6</td>
</tr>
<tr>
  <th>Date:</th>             <td>Mon, 12 Aug 2019</td> <th>  Prob (F-statistic):</th> <td>5.08e-88</td>
</tr>
<tr>
  <th>Time:</th>                 <td>16:04:22</td>     <th>  Log-Likelihood:    </th> <td> -1641.5</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>   506</td>      <th>  AIC:               </th> <td>   3287.</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>   504</td>      <th>  BIC:               </th> <td>   3295.</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     1</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
    <td></td>       <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>const</th> <td>   34.5538</td> <td>    0.563</td> <td>   61.415</td> <td> 0.000</td> <td>   33.448</td> <td>   35.659</td>
</tr>
<tr>
  <th>LSTAT</th> <td>   -0.9500</td> <td>    0.039</td> <td>  -24.528</td> <td> 0.000</td> <td>   -1.026</td> <td>   -0.874</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>137.043</td> <th>  Durbin-Watson:     </th> <td>   0.892</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td> 291.373</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 1.453</td>  <th>  Prob(JB):          </th> <td>5.36e-64</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 5.319</td>  <th>  Cond. No.          </th> <td>    29.7</td>
</tr>
</table><br/><br/>Warnings:<br/>[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.




```python
pred2=fitted_model2.predict(rm1)
pred3=fitted_model3.predict(lstat1)

```


```python
import matplotlib.pyplot as plt
plt.scatter(rm,target,label="data")
plt.plot(rm,pred2,label="result")
plt.legend()
plt.show()
```



![output_29_0](./img/Linearregression/output_29_0.png)




```python
import matplotlib.pyplot as plt
plt.scatter(lstat,target,label="data")
plt.plot(lstat,pred3,label="result")
plt.legend()
plt.show()
```



![output_30_0](./img/Linearregression/output_30_0.png)




```python
fitted_model2.resid.plot()
plt.xlabel("residual_number")
plt.show()
```



![output_31_0](./img/Linearregression/output_31_0.png)




```python
fitted_model3.resid.plot()
plt.xlabel("residual_number")
plt.show()
```



![output_32_0](./img/Linearregression/output_32_0.png)



  
```python
fitted_model1.resid.plot(label="crim")
fitted_model2.resid.plot(label="rm")
fitted_model3.resid.plot(label="lstat")
plt.legend()
```
