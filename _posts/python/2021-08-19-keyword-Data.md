---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 키워드 데이터 분석
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap :
changefreq : daily
priority : 1.0
---
{% include python-table-of-contents.html %}

# 패스트캠퍼스 키워드 데이터 분석


```python
import pandas as pd 
from pandas import DataFrame
from pandas import Series
import seaborn as sns
```


```python
import matplotlib.pyplot as plt
```


```python
# matplotlib 한글 폰트 출력코드
# 출처 : 데이터공방( https://kiddwannabe.blog.me)

import matplotlib
from matplotlib import font_manager, rc
import platform

try : 
    if platform.system() == 'Windows':
    # 윈도우인 경우
        font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
        rc('font', family=font_name)
    else:    
    # Mac 인 경우
        rc('font', family='AppleGothic')
except : 
    pass
matplotlib.rcParams['axes.unicode_minus'] = False   
```


```python
df=pd.read_excel('naverreport.xls')
```


```python
df.head()
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
      <th>캠페인보고서(2019.02.01.~2019.04.30.),ftasia</th>
      <th>Unnamed: 1</th>
      <th>Unnamed: 2</th>
      <th>Unnamed: 3</th>
      <th>Unnamed: 4</th>
      <th>Unnamed: 5</th>
      <th>Unnamed: 6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>광고그룹</td>
      <td>키워드</td>
      <td>노출수</td>
      <td>클릭수</td>
      <td>클릭률(%)</td>
      <td>평균클릭비용(VAT포함,원)</td>
      <td>총비용(VAT포함,원)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>올인원 패키지 : 디자인 툴_파워컨텐츠_포토샵</td>
      <td>-</td>
      <td>2319456</td>
      <td>9606</td>
      <td>0.414149</td>
      <td>261.549448</td>
      <td>2512444</td>
    </tr>
    <tr>
      <th>2</th>
      <td>올인원 패키지 : 업무자동화_VBA</td>
      <td>-</td>
      <td>767491</td>
      <td>8058</td>
      <td>1.049915</td>
      <td>295.974808</td>
      <td>2384965</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ㅍAOP 전체_중복키워드_디자인(삭제)</td>
      <td>일러스트</td>
      <td>1137840</td>
      <td>324</td>
      <td>0.028475</td>
      <td>4841.66358</td>
      <td>1568699</td>
    </tr>
    <tr>
      <th>4</th>
      <td>올인원 패키지 : 데이터 분석 입문 온라인_파콘</td>
      <td>-</td>
      <td>694106</td>
      <td>1863.6</td>
      <td>0.268489</td>
      <td>630.593475</td>
      <td>1175174</td>
    </tr>
  </tbody>
</table>
</div>




```python
df=pd.read_excel('naverreport.xls',skiprows=[0])
```


```python
df.head()
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
      <th>광고그룹</th>
      <th>키워드</th>
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>올인원 패키지 : 디자인 툴_파워컨텐츠_포토샵</td>
      <td>-</td>
      <td>2319456</td>
      <td>9606.0</td>
      <td>0.414149</td>
      <td>261.549448</td>
      <td>2512444</td>
    </tr>
    <tr>
      <th>1</th>
      <td>올인원 패키지 : 업무자동화_VBA</td>
      <td>-</td>
      <td>767491</td>
      <td>8058.0</td>
      <td>1.049915</td>
      <td>295.974808</td>
      <td>2384965</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ㅍAOP 전체_중복키워드_디자인(삭제)</td>
      <td>일러스트</td>
      <td>1137840</td>
      <td>324.0</td>
      <td>0.028475</td>
      <td>4841.663580</td>
      <td>1568699</td>
    </tr>
    <tr>
      <th>3</th>
      <td>올인원 패키지 : 데이터 분석 입문 온라인_파콘</td>
      <td>-</td>
      <td>694106</td>
      <td>1863.6</td>
      <td>0.268489</td>
      <td>630.593475</td>
      <td>1175174</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3. html</td>
      <td>HTML</td>
      <td>9626374</td>
      <td>813.6</td>
      <td>0.008452</td>
      <td>1408.435349</td>
      <td>1145903</td>
    </tr>
  </tbody>
</table>
</div>



# 키워드 기준 group


```python
grouped = df.groupby('키워드')
```


```python
grouped
```




    <pandas.core.groupby.generic.DataFrameGroupBy object at 0x00000124B3781EE0>




```python
grouped.count()
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
      <th>광고그룹</th>
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
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
      <th>-</th>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>회계</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>회계책</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>회사분위기</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>회사소개서</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>후위표기법</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>1112 rows × 6 columns</p>
</div>




```python
grouped.mean()
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-</th>
      <td>510444.5</td>
      <td>2636.7</td>
      <td>0.516004</td>
      <td>388.533776</td>
      <td>848233.375</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1976.0</td>
      <td>2.4</td>
      <td>0.121457</td>
      <td>1283.333333</td>
      <td>3080.000</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034.0</td>
      <td>1.2</td>
      <td>0.116054</td>
      <td>2667.500000</td>
      <td>3201.000</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580.0</td>
      <td>1.2</td>
      <td>0.075949</td>
      <td>522.500000</td>
      <td>627.000</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948.0</td>
      <td>3.6</td>
      <td>0.072757</td>
      <td>275.000000</td>
      <td>990.000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>회계</th>
      <td>3269.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>회계책</th>
      <td>3742.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>회사분위기</th>
      <td>3599.0</td>
      <td>1.2</td>
      <td>0.033343</td>
      <td>64.166667</td>
      <td>77.000</td>
    </tr>
    <tr>
      <th>회사소개서</th>
      <td>5592.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
    </tr>
    <tr>
      <th>후위표기법</th>
      <td>1516.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
<p>1112 rows × 5 columns</p>
</div>




```python
grouped.median()
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-</th>
      <td>114258.5</td>
      <td>608.4</td>
      <td>0.428236</td>
      <td>317.072650</td>
      <td>301988.5</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1976.0</td>
      <td>2.4</td>
      <td>0.121457</td>
      <td>1283.333333</td>
      <td>3080.0</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034.0</td>
      <td>1.2</td>
      <td>0.116054</td>
      <td>2667.500000</td>
      <td>3201.0</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580.0</td>
      <td>1.2</td>
      <td>0.075949</td>
      <td>522.500000</td>
      <td>627.0</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948.0</td>
      <td>3.6</td>
      <td>0.072757</td>
      <td>275.000000</td>
      <td>990.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>회계</th>
      <td>3269.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>회계책</th>
      <td>3742.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>회사분위기</th>
      <td>3599.0</td>
      <td>1.2</td>
      <td>0.033343</td>
      <td>64.166667</td>
      <td>77.0</td>
    </tr>
    <tr>
      <th>회사소개서</th>
      <td>5592.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>후위표기법</th>
      <td>1516.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>1112 rows × 5 columns</p>
</div>




```python
grouped.sum()
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-</th>
      <td>4083556</td>
      <td>21093.6</td>
      <td>4.128033</td>
      <td>3108.270211</td>
      <td>6785867</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1976</td>
      <td>2.4</td>
      <td>0.121457</td>
      <td>1283.333333</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034</td>
      <td>1.2</td>
      <td>0.116054</td>
      <td>2667.500000</td>
      <td>3201</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580</td>
      <td>1.2</td>
      <td>0.075949</td>
      <td>522.500000</td>
      <td>627</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948</td>
      <td>3.6</td>
      <td>0.072757</td>
      <td>275.000000</td>
      <td>990</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>회계</th>
      <td>3269</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>회계책</th>
      <td>3742</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>회사분위기</th>
      <td>3599</td>
      <td>1.2</td>
      <td>0.033343</td>
      <td>64.166667</td>
      <td>77</td>
    </tr>
    <tr>
      <th>회사소개서</th>
      <td>5592</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>후위표기법</th>
      <td>1516</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1112 rows × 5 columns</p>
</div>




```python
df_group=grouped.sum()
df_group
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-</th>
      <td>4083556</td>
      <td>21093.6</td>
      <td>4.128033</td>
      <td>3108.270211</td>
      <td>6785867</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1976</td>
      <td>2.4</td>
      <td>0.121457</td>
      <td>1283.333333</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034</td>
      <td>1.2</td>
      <td>0.116054</td>
      <td>2667.500000</td>
      <td>3201</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580</td>
      <td>1.2</td>
      <td>0.075949</td>
      <td>522.500000</td>
      <td>627</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948</td>
      <td>3.6</td>
      <td>0.072757</td>
      <td>275.000000</td>
      <td>990</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>회계</th>
      <td>3269</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>회계책</th>
      <td>3742</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>회사분위기</th>
      <td>3599</td>
      <td>1.2</td>
      <td>0.033343</td>
      <td>64.166667</td>
      <td>77</td>
    </tr>
    <tr>
      <th>회사소개서</th>
      <td>5592</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>후위표기법</th>
      <td>1516</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1112 rows × 5 columns</p>
</div>




```python
#클릭률(ctr) = 클릭수 / 노출수
#데이터전처리 - 데이터프레임의 열 단위 수치연산
df_group['클릭률(%)']=df_group['클릭수']/df_group['노출수']
```


```python
df_group.head()
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10억건물</th>
      <td>1976</td>
      <td>2.4</td>
      <td>0</td>
      <td>1283</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034</td>
      <td>1.2</td>
      <td>0</td>
      <td>2668</td>
      <td>3201</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580</td>
      <td>1.2</td>
      <td>0</td>
      <td>522</td>
      <td>627</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948</td>
      <td>3.6</td>
      <td>0</td>
      <td>275</td>
      <td>990</td>
    </tr>
    <tr>
      <th>1인미디어</th>
      <td>37024</td>
      <td>31.2</td>
      <td>0</td>
      <td>1828</td>
      <td>57035</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 평균클릭비용 칼럼 반올림처리(round), 소수점 제거(astype(int)
df_group['평균클릭비용(VAT포함,원)']=round(df_group['평균클릭비용(VAT포함,원)'],0)
df_group['평균클릭비용(VAT포함,원)']=df_group['평균클릭비용(VAT포함,원)'].astype(int)
```


```python
df_group.head()
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-</th>
      <td>4083556</td>
      <td>21093.6</td>
      <td>0.005165</td>
      <td>3108</td>
      <td>6785867</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1976</td>
      <td>2.4</td>
      <td>0.001215</td>
      <td>1283</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034</td>
      <td>1.2</td>
      <td>0.001161</td>
      <td>2668</td>
      <td>3201</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580</td>
      <td>1.2</td>
      <td>0.000759</td>
      <td>522</td>
      <td>627</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948</td>
      <td>3.6</td>
      <td>0.000728</td>
      <td>275</td>
      <td>990</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_group['클릭률(%)']=round(df_group['클릭률(%)'],0)
df_group['클릭률(%)']=df_group['클릭률(%)'].astype(int)
```


```python
df_group.head()
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>-</th>
      <td>4083556</td>
      <td>21093.6</td>
      <td>0</td>
      <td>3108</td>
      <td>6785867</td>
    </tr>
    <tr>
      <th>10억건물</th>
      <td>1976</td>
      <td>2.4</td>
      <td>0</td>
      <td>1283</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034</td>
      <td>1.2</td>
      <td>0</td>
      <td>2668</td>
      <td>3201</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580</td>
      <td>1.2</td>
      <td>0</td>
      <td>522</td>
      <td>627</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948</td>
      <td>3.6</td>
      <td>0</td>
      <td>275</td>
      <td>990</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_group.drop(['-'],inplace=True)
```


```python
df_group
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10억건물</th>
      <td>1976</td>
      <td>2.4</td>
      <td>0</td>
      <td>1283</td>
      <td>3080</td>
    </tr>
    <tr>
      <th>11번가상품등록</th>
      <td>1034</td>
      <td>1.2</td>
      <td>0</td>
      <td>2668</td>
      <td>3201</td>
    </tr>
    <tr>
      <th>11번가입점</th>
      <td>1580</td>
      <td>1.2</td>
      <td>0</td>
      <td>522</td>
      <td>627</td>
    </tr>
    <tr>
      <th>11번가판매자</th>
      <td>4948</td>
      <td>3.6</td>
      <td>0</td>
      <td>275</td>
      <td>990</td>
    </tr>
    <tr>
      <th>1인미디어</th>
      <td>37024</td>
      <td>31.2</td>
      <td>0</td>
      <td>1828</td>
      <td>57035</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>회계</th>
      <td>3269</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>회계책</th>
      <td>3742</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>회사분위기</th>
      <td>3599</td>
      <td>1.2</td>
      <td>0</td>
      <td>64</td>
      <td>77</td>
    </tr>
    <tr>
      <th>회사소개서</th>
      <td>5592</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>후위표기법</th>
      <td>1516</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1111 rows × 5 columns</p>
</div>



# 데이터 시각화


```python
df_group['클릭수'].plot()
plt.xticks(fontsize=5) #x라벨 size 조절(10억건물등...)
plt.show()
```



![output_25_0](./img/keywordData/output_25_0.png)




```python
imp=df_group['노출수']
clk=df_group['클릭수']
```


```python
result = df_group[(imp>imp.quantile(0.95))&(clk>clk.quantile(0.95))]
```


```python
result
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
      <th>노출수</th>
      <th>클릭수</th>
      <th>클릭률(%)</th>
      <th>평균클릭비용(VAT포함,원)</th>
      <th>총비용(VAT포함,원)</th>
    </tr>
    <tr>
      <th>키워드</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>HTML</th>
      <td>10540218</td>
      <td>894.0</td>
      <td>0</td>
      <td>2602</td>
      <td>1241867</td>
    </tr>
    <tr>
      <th>가상화폐</th>
      <td>91369</td>
      <td>2838.0</td>
      <td>0</td>
      <td>283</td>
      <td>803770</td>
    </tr>
    <tr>
      <th>글씨체</th>
      <td>106648</td>
      <td>216.0</td>
      <td>0</td>
      <td>425</td>
      <td>91806</td>
    </tr>
    <tr>
      <th>마블</th>
      <td>907619</td>
      <td>228.0</td>
      <td>0</td>
      <td>265</td>
      <td>60533</td>
    </tr>
    <tr>
      <th>마케팅</th>
      <td>392941</td>
      <td>241.2</td>
      <td>0</td>
      <td>3431</td>
      <td>280742</td>
    </tr>
    <tr>
      <th>바이럴마케팅</th>
      <td>3095998</td>
      <td>261.6</td>
      <td>0</td>
      <td>220</td>
      <td>57563</td>
    </tr>
    <tr>
      <th>블록체인</th>
      <td>347748</td>
      <td>3117.6</td>
      <td>0</td>
      <td>4914</td>
      <td>975172</td>
    </tr>
    <tr>
      <th>스케치</th>
      <td>300594</td>
      <td>168.0</td>
      <td>0</td>
      <td>1959</td>
      <td>182864</td>
    </tr>
    <tr>
      <th>에프터이펙트</th>
      <td>113863</td>
      <td>282.0</td>
      <td>0</td>
      <td>8629</td>
      <td>270996</td>
    </tr>
    <tr>
      <th>엑셀</th>
      <td>2043217</td>
      <td>541.2</td>
      <td>0</td>
      <td>2081</td>
      <td>668206</td>
    </tr>
    <tr>
      <th>영상편집</th>
      <td>140077</td>
      <td>372.0</td>
      <td>0</td>
      <td>7585</td>
      <td>862763</td>
    </tr>
    <tr>
      <th>인디자인</th>
      <td>187745</td>
      <td>562.8</td>
      <td>0</td>
      <td>5824</td>
      <td>246026</td>
    </tr>
    <tr>
      <th>일러스트</th>
      <td>1238949</td>
      <td>358.8</td>
      <td>0</td>
      <td>8992</td>
      <td>1713129</td>
    </tr>
    <tr>
      <th>일러스트레이터</th>
      <td>250333</td>
      <td>783.6</td>
      <td>0</td>
      <td>5623</td>
      <td>280170</td>
    </tr>
    <tr>
      <th>컴퓨터활용능력</th>
      <td>139729</td>
      <td>1534.8</td>
      <td>0</td>
      <td>239</td>
      <td>367147</td>
    </tr>
    <tr>
      <th>컴퓨터활용능력1급</th>
      <td>94757</td>
      <td>1191.6</td>
      <td>0</td>
      <td>237</td>
      <td>282018</td>
    </tr>
    <tr>
      <th>컴퓨터활용능력2급</th>
      <td>88751</td>
      <td>1282.8</td>
      <td>0</td>
      <td>234</td>
      <td>300058</td>
    </tr>
    <tr>
      <th>코딩</th>
      <td>562162</td>
      <td>271.2</td>
      <td>0</td>
      <td>3243</td>
      <td>879560</td>
    </tr>
    <tr>
      <th>파이썬</th>
      <td>418986</td>
      <td>272.4</td>
      <td>0</td>
      <td>6070</td>
      <td>978450</td>
    </tr>
    <tr>
      <th>펀드</th>
      <td>157068</td>
      <td>208.8</td>
      <td>0</td>
      <td>468</td>
      <td>53097</td>
    </tr>
    <tr>
      <th>포토샵</th>
      <td>3731749</td>
      <td>3218.4</td>
      <td>0</td>
      <td>5167</td>
      <td>1527383</td>
    </tr>
    <tr>
      <th>폰트</th>
      <td>478588</td>
      <td>474.0</td>
      <td>0</td>
      <td>396</td>
      <td>187693</td>
    </tr>
    <tr>
      <th>프리미어프로</th>
      <td>340821</td>
      <td>171.6</td>
      <td>0</td>
      <td>2775</td>
      <td>255299</td>
    </tr>
  </tbody>
</table>
</div>




```python
bar = result['클릭수'].plot.bar(grid=True)
plt.xticks(fontsize=8) #x라벨 size 조절(10억건물등...)
bar.set_xlabel("키워드")
plt.show()

```



![output_29_0](./img/keywordData/output_29_0.png)
    

