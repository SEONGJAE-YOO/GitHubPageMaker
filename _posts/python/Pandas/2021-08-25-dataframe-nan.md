---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: DataFrame NaN 데이터 처리
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
1. NaN 처리 방법 이해하기



```python
import pandas as pd
```


```python
# data 출처: https://www.kaggle.com/hesh97/titanicdataset-traincsv/data
train_data = pd.read_csv('./train.csv')
train_data.head()
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



#### NaN 값 확인
- info함수를 통하여 개수 확인
- isna함수를 통해 boolean 타입으로 확인


```python
train_data.info() #Cabin 204, Age 714 을 보아 데이터 손실이 있다는 것을 알수 있다
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 12 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   PassengerId  891 non-null    int64  
     1   Survived     891 non-null    int64  
     2   Pclass       891 non-null    int64  
     3   Name         891 non-null    object 
     4   Sex          891 non-null    object 
     5   Age          714 non-null    float64
     6   SibSp        891 non-null    int64  
     7   Parch        891 non-null    int64  
     8   Ticket       891 non-null    object 
     9   Fare         891 non-null    float64
     10  Cabin        204 non-null    object 
     11  Embarked     889 non-null    object 
    dtypes: float64(2), int64(5), object(5)
    memory usage: 83.7+ KB



```python
train_data.isna() # true 인 경우에 NaN , 즉 데이터가 없다는 의미
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
      <td>True</td>
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
      <td>True</td>
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
      <td>True</td>
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
    </tr>
    <tr>
      <th>886</th>
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
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>887</th>
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
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>888</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>889</th>
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
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>890</th>
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
      <td>True</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 12 columns</p>
</div>




```python
train_data['Age'].isna()
```




    0      False
    1      False
    2      False
    3      False
    4      False
           ...  
    886    False
    887    False
    888     True
    889    False
    890    False
    Name: Age, Length: 891, dtype: bool



#### NaN 처리 방법
- 데이터에서 삭제
    - dropna 함수
- 다른 값으로 치환
    - fillna 함수

* NaN 데이터 삭제하기


```python
train_data.dropna()
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
      <th>6</th>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>McCarthy, Mr. Timothy J</td>
      <td>male</td>
      <td>54.0</td>
      <td>0</td>
      <td>0</td>
      <td>17463</td>
      <td>51.8625</td>
      <td>E46</td>
      <td>S</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>1</td>
      <td>3</td>
      <td>Sandstrom, Miss. Marguerite Rut</td>
      <td>female</td>
      <td>4.0</td>
      <td>1</td>
      <td>1</td>
      <td>PP 9549</td>
      <td>16.7000</td>
      <td>G6</td>
      <td>S</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>1</td>
      <td>1</td>
      <td>Bonnell, Miss. Elizabeth</td>
      <td>female</td>
      <td>58.0</td>
      <td>0</td>
      <td>0</td>
      <td>113783</td>
      <td>26.5500</td>
      <td>C103</td>
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
      <th>871</th>
      <td>872</td>
      <td>1</td>
      <td>1</td>
      <td>Beckwith, Mrs. Richard Leonard (Sallie Monypeny)</td>
      <td>female</td>
      <td>47.0</td>
      <td>1</td>
      <td>1</td>
      <td>11751</td>
      <td>52.5542</td>
      <td>D35</td>
      <td>S</td>
    </tr>
    <tr>
      <th>872</th>
      <td>873</td>
      <td>0</td>
      <td>1</td>
      <td>Carlsson, Mr. Frans Olof</td>
      <td>male</td>
      <td>33.0</td>
      <td>0</td>
      <td>0</td>
      <td>695</td>
      <td>5.0000</td>
      <td>B51 B53 B55</td>
      <td>S</td>
    </tr>
    <tr>
      <th>879</th>
      <td>880</td>
      <td>1</td>
      <td>1</td>
      <td>Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)</td>
      <td>female</td>
      <td>56.0</td>
      <td>0</td>
      <td>1</td>
      <td>11767</td>
      <td>83.1583</td>
      <td>C50</td>
      <td>C</td>
    </tr>
    <tr>
      <th>887</th>
      <td>888</td>
      <td>1</td>
      <td>1</td>
      <td>Graham, Miss. Margaret Edith</td>
      <td>female</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>112053</td>
      <td>30.0000</td>
      <td>B42</td>
      <td>S</td>
    </tr>
    <tr>
      <th>889</th>
      <td>890</td>
      <td>1</td>
      <td>1</td>
      <td>Behr, Mr. Karl Howell</td>
      <td>male</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>111369</td>
      <td>30.0000</td>
      <td>C148</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
<p>183 rows × 12 columns</p>
</div>




```python
train_data.dropna(subset=['Age', 'Cabin']) #Age , Cabin 컬럼 에서 True 값이 있는 인덱스 행열 제거 한다
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
      <th>6</th>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>McCarthy, Mr. Timothy J</td>
      <td>male</td>
      <td>54.0</td>
      <td>0</td>
      <td>0</td>
      <td>17463</td>
      <td>51.8625</td>
      <td>E46</td>
      <td>S</td>
    </tr>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>1</td>
      <td>3</td>
      <td>Sandstrom, Miss. Marguerite Rut</td>
      <td>female</td>
      <td>4.0</td>
      <td>1</td>
      <td>1</td>
      <td>PP 9549</td>
      <td>16.7000</td>
      <td>G6</td>
      <td>S</td>
    </tr>
    <tr>
      <th>11</th>
      <td>12</td>
      <td>1</td>
      <td>1</td>
      <td>Bonnell, Miss. Elizabeth</td>
      <td>female</td>
      <td>58.0</td>
      <td>0</td>
      <td>0</td>
      <td>113783</td>
      <td>26.5500</td>
      <td>C103</td>
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
      <th>871</th>
      <td>872</td>
      <td>1</td>
      <td>1</td>
      <td>Beckwith, Mrs. Richard Leonard (Sallie Monypeny)</td>
      <td>female</td>
      <td>47.0</td>
      <td>1</td>
      <td>1</td>
      <td>11751</td>
      <td>52.5542</td>
      <td>D35</td>
      <td>S</td>
    </tr>
    <tr>
      <th>872</th>
      <td>873</td>
      <td>0</td>
      <td>1</td>
      <td>Carlsson, Mr. Frans Olof</td>
      <td>male</td>
      <td>33.0</td>
      <td>0</td>
      <td>0</td>
      <td>695</td>
      <td>5.0000</td>
      <td>B51 B53 B55</td>
      <td>S</td>
    </tr>
    <tr>
      <th>879</th>
      <td>880</td>
      <td>1</td>
      <td>1</td>
      <td>Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)</td>
      <td>female</td>
      <td>56.0</td>
      <td>0</td>
      <td>1</td>
      <td>11767</td>
      <td>83.1583</td>
      <td>C50</td>
      <td>C</td>
    </tr>
    <tr>
      <th>887</th>
      <td>888</td>
      <td>1</td>
      <td>1</td>
      <td>Graham, Miss. Margaret Edith</td>
      <td>female</td>
      <td>19.0</td>
      <td>0</td>
      <td>0</td>
      <td>112053</td>
      <td>30.0000</td>
      <td>B42</td>
      <td>S</td>
    </tr>
    <tr>
      <th>889</th>
      <td>890</td>
      <td>1</td>
      <td>1</td>
      <td>Behr, Mr. Karl Howell</td>
      <td>male</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>111369</td>
      <td>30.0000</td>
      <td>C148</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
<p>185 rows × 12 columns</p>
</div>




```python
train_data.dropna(axis=1) # axis = 0 은 row 제거 , axis =1 은 columns 제거
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
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
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
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
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
    </tr>
    <tr>
      <th>886</th>
      <td>887</td>
      <td>0</td>
      <td>2</td>
      <td>Montvila, Rev. Juozas</td>
      <td>male</td>
      <td>0</td>
      <td>0</td>
      <td>211536</td>
      <td>13.0000</td>
    </tr>
    <tr>
      <th>887</th>
      <td>888</td>
      <td>1</td>
      <td>1</td>
      <td>Graham, Miss. Margaret Edith</td>
      <td>female</td>
      <td>0</td>
      <td>0</td>
      <td>112053</td>
      <td>30.0000</td>
    </tr>
    <tr>
      <th>888</th>
      <td>889</td>
      <td>0</td>
      <td>3</td>
      <td>Johnston, Miss. Catherine Helen "Carrie"</td>
      <td>female</td>
      <td>1</td>
      <td>2</td>
      <td>W./C. 6607</td>
      <td>23.4500</td>
    </tr>
    <tr>
      <th>889</th>
      <td>890</td>
      <td>1</td>
      <td>1</td>
      <td>Behr, Mr. Karl Howell</td>
      <td>male</td>
      <td>0</td>
      <td>0</td>
      <td>111369</td>
      <td>30.0000</td>
    </tr>
    <tr>
      <th>890</th>
      <td>891</td>
      <td>0</td>
      <td>3</td>
      <td>Dooley, Mr. Patrick</td>
      <td>male</td>
      <td>0</td>
      <td>0</td>
      <td>370376</td>
      <td>7.7500</td>
    </tr>
  </tbody>
</table>
<p>891 rows × 9 columns</p>
</div>



* NaN 값 대체하기
- 평균으로 대체하기
- 생존자/사망자 별 평균으로 대체하기


```python
train_data['Age'].mean()
```




    29.69911764705882




```python
train_data['Age'].fillna(train_data['Age'].mean()) # Age 컬럼 888번째 NaN 값을 전체 평균 값으로 대체한다(29.69911764705882)
```




    0      22.000000
    1      38.000000
    2      26.000000
    3      35.000000
    4      35.000000
             ...    
    886    27.000000
    887    19.000000
    888    29.699118
    889    26.000000
    890    32.000000
    Name: Age, Length: 891, dtype: float64




```python
train_data['Survived'] == 1 # 생존자 확인 
```




    0      False
    1       True
    2       True
    3       True
    4      False
           ...  
    886    False
    887     True
    888    False
    889     True
    890    False
    Name: Survived, Length: 891, dtype: bool




```python
train_data[train_data['Survived'] == 1]['Age'] # 생존자 나이 추출
```




    1      38.0
    2      26.0
    3      35.0
    8      27.0
    9      14.0
           ... 
    875    15.0
    879    56.0
    880    25.0
    887    19.0
    889    26.0
    Name: Age, Length: 342, dtype: float64




```python
# 생존자 나이 평균
mean1 = train_data[train_data['Survived'] == 1]['Age'].mean()

# 사망자 나이 평균
mean0 = train_data[train_data['Survived'] == 0]['Age'].mean()

print(mean1, mean0)
```

    28.343689655172415 30.62617924528302



```python
train_data[train_data['Survived'] == 1]['Age'].fillna(mean1) # 생존자 나이 중에 NaN 값이 28.343689655172415으로 대체됨 
train_data[train_data['Survived'] == 0]['Age'].fillna(mean0) # 사망자 나이 중에 NaN 값이 30.62617924528302 으로 대체됨
```




    0      22.000000
    4      35.000000
    5      30.626179
    6      54.000000
    7       2.000000
             ...    
    884    25.000000
    885    39.000000
    886    27.000000
    888    30.626179
    890    32.000000
    Name: Age, Length: 549, dtype: float64




```python
#실제 데이터에 채우고 싶다면 loc 이용하기
train_data.loc[train_data['Survived'] == 1, 'Age'] = train_data[train_data['Survived'] == 1]['Age'].fillna(mean1)
train_data.loc[train_data['Survived'] == 0, 'Age'] = train_data[train_data['Survived'] == 0]['Age'].fillna(mean0)
```


```python
train_data
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
      <td>22.000000</td>
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
      <td>38.000000</td>
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
      <td>26.000000</td>
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
      <td>35.000000</td>
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
      <td>35.000000</td>
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
      <td>887</td>
      <td>0</td>
      <td>2</td>
      <td>Montvila, Rev. Juozas</td>
      <td>male</td>
      <td>27.000000</td>
      <td>0</td>
      <td>0</td>
      <td>211536</td>
      <td>13.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>887</th>
      <td>888</td>
      <td>1</td>
      <td>1</td>
      <td>Graham, Miss. Margaret Edith</td>
      <td>female</td>
      <td>19.000000</td>
      <td>0</td>
      <td>0</td>
      <td>112053</td>
      <td>30.0000</td>
      <td>B42</td>
      <td>S</td>
    </tr>
    <tr>
      <th>888</th>
      <td>889</td>
      <td>0</td>
      <td>3</td>
      <td>Johnston, Miss. Catherine Helen "Carrie"</td>
      <td>female</td>
      <td>30.626179</td>
      <td>1</td>
      <td>2</td>
      <td>W./C. 6607</td>
      <td>23.4500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>889</th>
      <td>890</td>
      <td>1</td>
      <td>1</td>
      <td>Behr, Mr. Karl Howell</td>
      <td>male</td>
      <td>26.000000</td>
      <td>0</td>
      <td>0</td>
      <td>111369</td>
      <td>30.0000</td>
      <td>C148</td>
      <td>C</td>
    </tr>
    <tr>
      <th>890</th>
      <td>891</td>
      <td>0</td>
      <td>3</td>
      <td>Dooley, Mr. Patrick</td>
      <td>male</td>
      <td>32.000000</td>
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
train_data[train_data['Age'] == 28.343689655172415] #생존자만 보기 / NaN 값이 바뀐 것을 확인 할수 있다
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
      <th>17</th>
      <td>18</td>
      <td>1</td>
      <td>2</td>
      <td>Williams, Mr. Charles Eugene</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>244373</td>
      <td>13.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>19</th>
      <td>20</td>
      <td>1</td>
      <td>3</td>
      <td>Masselmani, Mrs. Fatima</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>2649</td>
      <td>7.2250</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>28</th>
      <td>29</td>
      <td>1</td>
      <td>3</td>
      <td>O'Dwyer, Miss. Ellen "Nellie"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>330959</td>
      <td>7.8792</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>31</th>
      <td>32</td>
      <td>1</td>
      <td>1</td>
      <td>Spencer, Mrs. William Augustus (Marie Eugenie)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17569</td>
      <td>146.5208</td>
      <td>B78</td>
      <td>C</td>
    </tr>
    <tr>
      <th>32</th>
      <td>33</td>
      <td>1</td>
      <td>3</td>
      <td>Glynn, Miss. Mary Agatha</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>335677</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>36</th>
      <td>37</td>
      <td>1</td>
      <td>3</td>
      <td>Mamee, Mr. Hanna</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>2677</td>
      <td>7.2292</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>47</th>
      <td>48</td>
      <td>1</td>
      <td>3</td>
      <td>O'Driscoll, Miss. Bridget</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>14311</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>55</th>
      <td>56</td>
      <td>1</td>
      <td>1</td>
      <td>Woolner, Mr. Hugh</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>19947</td>
      <td>35.5000</td>
      <td>C52</td>
      <td>S</td>
    </tr>
    <tr>
      <th>65</th>
      <td>66</td>
      <td>1</td>
      <td>3</td>
      <td>Moubarek, Master. Gerios</td>
      <td>male</td>
      <td>28.34369</td>
      <td>1</td>
      <td>1</td>
      <td>2661</td>
      <td>15.2458</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>82</th>
      <td>83</td>
      <td>1</td>
      <td>3</td>
      <td>McDermott, Miss. Brigdet Delia</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>330932</td>
      <td>7.7875</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>107</th>
      <td>108</td>
      <td>1</td>
      <td>3</td>
      <td>Moss, Mr. Albert Johan</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>312991</td>
      <td>7.7750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>109</th>
      <td>110</td>
      <td>1</td>
      <td>3</td>
      <td>Moran, Miss. Bertha</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>371110</td>
      <td>24.1500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>128</th>
      <td>129</td>
      <td>1</td>
      <td>3</td>
      <td>Peter, Miss. Anna</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>1</td>
      <td>2668</td>
      <td>22.3583</td>
      <td>F E69</td>
      <td>C</td>
    </tr>
    <tr>
      <th>166</th>
      <td>167</td>
      <td>1</td>
      <td>1</td>
      <td>Chibnall, Mrs. (Edith Martha Bowerman)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>1</td>
      <td>113505</td>
      <td>55.0000</td>
      <td>E33</td>
      <td>S</td>
    </tr>
    <tr>
      <th>186</th>
      <td>187</td>
      <td>1</td>
      <td>3</td>
      <td>O'Brien, Mrs. Thomas (Johanna "Hannah" Godfrey)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>370365</td>
      <td>15.5000</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>198</th>
      <td>199</td>
      <td>1</td>
      <td>3</td>
      <td>Madigan, Miss. Margaret "Maggie"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>370370</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>241</th>
      <td>242</td>
      <td>1</td>
      <td>3</td>
      <td>Murphy, Miss. Katherine "Kate"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>367230</td>
      <td>15.5000</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>256</th>
      <td>257</td>
      <td>1</td>
      <td>1</td>
      <td>Thorne, Mrs. Gertrude Maybelle</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17585</td>
      <td>79.2000</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>274</th>
      <td>275</td>
      <td>1</td>
      <td>3</td>
      <td>Healy, Miss. Hanora "Nora"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>370375</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>298</th>
      <td>299</td>
      <td>1</td>
      <td>1</td>
      <td>Saalfeld, Mr. Adolphe</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>19988</td>
      <td>30.5000</td>
      <td>C106</td>
      <td>S</td>
    </tr>
    <tr>
      <th>300</th>
      <td>301</td>
      <td>1</td>
      <td>3</td>
      <td>Kelly, Miss. Anna Katherine "Annie Kate"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>9234</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>301</th>
      <td>302</td>
      <td>1</td>
      <td>3</td>
      <td>McCoy, Mr. Bernard</td>
      <td>male</td>
      <td>28.34369</td>
      <td>2</td>
      <td>0</td>
      <td>367226</td>
      <td>23.2500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>303</th>
      <td>304</td>
      <td>1</td>
      <td>2</td>
      <td>Keane, Miss. Nora A</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>226593</td>
      <td>12.3500</td>
      <td>E101</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>306</th>
      <td>307</td>
      <td>1</td>
      <td>1</td>
      <td>Fleming, Miss. Margaret</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>17421</td>
      <td>110.8833</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>330</th>
      <td>331</td>
      <td>1</td>
      <td>3</td>
      <td>McCoy, Miss. Agnes</td>
      <td>female</td>
      <td>28.34369</td>
      <td>2</td>
      <td>0</td>
      <td>367226</td>
      <td>23.2500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>334</th>
      <td>335</td>
      <td>1</td>
      <td>1</td>
      <td>Frauenthal, Mrs. Henry William (Clara Heinshei...</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17611</td>
      <td>133.6500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>347</th>
      <td>348</td>
      <td>1</td>
      <td>3</td>
      <td>Davison, Mrs. Thomas Henry (Mary E Finck)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>386525</td>
      <td>16.1000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>358</th>
      <td>359</td>
      <td>1</td>
      <td>3</td>
      <td>McGovern, Miss. Mary</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>330931</td>
      <td>7.8792</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>359</th>
      <td>360</td>
      <td>1</td>
      <td>3</td>
      <td>Mockler, Miss. Helen Mary "Ellie"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>330980</td>
      <td>7.8792</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>367</th>
      <td>368</td>
      <td>1</td>
      <td>3</td>
      <td>Moussa, Mrs. (Mantoura Boulos)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>2626</td>
      <td>7.2292</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>368</th>
      <td>369</td>
      <td>1</td>
      <td>3</td>
      <td>Jermyn, Miss. Annie</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>14313</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>375</th>
      <td>376</td>
      <td>1</td>
      <td>1</td>
      <td>Meyer, Mrs. Edgar Joseph (Leila Saks)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17604</td>
      <td>82.1708</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>431</th>
      <td>432</td>
      <td>1</td>
      <td>3</td>
      <td>Thorneycroft, Mrs. Percival (Florence Kate White)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>376564</td>
      <td>16.1000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>444</th>
      <td>445</td>
      <td>1</td>
      <td>3</td>
      <td>Johannesen-Bratthammer, Mr. Bernt</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>65306</td>
      <td>8.1125</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>457</th>
      <td>458</td>
      <td>1</td>
      <td>1</td>
      <td>Kenyon, Mrs. Frederick R (Marion)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>17464</td>
      <td>51.8625</td>
      <td>D21</td>
      <td>S</td>
    </tr>
    <tr>
      <th>507</th>
      <td>508</td>
      <td>1</td>
      <td>1</td>
      <td>Bradley, Mr. George ("George Arthur Brayton")</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>111427</td>
      <td>26.5500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>533</th>
      <td>534</td>
      <td>1</td>
      <td>3</td>
      <td>Peter, Mrs. Catherine (Catherine Rizk)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>2</td>
      <td>2668</td>
      <td>22.3583</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>547</th>
      <td>548</td>
      <td>1</td>
      <td>2</td>
      <td>Padro y Manent, Mr. Julian</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>SC/PARIS 2146</td>
      <td>13.8625</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>573</th>
      <td>574</td>
      <td>1</td>
      <td>3</td>
      <td>Kelly, Miss. Mary</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>14312</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>596</th>
      <td>597</td>
      <td>1</td>
      <td>2</td>
      <td>Leitch, Miss. Jessie Wills</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>248727</td>
      <td>33.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>612</th>
      <td>613</td>
      <td>1</td>
      <td>3</td>
      <td>Murphy, Miss. Margaret Jane</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>367230</td>
      <td>15.5000</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>643</th>
      <td>644</td>
      <td>1</td>
      <td>3</td>
      <td>Foo, Mr. Choong</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>1601</td>
      <td>56.4958</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>653</th>
      <td>654</td>
      <td>1</td>
      <td>3</td>
      <td>O'Leary, Miss. Hanora "Norah"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>330919</td>
      <td>7.8292</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>669</th>
      <td>670</td>
      <td>1</td>
      <td>1</td>
      <td>Taylor, Mrs. Elmer Zebley (Juliet Cummins Wright)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>19996</td>
      <td>52.0000</td>
      <td>C126</td>
      <td>S</td>
    </tr>
    <tr>
      <th>692</th>
      <td>693</td>
      <td>1</td>
      <td>3</td>
      <td>Lam, Mr. Ali</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>1601</td>
      <td>56.4958</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>697</th>
      <td>698</td>
      <td>1</td>
      <td>3</td>
      <td>Mullens, Miss. Katherine "Katie"</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>35852</td>
      <td>7.7333</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>709</th>
      <td>710</td>
      <td>1</td>
      <td>3</td>
      <td>Moubarek, Master. Halim Gonios ("William George")</td>
      <td>male</td>
      <td>28.34369</td>
      <td>1</td>
      <td>1</td>
      <td>2661</td>
      <td>15.2458</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>727</th>
      <td>728</td>
      <td>1</td>
      <td>3</td>
      <td>Mannion, Miss. Margareth</td>
      <td>female</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>36866</td>
      <td>7.7375</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>740</th>
      <td>741</td>
      <td>1</td>
      <td>1</td>
      <td>Hawksford, Mr. Walter James</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>16988</td>
      <td>30.0000</td>
      <td>D45</td>
      <td>S</td>
    </tr>
    <tr>
      <th>828</th>
      <td>829</td>
      <td>1</td>
      <td>3</td>
      <td>McCormack, Mr. Thomas Joseph</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>367228</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>839</th>
      <td>840</td>
      <td>1</td>
      <td>1</td>
      <td>Marechal, Mr. Pierre</td>
      <td>male</td>
      <td>28.34369</td>
      <td>0</td>
      <td>0</td>
      <td>11774</td>
      <td>29.7000</td>
      <td>C47</td>
      <td>C</td>
    </tr>
    <tr>
      <th>849</th>
      <td>850</td>
      <td>1</td>
      <td>1</td>
      <td>Goldenberg, Mrs. Samuel L (Edwiga Grabowska)</td>
      <td>female</td>
      <td>28.34369</td>
      <td>1</td>
      <td>0</td>
      <td>17453</td>
      <td>89.1042</td>
      <td>C92</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
</div>




```python
train_data[train_data['Age'] == 30.62617924528302] #사망자만 보기 / NaN 값이 바뀐 것을 확인 할수 있다
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
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>330877</td>
      <td>8.4583</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>26</th>
      <td>27</td>
      <td>0</td>
      <td>3</td>
      <td>Emir, Mr. Farred Chehab</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>2631</td>
      <td>7.2250</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>29</th>
      <td>30</td>
      <td>0</td>
      <td>3</td>
      <td>Todoroff, Mr. Lalio</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>349216</td>
      <td>7.8958</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>42</th>
      <td>43</td>
      <td>0</td>
      <td>3</td>
      <td>Kraeff, Mr. Theodor</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>349253</td>
      <td>7.8958</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>45</th>
      <td>46</td>
      <td>0</td>
      <td>3</td>
      <td>Rogers, Mr. William John</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>S.C./A.4. 23567</td>
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
      <th>859</th>
      <td>860</td>
      <td>0</td>
      <td>3</td>
      <td>Razi, Mr. Raihed</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>2629</td>
      <td>7.2292</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>863</th>
      <td>864</td>
      <td>0</td>
      <td>3</td>
      <td>Sage, Miss. Dorothy Edith "Dolly"</td>
      <td>female</td>
      <td>30.626179</td>
      <td>8</td>
      <td>2</td>
      <td>CA. 2343</td>
      <td>69.5500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>868</th>
      <td>869</td>
      <td>0</td>
      <td>3</td>
      <td>van Melkebeke, Mr. Philemon</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>345777</td>
      <td>9.5000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>878</th>
      <td>879</td>
      <td>0</td>
      <td>3</td>
      <td>Laleff, Mr. Kristo</td>
      <td>male</td>
      <td>30.626179</td>
      <td>0</td>
      <td>0</td>
      <td>349217</td>
      <td>7.8958</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>888</th>
      <td>889</td>
      <td>0</td>
      <td>3</td>
      <td>Johnston, Miss. Catherine Helen "Carrie"</td>
      <td>female</td>
      <td>30.626179</td>
      <td>1</td>
      <td>2</td>
      <td>W./C. 6607</td>
      <td>23.4500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
<p>125 rows × 12 columns</p>
</div>



