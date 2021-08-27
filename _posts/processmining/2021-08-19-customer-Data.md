---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 고객 데이터 분석 프로젝트
tags: [processmining]
class: post-template
subclass: 'post tag-processmining'
author: SeongJae Yu 
sitemap :
changefreq : daily
priority : 1.0
---
{% include python-table-of-contents.html %}
 
# 고객 데이터 분석
### 데이터 출처 : UCI Machine Learning Repository

[링크주소 및 다운로드]https://archive.ics.uci.edu/ml/datasets/bank+marketing
<br>

#### Moro, S., Cortez, P., & Rita, P. (2014). A data-driven approach to predict the success of bank telemarketing. Decision Support Systems, 62, 22-31

# <데이터 소개>
- 해외의 은행이 진행한 마케팅 데이터
- 아웃바운드 텔레마케팅으로 마케팅 캠페인을 진행

# bank client data:
1 - age (numeric)<br>
2 - job : type of job (categorical: 'admin.','blue-collar','entrepreneur','housemaid','management','retired','self-employed','services','student','technician','unemployed','unknown')<br>
3 - marital : marital status (categorical: 'divorced','married','single','unknown'; note: 'divorced' means divorced or widowed)<br>
4 - education (categorical: 'basic.4y','basic.6y','basic.9y','high.school','illiterate','professional.course','university.degree','unknown')<br>
5 - default: has credit in default? (categorical: 'no','yes','unknown')<br>
6 - housing: has housing loan? (categorical: 'no','yes','unknown')<br>
7 - loan: has personal loan? (categorical: 'no','yes','unknown')<br>

# related with the last contact of the current campaign:
8 - contact: contact communication type (categorical: 'cellular','telephone') <br>
9 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')<br>
10 - day_of_week: last contact day of the week (categorical: 'mon','tue','wed','thu','fri')<br>
11 - duration: last contact duration, in seconds (numeric). <br>
# other attributes:
12 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)<br>
13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)<br>
14 - previous: number of contacts performed before this campaign and for this client (numeric)<br>
15 - poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success')<br>
# social and economic context attributes
16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)<br>
17 - cons.price.idx: consumer price index - monthly indicator (numeric) <br>
18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric) <br>
19 - euribor3m: euribor 3 month rate - daily indicator (numeric)<br>
20 - nr.employed: number of employees - quarterly indicator (numeric)<br>

# Output variable:
21 - y - has the client subscribed a term deposit? (binary: 'yes','no')<br>




```python
import pandas as pd
from pandas import Series
from pandas import DataFrame
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

# 데이터불러오기
파일명 : bank-additional-full.csv


```python
#window 방법1 : \\
#engine='python' - 에러 반환시, 디렉토리 혹은 파일명에 한글이 있을 경우 추가
df=pd.read_csv('bankadditionalfull.csv')
```


```python
#head()
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
      <th>age;"job";"marital";"education";"default";"housing";"loan";"contact";"month";"day_of_week";"duration";"campaign";"pdays";"previous";"poutcome";"emp.var.rate";"cons.price.idx";"cons.conf.idx";"euribor3m";"nr.employed";"y"</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>56;"housemaid";"married";"basic.4y";"no";"no";...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>57;"services";"married";"high.school";"unknown...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37;"services";"married";"high.school";"no";"ye...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>40;"admin.";"married";"basic.6y";"no";"no";"no...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>56;"services";"married";"high.school";"no";"no...</td>
    </tr>
  </tbody>
</table>
</div>



- 엑셀로 데이터를 열었을 때의 화면
- 콜론으로 구분된 데이터<br>
  ![customer1](./img/customerData/customer1.png)


```python
#window
#sep = ';'
df=pd.read_csv('bankadditionalfull.csv',
                 engine='python',sep=';')
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>56</td>
      <td>housemaid</td>
      <td>married</td>
      <td>basic.4y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>57</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>40</td>
      <td>admin.</td>
      <td>married</td>
      <td>basic.6y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>56</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>yes</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



## # 데이터탐색

- 학습목표 :
1. 데이터 탐색과정에서 사용되는 함수를 살펴보고 실전 사례를 통해 사용법을 익힌다.


```python
#head - 데이터의 첫 5행, default : 5행
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>56</td>
      <td>housemaid</td>
      <td>married</td>
      <td>basic.4y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>57</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>40</td>
      <td>admin.</td>
      <td>married</td>
      <td>basic.6y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>56</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>yes</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
df.head(10)
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>56</td>
      <td>housemaid</td>
      <td>married</td>
      <td>basic.4y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>57</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>40</td>
      <td>admin.</td>
      <td>married</td>
      <td>basic.6y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>56</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>yes</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>5</th>
      <td>45</td>
      <td>services</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>6</th>
      <td>59</td>
      <td>admin.</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>7</th>
      <td>41</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>unknown</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>8</th>
      <td>24</td>
      <td>technician</td>
      <td>single</td>
      <td>professional.course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>9</th>
      <td>25</td>
      <td>services</td>
      <td>single</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 21 columns</p>
</div>




```python
#tail - 데이터의 끝 5행, default : 5행
df.tail()
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41183</th>
      <td>73</td>
      <td>retired</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>41184</th>
      <td>46</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41185</th>
      <td>56</td>
      <td>retired</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41186</th>
      <td>44</td>
      <td>technician</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>41187</th>
      <td>74</td>
      <td>retired</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>3</td>
      <td>999</td>
      <td>1</td>
      <td>failure</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
df.tail(10)
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41178</th>
      <td>62</td>
      <td>retired</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>thu</td>
      <td>...</td>
      <td>2</td>
      <td>6</td>
      <td>3</td>
      <td>success</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.031</td>
      <td>4963.6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>41179</th>
      <td>64</td>
      <td>retired</td>
      <td>divorced</td>
      <td>professional.course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>3</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41180</th>
      <td>36</td>
      <td>admin.</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41181</th>
      <td>37</td>
      <td>admin.</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>41182</th>
      <td>29</td>
      <td>unemployed</td>
      <td>single</td>
      <td>basic.4y</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>9</td>
      <td>1</td>
      <td>success</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41183</th>
      <td>73</td>
      <td>retired</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>41184</th>
      <td>46</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41185</th>
      <td>56</td>
      <td>retired</td>
      <td>married</td>
      <td>university.degree</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>2</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
    <tr>
      <th>41186</th>
      <td>44</td>
      <td>technician</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>41187</th>
      <td>74</td>
      <td>retired</td>
      <td>married</td>
      <td>professional.course</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>cellular</td>
      <td>nov</td>
      <td>fri</td>
      <td>...</td>
      <td>3</td>
      <td>999</td>
      <td>1</td>
      <td>failure</td>
      <td>-1.1</td>
      <td>94.767</td>
      <td>-50.8</td>
      <td>1.028</td>
      <td>4963.6</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 21 columns</p>
</div>



# 결측치 확인


```python
# 결측치 확인
df.isnull()
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>41183</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>41184</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>41185</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>41186</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>41187</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>41188 rows × 21 columns</p>
</div>




```python
# 결측치 확인 - 열단위
df.isnull().sum()
```




    age               0
    job               0
    marital           0
    education         0
    default           0
    housing           0
    loan              0
    contact           0
    month             0
    day_of_week       0
    duration          0
    campaign          0
    pdays             0
    previous          0
    poutcome          0
    emp.var.rate      0
    cons.price.idx    0
    cons.conf.idx     0
    euribor3m         0
    nr.employed       0
    y                 0
    dtype: int64




```python
#shape - dataframe의 크기(행, 열의 수)
df.shape
```




    (41188, 21)




```python
#describe() - 열에 대한 기술통계량
#데이터의 수, 평균, 표준편차, 최소값, 1사분위수, 2사분위수, 3사분위수, 최대값
df.describe()
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
      <th>age</th>
      <th>duration</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>41188.00000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
      <td>41188.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>40.02406</td>
      <td>258.285010</td>
      <td>2.567593</td>
      <td>962.475454</td>
      <td>0.172963</td>
      <td>0.081886</td>
      <td>93.575664</td>
      <td>-40.502600</td>
      <td>3.621291</td>
      <td>5167.035911</td>
    </tr>
    <tr>
      <th>std</th>
      <td>10.42125</td>
      <td>259.279249</td>
      <td>2.770014</td>
      <td>186.910907</td>
      <td>0.494901</td>
      <td>1.570960</td>
      <td>0.578840</td>
      <td>4.628198</td>
      <td>1.734447</td>
      <td>72.251528</td>
    </tr>
    <tr>
      <th>min</th>
      <td>17.00000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-3.400000</td>
      <td>92.201000</td>
      <td>-50.800000</td>
      <td>0.634000</td>
      <td>4963.600000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>32.00000</td>
      <td>102.000000</td>
      <td>1.000000</td>
      <td>999.000000</td>
      <td>0.000000</td>
      <td>-1.800000</td>
      <td>93.075000</td>
      <td>-42.700000</td>
      <td>1.344000</td>
      <td>5099.100000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>38.00000</td>
      <td>180.000000</td>
      <td>2.000000</td>
      <td>999.000000</td>
      <td>0.000000</td>
      <td>1.100000</td>
      <td>93.749000</td>
      <td>-41.800000</td>
      <td>4.857000</td>
      <td>5191.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>47.00000</td>
      <td>319.000000</td>
      <td>3.000000</td>
      <td>999.000000</td>
      <td>0.000000</td>
      <td>1.400000</td>
      <td>93.994000</td>
      <td>-36.400000</td>
      <td>4.961000</td>
      <td>5228.100000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>98.00000</td>
      <td>4918.000000</td>
      <td>56.000000</td>
      <td>999.000000</td>
      <td>7.000000</td>
      <td>1.400000</td>
      <td>94.767000</td>
      <td>-26.900000</td>
      <td>5.045000</td>
      <td>5228.100000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#columns - 칼럼명 반환
df.columns
```




    Index(['age', 'job', 'marital', 'education', 'default', 'housing', 'loan',
           'contact', 'month', 'day_of_week', 'duration', 'campaign', 'pdays',
           'previous', 'poutcome', 'emp.var.rate', 'cons.price.idx',
           'cons.conf.idx', 'euribor3m', 'nr.employed', 'y'],
          dtype='object')




```python
#unique() - 열의 고유값
#education
df['education'].unique()
```




    array(['basic.4y', 'high.school', 'basic.6y', 'basic.9y',
           'professional.course', 'unknown', 'university.degree',
           'illiterate'], dtype=object)




```python
#value_counts() - 열의 고유값 빈도
#education
df['education'].value_counts()
```




    university.degree      12168
    high.school             9515
    basic.9y                6045
    professional.course     5243
    basic.4y                4176
    basic.6y                2292
    unknown                 1731
    illiterate                18
    Name: education, dtype: int64




```python
#unique() - 열의 고유값
#marital
df['marital'].unique()
```




    array(['married', 'single', 'divorced', 'unknown'], dtype=object)




```python
#value_counts() - 열의 고유값 빈도
#marital
df['marital'].value_counts()
```




    married     24928
    single      11568
    divorced     4612
    unknown        80
    Name: marital, dtype: int64



# 데이터 시각화
- 학습목표 :
1. 현업의 데이터를 사용하여 데이터 시각화를 실습한다.
2. 데이터를 가공,처리하여 시각화를 진행한다.


```python
df['age']
```




    0        56
    1        57
    2        37
    3        40
    4        56
             ..
    41183    73
    41184    46
    41185    56
    41186    44
    41187    74
    Name: age, Length: 41188, dtype: int64




```python
df['age'].plot()
plt.show()
#x축이 인덱스 순서
```



![output_30_0](./img/customerData/output_30_0.png)



- age 칼럼 선그래프 그리기(오름차순)
1. 노출수칼럼을 수치 순서대로 오름차순 정렬
2. 정렬된 데이터(시리즈)의 형태대로 인덱스 재생성


```python
#오름차순 정렬
#age칼럼 
age=df['age'].sort_values()
```


```python
#age변수출력
age
```




    38274    17
    37579    17
    37539    17
    37140    17
    37558    17
             ..
    40450    92
    38921    94
    27826    95
    38455    98
    38452    98
    Name: age, Length: 41188, dtype: int64




```python
#reset_index - 인덱스 재생성, 기존 인덱스를 데이터프레임의 열로 반환
age=age.reset_index()
```


```python
#age 변수출력
age
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
      <th>index</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38274</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>37579</td>
      <td>17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37539</td>
      <td>17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37140</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>37558</td>
      <td>17</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>41183</th>
      <td>40450</td>
      <td>92</td>
    </tr>
    <tr>
      <th>41184</th>
      <td>38921</td>
      <td>94</td>
    </tr>
    <tr>
      <th>41185</th>
      <td>27826</td>
      <td>95</td>
    </tr>
    <tr>
      <th>41186</th>
      <td>38455</td>
      <td>98</td>
    </tr>
    <tr>
      <th>41187</th>
      <td>38452</td>
      <td>98</td>
    </tr>
  </tbody>
</table>
<p>41188 rows × 2 columns</p>
</div>




```python
#drop(axis=1) - 삭제(열 기준)
age=age.drop('index',axis=1)
```


```python
#age 변수출력
age
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
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>41183</th>
      <td>92</td>
    </tr>
    <tr>
      <th>41184</th>
      <td>94</td>
    </tr>
    <tr>
      <th>41185</th>
      <td>95</td>
    </tr>
    <tr>
      <th>41186</th>
      <td>98</td>
    </tr>
    <tr>
      <th>41187</th>
      <td>98</td>
    </tr>
  </tbody>
</table>
<p>41188 rows × 1 columns</p>
</div>




```python
#plotting
age.plot()
plt.show()
#값의 오름차순별로 정렬한 그래프
#보통 '나이'를 20대,30대,40..대로 나누어 데이터를 확인함
```



![output_38_0](./img/customerData/output_38_0.png)




```python
#계급간 빈도를 나타내주는 히스토그램
df['age'].plot.hist()
plt.show()
```



![output_39_0](./img/customerData/output_39_0.png)




```python
#히스토그램
#bins - 계급구간(10,20,30...100)
#figsize=[15,8]
#xticks(fontsize=15)
#yticks(fontsize=15)
#plt.title('Histogram of df.age',fontsize=20)

df['age'].plot.hist(bins=range(10,101,10),figsize=[15,8])
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.title('Histogram of df.age',fontsize=20)
plt.show()
```



![output_40_0](./img/customerData/output_40_0.png)




```python
#시각화 예제2 : duration(전화통화시간) 선 그래프 시각화
(((df['duration'].sort_values()).reset_index()).drop('index',axis=1)).plot()
plt.show()
#1. 선그래프로 데이터의 패턴 분석
#2. 히스토그램으로 전화통화 시간별 빈도 분석
```



![output_41_0](./img/customerData/output_41_0.png)




```python
#히스토그램의 계급구간을 설정하기 위한 최소값, 최대값 파악
#describe()
df['duration'].describe()
```




    count    41188.000000
    mean       258.285010
    std        259.279249
    min          0.000000
    25%        102.000000
    50%        180.000000
    75%        319.000000
    max       4918.000000
    Name: duration, dtype: float64




```python
#bins=range(0,5001,100)
df['duration'].plot.hist(bins=range(0,5001,100))
plt.show()
```



![output_43_0](./img/customerData/output_43_0.png)




```python
#히스토그램
#bins - 계급구간(0,100,200...5000)
#figsize=[15,8]
#xticks(fontsize=15)
#yticks(fontsize=15)
#plt.title('Histogram of df.duration',fontsize=20)

df['duration'].plot.hist(bins=range(0,5001,100), figsize=[15,8])
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.title('Histogram of df.duration',fontsize=20)
plt.show()
```



![output_44_0](./img/customerData/output_44_0.png)



# 막대그래프, 가로막대그래프


```python
#선그래프
#marital
df['marital'].plot()
plt.show()
#no numeric data to plot
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-36-7df365c79815> in <module>
          1 #선그래프
          2 #marital
    ----> 3 df['marital'].plot()
          4 plt.show()
          5 #no numeric data to plot
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_core.py in __call__(self, *args, **kwargs)
        953                     data.columns = label_name
        954 
    --> 955         return plot_backend.plot(data, kind=kind, **kwargs)
        956 
        957     __call__.__doc__ = __doc__
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_matplotlib\__init__.py in plot(data, kind, **kwargs)
         59             kwargs["ax"] = getattr(ax, "left_ax", ax)
         60     plot_obj = PLOT_CLASSES[kind](data, **kwargs)
    ---> 61     plot_obj.generate()
         62     plot_obj.draw()
         63     return plot_obj.result
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_matplotlib\core.py in generate(self)
        276     def generate(self):
        277         self._args_adjust()
    --> 278         self._compute_plot_data()
        279         self._setup_subplots()
        280         self._make_plot()
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_matplotlib\core.py in _compute_plot_data(self)
        439         # no non-numeric frames or series allowed
        440         if is_empty:
    --> 441             raise TypeError("no numeric data to plot")
        442 
        443         self.data = numeric_data.apply(self._convert_to_ndarray)
    

    TypeError: no numeric data to plot


- 숫자데이터가 아니기 때문에 시각화 할 수 없다.


```python
#히스토그램
df['marital'].plot.hist()
#no numeric data to plot
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-38-414b9b3e32cb> in <module>
          1 #히스토그램
    ----> 2 df['marital'].plot.hist()
          3 #no numeric data to plot
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_core.py in hist(self, by, bins, **kwargs)
       1294             >>> ax = df.plot.hist(bins=12, alpha=0.5)
       1295         """
    -> 1296         return self(kind="hist", by=by, bins=bins, **kwargs)
       1297 
       1298     def kde(self, bw_method=None, ind=None, **kwargs):
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_core.py in __call__(self, *args, **kwargs)
        953                     data.columns = label_name
        954 
    --> 955         return plot_backend.plot(data, kind=kind, **kwargs)
        956 
        957     __call__.__doc__ = __doc__
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_matplotlib\__init__.py in plot(data, kind, **kwargs)
         59             kwargs["ax"] = getattr(ax, "left_ax", ax)
         60     plot_obj = PLOT_CLASSES[kind](data, **kwargs)
    ---> 61     plot_obj.generate()
         62     plot_obj.draw()
         63     return plot_obj.result
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_matplotlib\core.py in generate(self)
        276     def generate(self):
        277         self._args_adjust()
    --> 278         self._compute_plot_data()
        279         self._setup_subplots()
        280         self._make_plot()
    

    ~\anaconda3\lib\site-packages\pandas\plotting\_matplotlib\core.py in _compute_plot_data(self)
        439         # no non-numeric frames or series allowed
        440         if is_empty:
    --> 441             raise TypeError("no numeric data to plot")
        442 
        443         self.data = numeric_data.apply(self._convert_to_ndarray)
    

    TypeError: no numeric data to plot


- 숫자데이터가 아니기 때문에 시각화 할 수 없다.


```python
#unique()함수를 사용한 age칼럼 고유값 확인 
df['age'].unique()
#선그래프를 그린 age칼럼 데이터는 수치데이터
```




    array([56, 57, 37, 40, 45, 59, 41, 24, 25, 29, 35, 54, 46, 50, 39, 30, 55,
           49, 34, 52, 58, 32, 38, 44, 42, 60, 53, 47, 51, 48, 33, 31, 43, 36,
           28, 27, 26, 22, 23, 20, 21, 61, 19, 18, 70, 66, 76, 67, 73, 88, 95,
           77, 68, 75, 63, 80, 62, 65, 72, 82, 64, 71, 69, 78, 85, 79, 83, 81,
           74, 17, 87, 91, 86, 98, 94, 84, 92, 89], dtype=int64)




```python
#unique()함수를 사용한 marital칼럼 고유값 확인 
df['marital'].unique()
#marital칼럼의 데이터는 문자
```




    array(['married', 'single', 'divorced', 'unknown'], dtype=object)



- 막대그래프를 통한 시각화
1. value_counts
2. 막대그래프 시각화


```python
#1. value_counts() - 문자데이터는 value_counts() 함수를 사용하여 시각화 할 수 있다.
#marital
marital=df['marital'].value_counts()
```


```python
#2. marital변수 막대그래프 시각화
marital.plot.bar()
plt.show()
```



![output_54_0](./img/customerData/output_54_0.png)




```python
#2-1. 가로막대그래프 시각화
marital.plot.barh()
plt.show()
```



![output_55_0](./img/customerData/output_55_0.png)




```python
df['education'].unique()
```




    array(['basic.4y', 'high.school', 'basic.6y', 'basic.9y',
           'professional.course', 'unknown', 'university.degree',
           'illiterate'], dtype=object)




```python
#education 가로막대그래프 한줄 코드
#value_counts(),plot.barh()
education=df['education'].value_counts()
```


```python
education.plot.barh()
plt.show()
```



![output_58_0](./img/customerData/output_58_0.png)



# 데이터 분석

# 분석주제 1 :
### 대출이 있는 사람이라면 은행 상품에 잘 가입하지 않을 것이다.

- 학습목표 :
1. 가설검증과정 코딩 실습하기
2. groupby활용한 실습 진행하기

- 분석을 위한 코딩과정 도식화
1. 가입여부에 따라 가입한 그룹과 가입하지 않은 그룹으로 나눈다.
2. 나뉜 데이터를 대출여부에 따라 나눈다.
3. 가입한 그룹 중 대출이 있는 사람의 비중과, 가입하지 않은 그룹 중 대출이 있는 사람의 비중을 비교한다.

<br><br>


![customer2](./img/customerData/customer2.png)


```python
# 1. 가입여부에 따라 가입한 그룹과 가입하지 않은 그룹으로 나눈다.
#가입여부에 대한 칼럼 : 'y'
#unique()
df['y'].unique()
#groupby사용 - yes, no그룹으로 나뉘게 됨
```




    array(['no', 'yes'], dtype=object)




```python
# 1. 가입여부에 따라 가입한 그룹과 가입하지 않은 그룹으로 나눈다.
#groupby('y')
grouped=df.groupby('y')
```


```python
# 1. 가입여부에 따라 가입한 그룹과 가입하지 않은 그룹으로 나눈다.
#get_group('yes') - y칼럼이 'yes'인 데이터프레임 추출 - 가입한 그룹만 추출
#get_group('no') - y칼럼이 'no'인 데이터프레임 추출 - 가입하지 않은 그룹만 추출
yes_group=grouped.get_group('yes')
no_group=grouped.get_group('no')
```


```python
# 1. 가입여부에 따라 가입한 그룹과 가입하지 않은 그룹으로 나눈다.
#yes_group 출력
yes_group.head()
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>75</th>
      <td>41</td>
      <td>blue-collar</td>
      <td>divorced</td>
      <td>basic.4y</td>
      <td>unknown</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>83</th>
      <td>49</td>
      <td>entrepreneur</td>
      <td>married</td>
      <td>university.degree</td>
      <td>unknown</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>88</th>
      <td>49</td>
      <td>technician</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>129</th>
      <td>41</td>
      <td>technician</td>
      <td>married</td>
      <td>professional.course</td>
      <td>unknown</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>139</th>
      <td>45</td>
      <td>blue-collar</td>
      <td>married</td>
      <td>basic.9y</td>
      <td>unknown</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>yes</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
# 1-3. 가입여부에 따라 가입한 그룹과 가입하지 않은 그룹으로 나눈다.
#no_group 출력
no_group.head()
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
      <th>age</th>
      <th>job</th>
      <th>marital</th>
      <th>education</th>
      <th>default</th>
      <th>housing</th>
      <th>loan</th>
      <th>contact</th>
      <th>month</th>
      <th>day_of_week</th>
      <th>...</th>
      <th>campaign</th>
      <th>pdays</th>
      <th>previous</th>
      <th>poutcome</th>
      <th>emp.var.rate</th>
      <th>cons.price.idx</th>
      <th>cons.conf.idx</th>
      <th>euribor3m</th>
      <th>nr.employed</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>56</td>
      <td>housemaid</td>
      <td>married</td>
      <td>basic.4y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>1</th>
      <td>57</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>unknown</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>2</th>
      <td>37</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>yes</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>3</th>
      <td>40</td>
      <td>admin.</td>
      <td>married</td>
      <td>basic.6y</td>
      <td>no</td>
      <td>no</td>
      <td>no</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
    <tr>
      <th>4</th>
      <td>56</td>
      <td>services</td>
      <td>married</td>
      <td>high.school</td>
      <td>no</td>
      <td>no</td>
      <td>yes</td>
      <td>telephone</td>
      <td>may</td>
      <td>mon</td>
      <td>...</td>
      <td>1</td>
      <td>999</td>
      <td>0</td>
      <td>nonexistent</td>
      <td>1.1</td>
      <td>93.994</td>
      <td>-36.4</td>
      <td>4.857</td>
      <td>5191.0</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
#2. 나뉜 데이터(yes_group,no_group)를 대출여부(loan)에 따라 나눈다.
#value_counts
yes=yes_group['loan'].value_counts()
```


```python
#yes변수 출력
yes
#yes_group의 대출여부 빈도 출력
```




    no         3850
    yes         683
    unknown     107
    Name: loan, dtype: int64




```python
#2. 나뉜 데이터(yes_group,no_group)를 대출여부(loan)에 따라 나눈다.
#value_counts
no=no_group['loan'].value_counts()
```


```python
#no변수 출력
no
#no_group의 대출여부 빈도 출력
```




    no         30100
    yes         5565
    unknown      883
    Name: loan, dtype: int64




```python
#3. 가입한 그룹 내 대출이 있는 사람의 비중과, 가입하지 않은 그룹 내 대출이 있는 사람의 비중을 비교한다(yes_group).
#비중 : 시리즈 변수 각각의 value를 시리즈의 총합으로 나눔
#시리즈는 산술연산자(+,-,*,/,%,**,//)와 함께 사용가능
#series/series.sum()
yes=yes/yes.sum()
```


```python
#yes 출력
yes
```




    no         0.829741
    yes        0.147198
    unknown    0.023060
    Name: loan, dtype: float64




```python
#3. 가입한 그룹 내 대출이 있는 사람의 비중과, 가입하지 않은 그룹 내 대출이 있는 사람의 비중을 비교한다(no_group).
no=no/no.sum()
```


```python
#no 출력
no
```




    no         0.823574
    yes        0.152266
    unknown    0.024160
    Name: loan, dtype: float64




```python
#3. 가입한 그룹 중 대출이 있는 사람의 비중과, 가입하지 않은 그룹 중 대출이 있는 사람의 비중을 비교한다.
#concat : 시리즈 혹은 데이터프레임 결합(default-행방향 결합)
pd.concat([yes,no],axis=1)# column 합치기
#칼럼명이 모두 loan
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
      <th>loan</th>
      <th>loan</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>no</th>
      <td>0.829741</td>
      <td>0.823574</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>0.147198</td>
      <td>0.152266</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>0.023060</td>
      <td>0.024160</td>
    </tr>
  </tbody>
</table>
</div>




```python
#series.name : 시리즈의 이름 설정
yes.name='y_yes'
```


```python
#series.name : 시리즈의 이름 설정
no.name='y_no'
```


```python
pd.concat([yes,no],axis=1)
#=> 가입한 그룹의 대출 비중이 가입하지 않은 그룹보다 0.005 더 적다.
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
      <th>y_yes</th>
      <th>y_no</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>no</th>
      <td>0.829741</td>
      <td>0.823574</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>0.147198</td>
      <td>0.152266</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>0.023060</td>
      <td>0.024160</td>
    </tr>
  </tbody>
</table>
</div>



# 분석주제 2 :
# 같은 상품을 새로운 고객에게 마케팅 하려고한다.
# 연령과 상품가입여부, 직업을 함께 고려할때 마케팅 전략을 변화시켜야 할 그룹은?

- 학습목표 :
1. 가설검증과정 코딩 실습하기
2. pivot_table활용한 실습 진행하기

- 분석조건 : 세 개의 칼럼(age, job, y)을 함께 분석해야 함
- pd.pivot_table('데이터프레임 변수',values=집계 대상 칼럼(수치 데이터), index=행 인덱스가 될 칼럼명, columns=열 인덱스가 될 칼럼명, aggfunc=집계함수-sum,mean,min,max,std,var)

# pivot_table 사용예제


```python
#pd.pivot_table('데이터프레임 변수',values=집계 대상 칼럼, index=행 인덱스가 될 칼럼명, columns=열 인덱스가 될 칼럼명, aggfunc=sum)
pd.pivot_table(df,values='age',index='y',columns='job',aggfunc='mean')
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
      <th>job</th>
      <th>admin.</th>
      <th>blue-collar</th>
      <th>entrepreneur</th>
      <th>housemaid</th>
      <th>management</th>
      <th>retired</th>
      <th>self-employed</th>
      <th>services</th>
      <th>student</th>
      <th>technician</th>
      <th>unemployed</th>
      <th>unknown</th>
    </tr>
    <tr>
      <th>y</th>
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
      <th>no</th>
      <td>38.219846</td>
      <td>39.582057</td>
      <td>41.703453</td>
      <td>44.705451</td>
      <td>42.309707</td>
      <td>59.926128</td>
      <td>40.176887</td>
      <td>38.090236</td>
      <td>26.396667</td>
      <td>38.600033</td>
      <td>39.844828</td>
      <td>45.375427</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>37.968935</td>
      <td>39.200627</td>
      <td>41.935484</td>
      <td>52.650943</td>
      <td>42.783537</td>
      <td>68.253456</td>
      <td>38.006711</td>
      <td>36.077399</td>
      <td>24.800000</td>
      <td>37.746575</td>
      <td>39.062500</td>
      <td>47.054054</td>
    </tr>
  </tbody>
</table>
</div>




```python
#values,index,columns파라미터를 일일이 쓰지 않고 순서대로 입력하여 실행 가능
pd.pivot_table(df,'age','y','job',aggfunc='mean')
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
      <th>job</th>
      <th>admin.</th>
      <th>blue-collar</th>
      <th>entrepreneur</th>
      <th>housemaid</th>
      <th>management</th>
      <th>retired</th>
      <th>self-employed</th>
      <th>services</th>
      <th>student</th>
      <th>technician</th>
      <th>unemployed</th>
      <th>unknown</th>
    </tr>
    <tr>
      <th>y</th>
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
      <th>no</th>
      <td>38.219846</td>
      <td>39.582057</td>
      <td>41.703453</td>
      <td>44.705451</td>
      <td>42.309707</td>
      <td>59.926128</td>
      <td>40.176887</td>
      <td>38.090236</td>
      <td>26.396667</td>
      <td>38.600033</td>
      <td>39.844828</td>
      <td>45.375427</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>37.968935</td>
      <td>39.200627</td>
      <td>41.935484</td>
      <td>52.650943</td>
      <td>42.783537</td>
      <td>68.253456</td>
      <td>38.006711</td>
      <td>36.077399</td>
      <td>24.800000</td>
      <td>37.746575</td>
      <td>39.062500</td>
      <td>47.054054</td>
    </tr>
  </tbody>
</table>
</div>




```python
#멀티 인덱스(multi-index) - 행 인덱스
#['y','marital']
pd.pivot_table(df,'age',['y','marital'],'job',aggfunc='mean')
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
      <th>job</th>
      <th>admin.</th>
      <th>blue-collar</th>
      <th>entrepreneur</th>
      <th>housemaid</th>
      <th>management</th>
      <th>retired</th>
      <th>self-employed</th>
      <th>services</th>
      <th>student</th>
      <th>technician</th>
      <th>unemployed</th>
      <th>unknown</th>
    </tr>
    <tr>
      <th>y</th>
      <th>marital</th>
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
      <th rowspan="4" valign="top">no</th>
      <th>divorced</th>
      <td>43.098432</td>
      <td>42.903704</td>
      <td>44.042424</td>
      <td>48.806897</td>
      <td>46.123288</td>
      <td>61.480469</td>
      <td>42.871795</td>
      <td>41.991984</td>
      <td>34.500000</td>
      <td>42.173484</td>
      <td>42.140351</td>
      <td>43.300000</td>
    </tr>
    <tr>
      <th>married</th>
      <td>40.148663</td>
      <td>40.857804</td>
      <td>42.477111</td>
      <td>44.849218</td>
      <td>43.634997</td>
      <td>60.019048</td>
      <td>42.349148</td>
      <td>39.992951</td>
      <td>30.484848</td>
      <td>40.686245</td>
      <td>41.636861</td>
      <td>47.532110</td>
    </tr>
    <tr>
      <th>single</th>
      <td>33.858265</td>
      <td>33.409255</td>
      <td>35.472527</td>
      <td>38.087379</td>
      <td>34.070776</td>
      <td>53.938272</td>
      <td>33.783537</td>
      <td>32.159921</td>
      <td>26.062500</td>
      <td>33.950697</td>
      <td>33.536946</td>
      <td>38.288136</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>34.666667</td>
      <td>42.818182</td>
      <td>35.500000</td>
      <td>40.000000</td>
      <td>51.000000</td>
      <td>59.750000</td>
      <td>39.400000</td>
      <td>40.000000</td>
      <td>30.000000</td>
      <td>33.300000</td>
      <td>47.200000</td>
      <td>40.166667</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">yes</th>
      <th>divorced</th>
      <td>44.878788</td>
      <td>42.037736</td>
      <td>44.857143</td>
      <td>57.000000</td>
      <td>46.692308</td>
      <td>72.739130</td>
      <td>41.875000</td>
      <td>43.484848</td>
      <td>35.666667</td>
      <td>40.738462</td>
      <td>47.900000</td>
      <td>76.333333</td>
    </tr>
    <tr>
      <th>married</th>
      <td>41.386503</td>
      <td>41.363420</td>
      <td>43.090909</td>
      <td>54.256757</td>
      <td>44.756637</td>
      <td>67.033435</td>
      <td>41.036585</td>
      <td>38.379518</td>
      <td>31.250000</td>
      <td>41.398438</td>
      <td>41.941860</td>
      <td>58.750000</td>
    </tr>
    <tr>
      <th>single</th>
      <td>32.404594</td>
      <td>32.652174</td>
      <td>35.666667</td>
      <td>40.875000</td>
      <td>33.285714</td>
      <td>67.500000</td>
      <td>31.921569</td>
      <td>31.024194</td>
      <td>24.481061</td>
      <td>32.078853</td>
      <td>32.062500</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>42.500000</td>
      <td>37.000000</td>
      <td>31.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>66.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.000000</td>
      <td>NaN</td>
      <td>40.666667</td>
    </tr>
  </tbody>
</table>
</div>




```python
#멀티 인덱스(multi-index) - 열 인덱스
#['job','contact']
pd.pivot_table(df,'age',['y','marital'],['job','contact'],aggfunc='mean')
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
      <th>job</th>
      <th colspan="2" halign="left">admin.</th>
      <th colspan="2" halign="left">blue-collar</th>
      <th colspan="2" halign="left">entrepreneur</th>
      <th colspan="2" halign="left">housemaid</th>
      <th colspan="2" halign="left">management</th>
      <th>...</th>
      <th colspan="2" halign="left">services</th>
      <th colspan="2" halign="left">student</th>
      <th colspan="2" halign="left">technician</th>
      <th colspan="2" halign="left">unemployed</th>
      <th colspan="2" halign="left">unknown</th>
    </tr>
    <tr>
      <th></th>
      <th>contact</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>...</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
    </tr>
    <tr>
      <th>y</th>
      <th>marital</th>
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
      <th rowspan="4" valign="top">no</th>
      <th>divorced</th>
      <td>43.143639</td>
      <td>43.019185</td>
      <td>42.906907</td>
      <td>42.900585</td>
      <td>43.301887</td>
      <td>45.372881</td>
      <td>48.292683</td>
      <td>49.476190</td>
      <td>44.786164</td>
      <td>47.721805</td>
      <td>...</td>
      <td>42.028269</td>
      <td>41.944444</td>
      <td>36.000000</td>
      <td>27.000000</td>
      <td>42.069034</td>
      <td>42.435644</td>
      <td>42.253731</td>
      <td>41.978723</td>
      <td>46.000000</td>
      <td>40.600000</td>
    </tr>
    <tr>
      <th>married</th>
      <td>40.349554</td>
      <td>39.800357</td>
      <td>41.182477</td>
      <td>40.494249</td>
      <td>42.213628</td>
      <td>42.802273</td>
      <td>45.900966</td>
      <td>43.342561</td>
      <td>43.740678</td>
      <td>43.452416</td>
      <td>...</td>
      <td>40.483733</td>
      <td>39.395833</td>
      <td>32.150000</td>
      <td>27.923077</td>
      <td>40.283568</td>
      <td>41.365495</td>
      <td>41.452769</td>
      <td>41.871369</td>
      <td>48.349057</td>
      <td>46.758929</td>
    </tr>
    <tr>
      <th>single</th>
      <td>33.515821</td>
      <td>34.646707</td>
      <td>32.872838</td>
      <td>34.183554</td>
      <td>36.181034</td>
      <td>34.227273</td>
      <td>36.827586</td>
      <td>39.711111</td>
      <td>33.747405</td>
      <td>34.697987</td>
      <td>...</td>
      <td>32.360927</td>
      <td>31.863081</td>
      <td>25.396509</td>
      <td>27.742138</td>
      <td>33.529248</td>
      <td>35.008741</td>
      <td>33.252101</td>
      <td>33.940476</td>
      <td>39.535714</td>
      <td>37.161290</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>35.000000</td>
      <td>31.000000</td>
      <td>43.800000</td>
      <td>42.000000</td>
      <td>40.000000</td>
      <td>31.000000</td>
      <td>40.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>51.000000</td>
      <td>...</td>
      <td>34.500000</td>
      <td>42.750000</td>
      <td>30.000000</td>
      <td>NaN</td>
      <td>30.285714</td>
      <td>40.333333</td>
      <td>46.500000</td>
      <td>50.000000</td>
      <td>48.333333</td>
      <td>32.000000</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">yes</th>
      <th>divorced</th>
      <td>45.028302</td>
      <td>44.269231</td>
      <td>41.500000</td>
      <td>43.400000</td>
      <td>45.333333</td>
      <td>44.000000</td>
      <td>59.857143</td>
      <td>37.000000</td>
      <td>46.297297</td>
      <td>54.000000</td>
      <td>...</td>
      <td>43.083333</td>
      <td>44.555556</td>
      <td>35.666667</td>
      <td>NaN</td>
      <td>40.701754</td>
      <td>41.000000</td>
      <td>51.000000</td>
      <td>40.666667</td>
      <td>76.333333</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>married</th>
      <td>41.362832</td>
      <td>41.540230</td>
      <td>41.682432</td>
      <td>40.608000</td>
      <td>44.177419</td>
      <td>40.500000</td>
      <td>55.375000</td>
      <td>50.777778</td>
      <td>45.401099</td>
      <td>42.090909</td>
      <td>...</td>
      <td>38.634921</td>
      <td>37.575000</td>
      <td>30.333333</td>
      <td>34.000000</td>
      <td>41.299065</td>
      <td>41.904762</td>
      <td>42.746479</td>
      <td>38.133333</td>
      <td>58.181818</td>
      <td>60.000000</td>
    </tr>
    <tr>
      <th>single</th>
      <td>32.117284</td>
      <td>34.150000</td>
      <td>31.934426</td>
      <td>34.897436</td>
      <td>35.117647</td>
      <td>38.000000</td>
      <td>40.769231</td>
      <td>41.333333</td>
      <td>32.763636</td>
      <td>36.875000</td>
      <td>...</td>
      <td>30.846154</td>
      <td>31.950000</td>
      <td>24.174468</td>
      <td>26.965517</td>
      <td>31.995902</td>
      <td>32.657143</td>
      <td>32.355556</td>
      <td>27.666667</td>
      <td>31.000000</td>
      <td>28.000000</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>42.500000</td>
      <td>NaN</td>
      <td>37.000000</td>
      <td>NaN</td>
      <td>31.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>45.000000</td>
      <td>32.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 24 columns</p>
</div>




```python
#fill-value - 결측치 대체
#fill_value=0
pd.pivot_table(df,'age',['y','marital'],['job','contact'],aggfunc='mean',fill_value=0)
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
      <th>job</th>
      <th colspan="2" halign="left">admin.</th>
      <th colspan="2" halign="left">blue-collar</th>
      <th colspan="2" halign="left">entrepreneur</th>
      <th colspan="2" halign="left">housemaid</th>
      <th colspan="2" halign="left">management</th>
      <th>...</th>
      <th colspan="2" halign="left">services</th>
      <th colspan="2" halign="left">student</th>
      <th colspan="2" halign="left">technician</th>
      <th colspan="2" halign="left">unemployed</th>
      <th colspan="2" halign="left">unknown</th>
    </tr>
    <tr>
      <th></th>
      <th>contact</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>...</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
      <th>cellular</th>
      <th>telephone</th>
    </tr>
    <tr>
      <th>y</th>
      <th>marital</th>
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
      <th rowspan="4" valign="top">no</th>
      <th>divorced</th>
      <td>43.143639</td>
      <td>43.019185</td>
      <td>42.906907</td>
      <td>42.900585</td>
      <td>43.301887</td>
      <td>45.372881</td>
      <td>48.292683</td>
      <td>49.476190</td>
      <td>44.786164</td>
      <td>47.721805</td>
      <td>...</td>
      <td>42.028269</td>
      <td>41.944444</td>
      <td>36.000000</td>
      <td>27.000000</td>
      <td>42.069034</td>
      <td>42.435644</td>
      <td>42.253731</td>
      <td>41.978723</td>
      <td>46.000000</td>
      <td>40.600000</td>
    </tr>
    <tr>
      <th>married</th>
      <td>40.349554</td>
      <td>39.800357</td>
      <td>41.182477</td>
      <td>40.494249</td>
      <td>42.213628</td>
      <td>42.802273</td>
      <td>45.900966</td>
      <td>43.342561</td>
      <td>43.740678</td>
      <td>43.452416</td>
      <td>...</td>
      <td>40.483733</td>
      <td>39.395833</td>
      <td>32.150000</td>
      <td>27.923077</td>
      <td>40.283568</td>
      <td>41.365495</td>
      <td>41.452769</td>
      <td>41.871369</td>
      <td>48.349057</td>
      <td>46.758929</td>
    </tr>
    <tr>
      <th>single</th>
      <td>33.515821</td>
      <td>34.646707</td>
      <td>32.872838</td>
      <td>34.183554</td>
      <td>36.181034</td>
      <td>34.227273</td>
      <td>36.827586</td>
      <td>39.711111</td>
      <td>33.747405</td>
      <td>34.697987</td>
      <td>...</td>
      <td>32.360927</td>
      <td>31.863081</td>
      <td>25.396509</td>
      <td>27.742138</td>
      <td>33.529248</td>
      <td>35.008741</td>
      <td>33.252101</td>
      <td>33.940476</td>
      <td>39.535714</td>
      <td>37.161290</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>35.000000</td>
      <td>31.000000</td>
      <td>43.800000</td>
      <td>42.000000</td>
      <td>40.000000</td>
      <td>31.000000</td>
      <td>40.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>51.000000</td>
      <td>...</td>
      <td>34.500000</td>
      <td>42.750000</td>
      <td>30.000000</td>
      <td>0.000000</td>
      <td>30.285714</td>
      <td>40.333333</td>
      <td>46.500000</td>
      <td>50.000000</td>
      <td>48.333333</td>
      <td>32.000000</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">yes</th>
      <th>divorced</th>
      <td>45.028302</td>
      <td>44.269231</td>
      <td>41.500000</td>
      <td>43.400000</td>
      <td>45.333333</td>
      <td>44.000000</td>
      <td>59.857143</td>
      <td>37.000000</td>
      <td>46.297297</td>
      <td>54.000000</td>
      <td>...</td>
      <td>43.083333</td>
      <td>44.555556</td>
      <td>35.666667</td>
      <td>0.000000</td>
      <td>40.701754</td>
      <td>41.000000</td>
      <td>51.000000</td>
      <td>40.666667</td>
      <td>76.333333</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>married</th>
      <td>41.362832</td>
      <td>41.540230</td>
      <td>41.682432</td>
      <td>40.608000</td>
      <td>44.177419</td>
      <td>40.500000</td>
      <td>55.375000</td>
      <td>50.777778</td>
      <td>45.401099</td>
      <td>42.090909</td>
      <td>...</td>
      <td>38.634921</td>
      <td>37.575000</td>
      <td>30.333333</td>
      <td>34.000000</td>
      <td>41.299065</td>
      <td>41.904762</td>
      <td>42.746479</td>
      <td>38.133333</td>
      <td>58.181818</td>
      <td>60.000000</td>
    </tr>
    <tr>
      <th>single</th>
      <td>32.117284</td>
      <td>34.150000</td>
      <td>31.934426</td>
      <td>34.897436</td>
      <td>35.117647</td>
      <td>38.000000</td>
      <td>40.769231</td>
      <td>41.333333</td>
      <td>32.763636</td>
      <td>36.875000</td>
      <td>...</td>
      <td>30.846154</td>
      <td>31.950000</td>
      <td>24.174468</td>
      <td>26.965517</td>
      <td>31.995902</td>
      <td>32.657143</td>
      <td>32.355556</td>
      <td>27.666667</td>
      <td>31.000000</td>
      <td>28.000000</td>
    </tr>
    <tr>
      <th>unknown</th>
      <td>42.500000</td>
      <td>0.000000</td>
      <td>37.000000</td>
      <td>0.000000</td>
      <td>31.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>30.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>45.000000</td>
      <td>32.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 24 columns</p>
</div>



# pivot_table를 사용한 주제2 분석
#### 같은 상품을 새로운 고객에게 마케팅 하려고한다.
#### 연령과 상품가입여부, 직업을 함께 고려할때 마케팅 전략을 변화시켜야 할 그룹은?



```python
#pivot_table
pivot=pd.pivot_table(df,values='age',index='y',columns='job',aggfunc='mean')
```


```python
#pivot 변수 출력
pivot
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
      <th>job</th>
      <th>admin.</th>
      <th>blue-collar</th>
      <th>entrepreneur</th>
      <th>housemaid</th>
      <th>management</th>
      <th>retired</th>
      <th>self-employed</th>
      <th>services</th>
      <th>student</th>
      <th>technician</th>
      <th>unemployed</th>
      <th>unknown</th>
    </tr>
    <tr>
      <th>y</th>
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
      <th>no</th>
      <td>38.219846</td>
      <td>39.582057</td>
      <td>41.703453</td>
      <td>44.705451</td>
      <td>42.309707</td>
      <td>59.926128</td>
      <td>40.176887</td>
      <td>38.090236</td>
      <td>26.396667</td>
      <td>38.600033</td>
      <td>39.844828</td>
      <td>45.375427</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>37.968935</td>
      <td>39.200627</td>
      <td>41.935484</td>
      <td>52.650943</td>
      <td>42.783537</td>
      <td>68.253456</td>
      <td>38.006711</td>
      <td>36.077399</td>
      <td>24.800000</td>
      <td>37.746575</td>
      <td>39.062500</td>
      <td>47.054054</td>
    </tr>
  </tbody>
</table>
</div>




```python
#yes행과 no행의 차 연산(loc인덱서 사용)
pivot.loc['yes']-pivot.loc['no']
```




    job
    admin.          -0.250911
    blue-collar     -0.381430
    entrepreneur     0.232030
    housemaid        7.945493
    management       0.473829
    retired          8.327329
    self-employed   -2.170175
    services        -2.012836
    student         -1.596667
    technician      -0.853458
    unemployed      -0.782328
    unknown          1.678627
    dtype: float64




```python
#diff행 생성(yes행과 no행의 차)
pivot.loc['diff']=pivot.loc['yes']-pivot.loc['no']
```


```python
#pivot 변수 출력
pivot
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
      <th>job</th>
      <th>admin.</th>
      <th>blue-collar</th>
      <th>entrepreneur</th>
      <th>housemaid</th>
      <th>management</th>
      <th>retired</th>
      <th>self-employed</th>
      <th>services</th>
      <th>student</th>
      <th>technician</th>
      <th>unemployed</th>
      <th>unknown</th>
    </tr>
    <tr>
      <th>y</th>
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
      <th>no</th>
      <td>38.219846</td>
      <td>39.582057</td>
      <td>41.703453</td>
      <td>44.705451</td>
      <td>42.309707</td>
      <td>59.926128</td>
      <td>40.176887</td>
      <td>38.090236</td>
      <td>26.396667</td>
      <td>38.600033</td>
      <td>39.844828</td>
      <td>45.375427</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>37.968935</td>
      <td>39.200627</td>
      <td>41.935484</td>
      <td>52.650943</td>
      <td>42.783537</td>
      <td>68.253456</td>
      <td>38.006711</td>
      <td>36.077399</td>
      <td>24.800000</td>
      <td>37.746575</td>
      <td>39.062500</td>
      <td>47.054054</td>
    </tr>
    <tr>
      <th>diff</th>
      <td>-0.250911</td>
      <td>-0.381430</td>
      <td>0.232030</td>
      <td>7.945493</td>
      <td>0.473829</td>
      <td>8.327329</td>
      <td>-2.170175</td>
      <td>-2.012836</td>
      <td>-1.596667</td>
      <td>-0.853458</td>
      <td>-0.782328</td>
      <td>1.678627</td>
    </tr>
  </tbody>
</table>
</div>




```python
#diff 기준으로 내림차순 정렬 
#sort_values() - default : 열 기준 오름차순 정렬
#axis=1,ascending=False : 행 기준 내림차순 정렬
#axis=1 안넣어주면 오류 발생
result=pivot.sort_values('diff',axis=1,ascending=False)
```


```python
#result 출력
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
      <th>job</th>
      <th>retired</th>
      <th>housemaid</th>
      <th>unknown</th>
      <th>management</th>
      <th>entrepreneur</th>
      <th>admin.</th>
      <th>blue-collar</th>
      <th>unemployed</th>
      <th>technician</th>
      <th>student</th>
      <th>services</th>
      <th>self-employed</th>
    </tr>
    <tr>
      <th>y</th>
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
      <th>no</th>
      <td>59.926128</td>
      <td>44.705451</td>
      <td>45.375427</td>
      <td>42.309707</td>
      <td>41.703453</td>
      <td>38.219846</td>
      <td>39.582057</td>
      <td>39.844828</td>
      <td>38.600033</td>
      <td>26.396667</td>
      <td>38.090236</td>
      <td>40.176887</td>
    </tr>
    <tr>
      <th>yes</th>
      <td>68.253456</td>
      <td>52.650943</td>
      <td>47.054054</td>
      <td>42.783537</td>
      <td>41.935484</td>
      <td>37.968935</td>
      <td>39.200627</td>
      <td>39.062500</td>
      <td>37.746575</td>
      <td>24.800000</td>
      <td>36.077399</td>
      <td>38.006711</td>
    </tr>
    <tr>
      <th>diff</th>
      <td>8.327329</td>
      <td>7.945493</td>
      <td>1.678627</td>
      <td>0.473829</td>
      <td>0.232030</td>
      <td>-0.250911</td>
      <td>-0.381430</td>
      <td>-0.782328</td>
      <td>-0.853458</td>
      <td>-1.596667</td>
      <td>-2.012836</td>
      <td>-2.170175</td>
    </tr>
  </tbody>
</table>
</div>




```python
#result의 diff행 막대그래프 시각화
result.loc['diff'].plot.bar()
plt.show()
```



![output_95_0](./img/customerData/output_95_0.png)




```python
#result의 diff행 막대그래프 시각화
#figsize=[15,10]
#title('주제2 시각화',fontsize=20)
#x축 눈금 - fontsize=16,rotation=45
#y축 눈금 - fontsize=16)
#xlabel - 'job',fontsize=16
#ylabel - 'diff',fontsize=16

result.loc['diff'].plot.bar(figsize=[15,10])
plt.title('주제2 시각화',fontsize=20)
plt.xticks(fontsize=16,rotation=45)
plt.yticks(fontsize=16)
plt.xlabel('job',fontsize=16)
plt.ylabel('diff',fontsize=16)
plt.show()
```



![output_96_0](./img/customerData/output_96_0.png)



- 결론 - retired , housemaid 경우는 연령을 높여서 더욱 타겟팅 해야한다
