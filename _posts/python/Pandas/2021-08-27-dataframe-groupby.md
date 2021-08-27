---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: DataFrame group by 이해하기
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
1. DataFrame groupby 이해하기


```python
import pandas as pd
import numpy as np
```

#### group by
+ 아래의 세 단계를 적용하여 데이터를 그룹화(groupping) (SQL의 group by 와 개념적으로는 동일, 사용법은 유사)
    - 데이터 분할
    - operation 적용
    - 데이터 병합


```python
# data 출처: https://www.kaggle.com/hesh97/titanicdataset-traincsv/data
df = pd.read_csv('./train.csv')
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



#### GroupBy groups 속성
- 각 그룹과 그룹에 속한 index를 dict 형태로 표현


```python
class_group = df.groupby('Pclass')
class_group
```




    <pandas.core.groupby.generic.DataFrameGroupBy object at 0x0000018605571DC0>




```python
class_group.groups
```




    {1: [1, 3, 6, 11, 23, 27, 30, 31, 34, 35, 52, 54, 55, 61, 62, 64, 83, 88, 92, 96, 97, 102, 110, 118, 124, 136, 137, 139, 151, 155, 166, 168, 170, 174, 177, 185, 187, 194, 195, 209, 215, 218, 224, 230, 245, 248, 252, 256, 257, 258, 262, 263, 268, 269, 270, 273, 275, 284, 290, 291, 295, 297, 298, 299, 305, 306, 307, 309, 310, 311, 318, 319, 325, 329, 331, 332, 334, 336, 337, 339, 341, 351, 356, 366, 369, 370, 373, 375, 377, 380, 383, 390, 393, 412, 430, 434, 435, 438, 445, 447, ...], 2: [9, 15, 17, 20, 21, 33, 41, 43, 53, 56, 58, 66, 70, 72, 78, 84, 98, 99, 117, 120, 122, 123, 133, 134, 135, 144, 145, 148, 149, 150, 161, 178, 181, 183, 190, 191, 193, 199, 211, 213, 217, 219, 221, 226, 228, 232, 234, 236, 237, 238, 239, 242, 247, 249, 259, 265, 272, 277, 288, 292, 303, 308, 312, 314, 316, 317, 322, 323, 327, 340, 342, 343, 344, 345, 346, 357, 361, 385, 387, 389, 397, 398, 399, 405, 407, 413, 416, 417, 418, 426, 427, 432, 437, 439, 440, 443, 446, 450, 458, 463, ...], 3: [0, 2, 4, 5, 7, 8, 10, 12, 13, 14, 16, 18, 19, 22, 24, 25, 26, 28, 29, 32, 36, 37, 38, 39, 40, 42, 44, 45, 46, 47, 48, 49, 50, 51, 57, 59, 60, 63, 65, 67, 68, 69, 71, 73, 74, 75, 76, 77, 79, 80, 81, 82, 85, 86, 87, 89, 90, 91, 93, 94, 95, 100, 101, 103, 104, 105, 106, 107, 108, 109, 111, 112, 113, 114, 115, 116, 119, 121, 125, 126, 127, 128, 129, 130, 131, 132, 138, 140, 141, 142, 143, 146, 147, 152, 153, 154, 156, 157, 158, 159, ...]}




```python
gender_group = df.groupby('Sex')
gender_group.groups
```




    {'female': [1, 2, 3, 8, 9, 10, 11, 14, 15, 18, 19, 22, 24, 25, 28, 31, 32, 38, 39, 40, 41, 43, 44, 47, 49, 52, 53, 56, 58, 61, 66, 68, 71, 79, 82, 84, 85, 88, 98, 100, 106, 109, 111, 113, 114, 119, 123, 128, 132, 133, 136, 140, 141, 142, 147, 151, 156, 161, 166, 167, 172, 177, 180, 184, 186, 190, 192, 194, 195, 198, 199, 205, 208, 211, 215, 216, 218, 229, 230, 233, 235, 237, 240, 241, 246, 247, 251, 254, 255, 256, 257, 258, 259, 264, 268, 269, 272, 274, 275, 276, ...], 'male': [0, 4, 5, 6, 7, 12, 13, 16, 17, 20, 21, 23, 26, 27, 29, 30, 33, 34, 35, 36, 37, 42, 45, 46, 48, 50, 51, 54, 55, 57, 59, 60, 62, 63, 64, 65, 67, 69, 70, 72, 73, 74, 75, 76, 77, 78, 80, 81, 83, 86, 87, 89, 90, 91, 92, 93, 94, 95, 96, 97, 99, 101, 102, 103, 104, 105, 107, 108, 110, 112, 115, 116, 117, 118, 120, 121, 122, 124, 125, 126, 127, 129, 130, 131, 134, 135, 137, 138, 139, 143, 144, 145, 146, 148, 149, 150, 152, 153, 154, 155, ...]}



#### groupping 함수
- 그룹 데이터에 적용 가능한 통계 함수(NaN은 제외하여 연산)
- count - 데이터 개수
- sum   - 데이터의 합
- mean, std, var - 평균, 표준편차, 분산
- min, max - 최소, 최대값


```python
class_group.count()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>Pclass</th>
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
      <th>1</th>
      <td>216</td>
      <td>216</td>
      <td>216</td>
      <td>216</td>
      <td>186</td>
      <td>216</td>
      <td>216</td>
      <td>216</td>
      <td>216</td>
      <td>176</td>
      <td>214</td>
    </tr>
    <tr>
      <th>2</th>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>173</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>16</td>
      <td>184</td>
    </tr>
    <tr>
      <th>3</th>
      <td>491</td>
      <td>491</td>
      <td>491</td>
      <td>491</td>
      <td>355</td>
      <td>491</td>
      <td>491</td>
      <td>491</td>
      <td>491</td>
      <td>12</td>
      <td>491</td>
    </tr>
  </tbody>
</table>
</div>




```python
class_group.mean()['Survived']
```




    Pclass
    1    0.629630
    2    0.472826
    3    0.242363
    Name: Survived, dtype: float64



* 성별에 따른 생존율 구해보기


```python
df.groupby('Sex').mean()['Survived']
```




    Sex
    female    0.742038
    male      0.188908
    Name: Survived, dtype: float64



#### 복수 columns로 groupping 하기
- groupby에 column 리스트를 전달
- 통계함수를 적용한 결과는 multiindex를 갖는 dataframe

* 클래스와 성별에 따른 생존률 구해보기


```python
df.groupby(['Pclass', 'Sex']).mean()['Survived']
```




    Pclass  Sex   
    1       female    0.968085
            male      0.368852
    2       female    0.921053
            male      0.157407
    3       female    0.500000
            male      0.135447
    Name: Survived, dtype: float64




```python
df.groupby(['Pclass', 'Sex']).mean()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th>Sex</th>
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
      <th rowspan="2" valign="top">1</th>
      <th>female</th>
      <td>469.212766</td>
      <td>0.968085</td>
      <td>34.611765</td>
      <td>0.553191</td>
      <td>0.457447</td>
      <td>106.125798</td>
    </tr>
    <tr>
      <th>male</th>
      <td>455.729508</td>
      <td>0.368852</td>
      <td>41.281386</td>
      <td>0.311475</td>
      <td>0.278689</td>
      <td>67.226127</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>female</th>
      <td>443.105263</td>
      <td>0.921053</td>
      <td>28.722973</td>
      <td>0.486842</td>
      <td>0.605263</td>
      <td>21.970121</td>
    </tr>
    <tr>
      <th>male</th>
      <td>447.962963</td>
      <td>0.157407</td>
      <td>30.740707</td>
      <td>0.342593</td>
      <td>0.222222</td>
      <td>19.741782</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">3</th>
      <th>female</th>
      <td>399.729167</td>
      <td>0.500000</td>
      <td>21.750000</td>
      <td>0.895833</td>
      <td>0.798611</td>
      <td>16.118810</td>
    </tr>
    <tr>
      <th>male</th>
      <td>455.515850</td>
      <td>0.135447</td>
      <td>26.507589</td>
      <td>0.498559</td>
      <td>0.224784</td>
      <td>12.661633</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby(['Pclass', 'Sex']).mean().loc[(2, 'female')] #Pclass 가 2인 평균값
```




    PassengerId    443.105263
    Survived         0.921053
    Age             28.722973
    SibSp            0.486842
    Parch            0.605263
    Fare            21.970121
    Name: (2, female), dtype: float64




```python
df.groupby(['Pclass', 'Sex']).mean().index
```




    MultiIndex([(1, 'female'),
                (1,   'male'),
                (2, 'female'),
                (2,   'male'),
                (3, 'female'),
                (3,   'male')],
               names=['Pclass', 'Sex'])



#### index를 이용한 group by
- index가 있는 경우, groupby 함수에 level 사용 가능
    - level은 index의 depth를 의미하며, 가장 왼쪽부터 0부터 증가

* **set_index** 함수
- column 데이터를 index 레벨로 변경
* **reset_index** 함수
- 인덱스 초기화


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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.set_index(['Pclass', 'Sex'])
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th>Sex</th>
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
      <th>3</th>
      <th>male</th>
      <td>1</td>
      <td>0</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <th>female</th>
      <td>2</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <th>female</th>
      <td>3</td>
      <td>1</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <th>female</th>
      <td>4</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <th>male</th>
      <td>5</td>
      <td>0</td>
      <td>Allen, Mr. William Henry</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>...</th>
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
    </tr>
    <tr>
      <th>2</th>
      <th>male</th>
      <td>887</td>
      <td>0</td>
      <td>Montvila, Rev. Juozas</td>
      <td>27.0</td>
      <td>0</td>
      <td>0</td>
      <td>211536</td>
      <td>13.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <th>female</th>
      <td>888</td>
      <td>1</td>
      <td>Graham, Miss. Margaret Edith</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>112053</td>
      <td>30.0000</td>
      <td>B42</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <th>female</th>
      <td>889</td>
      <td>0</td>
      <td>Johnston, Miss. Catherine Helen "Carrie"</td>
      <td>NaN</td>
      <td>1</td>
      <td>2</td>
      <td>W./C. 6607</td>
      <td>23.4500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <th>male</th>
      <td>890</td>
      <td>1</td>
      <td>Behr, Mr. Karl Howell</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>111369</td>
      <td>30.0000</td>
      <td>C148</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <th>male</th>
      <td>891</td>
      <td>0</td>
      <td>Dooley, Mr. Patrick</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>370376</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 10 columns</p>
</div>




```python
df.set_index(['Pclass', 'Sex']).reset_index()
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
      <th>Pclass</th>
      <th>Sex</th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>male</td>
      <td>1</td>
      <td>0</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>female</td>
      <td>2</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>female</td>
      <td>3</td>
      <td>1</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>female</td>
      <td>4</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>male</td>
      <td>5</td>
      <td>0</td>
      <td>Allen, Mr. William Henry</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
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
    </tr>
    <tr>
      <th>886</th>
      <td>2</td>
      <td>male</td>
      <td>887</td>
      <td>0</td>
      <td>Montvila, Rev. Juozas</td>
      <td>27.0</td>
      <td>0</td>
      <td>0</td>
      <td>211536</td>
      <td>13.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>887</th>
      <td>1</td>
      <td>female</td>
      <td>888</td>
      <td>1</td>
      <td>Graham, Miss. Margaret Edith</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>112053</td>
      <td>30.0000</td>
      <td>B42</td>
      <td>S</td>
    </tr>
    <tr>
      <th>888</th>
      <td>3</td>
      <td>female</td>
      <td>889</td>
      <td>0</td>
      <td>Johnston, Miss. Catherine Helen "Carrie"</td>
      <td>NaN</td>
      <td>1</td>
      <td>2</td>
      <td>W./C. 6607</td>
      <td>23.4500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>889</th>
      <td>1</td>
      <td>male</td>
      <td>890</td>
      <td>1</td>
      <td>Behr, Mr. Karl Howell</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>111369</td>
      <td>30.0000</td>
      <td>C148</td>
      <td>C</td>
    </tr>
    <tr>
      <th>890</th>
      <td>3</td>
      <td>male</td>
      <td>891</td>
      <td>0</td>
      <td>Dooley, Mr. Patrick</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>370376</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 12 columns</p>
</div>




```python
df.set_index('Age').groupby(level=0).mean()
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
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
    <tr>
      <th>Age</th>
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
      <th>0.42</th>
      <td>804.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>8.5167</td>
    </tr>
    <tr>
      <th>0.67</th>
      <td>756.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>14.5000</td>
    </tr>
    <tr>
      <th>0.75</th>
      <td>557.5</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>19.2583</td>
    </tr>
    <tr>
      <th>0.83</th>
      <td>455.5</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.5</td>
      <td>1.5</td>
      <td>23.8750</td>
    </tr>
    <tr>
      <th>0.92</th>
      <td>306.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>151.5500</td>
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
      <th>70.00</th>
      <td>709.5</td>
      <td>0.0</td>
      <td>1.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>40.7500</td>
    </tr>
    <tr>
      <th>70.50</th>
      <td>117.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.7500</td>
    </tr>
    <tr>
      <th>71.00</th>
      <td>295.5</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>42.0792</td>
    </tr>
    <tr>
      <th>74.00</th>
      <td>852.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.7750</td>
    </tr>
    <tr>
      <th>80.00</th>
      <td>631.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>30.0000</td>
    </tr>
  </tbody>
</table>
<p>88 rows × 6 columns</p>
</div>



#### 나이대별로 생존율 구하기


```python
import math
def age_categorize(age):
    if math.isnan(age):
        return -1
    return math.floor(age / 10) * 10
```


```python
df.set_index('Age').groupby(age_categorize).mean()['Survived']
```




    -1     0.293785
     0     0.612903
     10    0.401961
     20    0.350000
     30    0.437126
     40    0.382022
     50    0.416667
     60    0.315789
     70    0.000000
     80    1.000000
    Name: Survived, dtype: float64



#### MultiIndex를 이용한 groupping


```python
df.set_index(['Pclass', 'Sex']).groupby(level=[0, 1]).mean()['Age']
```




    Pclass  Sex   
    1       female    34.611765
            male      41.281386
    2       female    28.722973
            male      30.740707
    3       female    21.750000
            male      26.507589
    Name: Age, dtype: float64



#### aggregate(집계) 함수 사용하기
- groupby 결과에 집계함수를 적용하여 그룹별 데이터 확인 가능


```python
df.set_index(['Pclass', 'Sex']).groupby(level=[0, 1]).aggregate([np.mean, np.sum, np.max])
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
      <th></th>
      <th colspan="3" halign="left">PassengerId</th>
      <th colspan="3" halign="left">Survived</th>
      <th colspan="3" halign="left">Age</th>
      <th colspan="3" halign="left">SibSp</th>
      <th colspan="3" halign="left">Parch</th>
      <th colspan="3" halign="left">Fare</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>mean</th>
      <th>sum</th>
      <th>amax</th>
      <th>mean</th>
      <th>sum</th>
      <th>amax</th>
      <th>mean</th>
      <th>sum</th>
      <th>amax</th>
      <th>mean</th>
      <th>sum</th>
      <th>amax</th>
      <th>mean</th>
      <th>sum</th>
      <th>amax</th>
      <th>mean</th>
      <th>sum</th>
      <th>amax</th>
    </tr>
    <tr>
      <th>Pclass</th>
      <th>Sex</th>
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
      <th rowspan="2" valign="top">1</th>
      <th>female</th>
      <td>469.212766</td>
      <td>44106</td>
      <td>888</td>
      <td>0.968085</td>
      <td>91</td>
      <td>1</td>
      <td>34.611765</td>
      <td>2942.00</td>
      <td>63.0</td>
      <td>0.553191</td>
      <td>52</td>
      <td>3</td>
      <td>0.457447</td>
      <td>43</td>
      <td>2</td>
      <td>106.125798</td>
      <td>9975.8250</td>
      <td>512.3292</td>
    </tr>
    <tr>
      <th>male</th>
      <td>455.729508</td>
      <td>55599</td>
      <td>890</td>
      <td>0.368852</td>
      <td>45</td>
      <td>1</td>
      <td>41.281386</td>
      <td>4169.42</td>
      <td>80.0</td>
      <td>0.311475</td>
      <td>38</td>
      <td>3</td>
      <td>0.278689</td>
      <td>34</td>
      <td>4</td>
      <td>67.226127</td>
      <td>8201.5875</td>
      <td>512.3292</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">2</th>
      <th>female</th>
      <td>443.105263</td>
      <td>33676</td>
      <td>881</td>
      <td>0.921053</td>
      <td>70</td>
      <td>1</td>
      <td>28.722973</td>
      <td>2125.50</td>
      <td>57.0</td>
      <td>0.486842</td>
      <td>37</td>
      <td>3</td>
      <td>0.605263</td>
      <td>46</td>
      <td>3</td>
      <td>21.970121</td>
      <td>1669.7292</td>
      <td>65.0000</td>
    </tr>
    <tr>
      <th>male</th>
      <td>447.962963</td>
      <td>48380</td>
      <td>887</td>
      <td>0.157407</td>
      <td>17</td>
      <td>1</td>
      <td>30.740707</td>
      <td>3043.33</td>
      <td>70.0</td>
      <td>0.342593</td>
      <td>37</td>
      <td>2</td>
      <td>0.222222</td>
      <td>24</td>
      <td>2</td>
      <td>19.741782</td>
      <td>2132.1125</td>
      <td>73.5000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">3</th>
      <th>female</th>
      <td>399.729167</td>
      <td>57561</td>
      <td>889</td>
      <td>0.500000</td>
      <td>72</td>
      <td>1</td>
      <td>21.750000</td>
      <td>2218.50</td>
      <td>63.0</td>
      <td>0.895833</td>
      <td>129</td>
      <td>8</td>
      <td>0.798611</td>
      <td>115</td>
      <td>6</td>
      <td>16.118810</td>
      <td>2321.1086</td>
      <td>69.5500</td>
    </tr>
    <tr>
      <th>male</th>
      <td>455.515850</td>
      <td>158064</td>
      <td>891</td>
      <td>0.135447</td>
      <td>47</td>
      <td>1</td>
      <td>26.507589</td>
      <td>6706.42</td>
      <td>74.0</td>
      <td>0.498559</td>
      <td>173</td>
      <td>8</td>
      <td>0.224784</td>
      <td>78</td>
      <td>5</td>
      <td>12.661633</td>
      <td>4393.5865</td>
      <td>69.5500</td>
    </tr>
  </tbody>
</table>
</div>


