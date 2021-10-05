---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Summarize_Data
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-10-05
priority: 0.7  
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}


![20211005](./img/pandas/20211005.png)

```python
import pandas as pd
import seaborn as sns
import numpy as np
```


```python
df = sns.load_dataset('iris')
df.shape
```




    (150, 5)




```python
df.head(2)
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['species'].value_counts()
```




    setosa        50
    versicolor    50
    virginica     50
    Name: species, dtype: int64




```python
df["sepal_length"].value_counts()

```




    5.0    10
    5.1     9
    6.3     9
    5.7     8
    6.7     8
    5.8     7
    5.5     7
    6.4     7
    4.9     6
    5.4     6
    6.1     6
    6.0     6
    5.6     6
    4.8     5
    6.5     5
    6.2     4
    7.7     4
    6.9     4
    4.6     4
    5.2     4
    5.9     3
    4.4     3
    7.2     3
    6.8     3
    6.6     2
    4.7     2
    7.6     1
    7.4     1
    7.3     1
    7.0     1
    7.1     1
    5.3     1
    4.3     1
    4.5     1
    7.9     1
    Name: sepal_length, dtype: int64




```python
len(df)
```




    150




```python
df.shape #150행, 5열
```




    (150, 5)




```python
df['species'].nunique()
```




    3




```python
df.describe(include='all')
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>150.000000</td>
      <td>150.000000</td>
      <td>150.000000</td>
      <td>150.000000</td>
      <td>150</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>50</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>5.843333</td>
      <td>3.057333</td>
      <td>3.758000</td>
      <td>1.199333</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.828066</td>
      <td>0.435866</td>
      <td>1.765298</td>
      <td>0.762238</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>min</th>
      <td>4.300000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>0.100000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>5.100000</td>
      <td>2.800000</td>
      <td>1.600000</td>
      <td>0.300000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5.800000</td>
      <td>3.000000</td>
      <td>4.350000</td>
      <td>1.300000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>6.400000</td>
      <td>3.300000</td>
      <td>5.100000</td>
      <td>1.800000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7.900000</td>
      <td>4.400000</td>
      <td>6.900000</td>
      <td>2.500000</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['petal_width'].sum()
```




    179.90000000000003




```python
df['petal_width'].count() #행의 수
```




    150




```python
df.median()
```

    <ipython-input-12-6d467abf240d>:1: FutureWarning: Dropping of nuisance columns in DataFrame reductions (with 'numeric_only=None') is deprecated; in a future version this will raise TypeError.  Select only valid columns before calling the reduction.
      df.median()
    




    sepal_length    5.80
    sepal_width     3.00
    petal_length    4.35
    petal_width     1.30
    dtype: float64




```python
df.mean()
```

    <ipython-input-13-c61f0c8f89b5>:1: FutureWarning: Dropping of nuisance columns in DataFrame reductions (with 'numeric_only=None') is deprecated; in a future version this will raise TypeError.  Select only valid columns before calling the reduction.
      df.mean()
    




    sepal_length    5.843333
    sepal_width     3.057333
    petal_length    3.758000
    petal_width     1.199333
    dtype: float64




```python
df['petal_width'].quantile([0.25,0.75])
```




    0.25    0.3
    0.75    1.8
    Name: petal_width, dtype: float64




```python
df.quantile([0.25,0.75])

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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.25</th>
      <td>5.1</td>
      <td>2.8</td>
      <td>1.6</td>
      <td>0.3</td>
    </tr>
    <tr>
      <th>0.75</th>
      <td>6.4</td>
      <td>3.3</td>
      <td>5.1</td>
      <td>1.8</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.min()
```




    sepal_length       4.3
    sepal_width        2.0
    petal_length       1.0
    petal_width        0.1
    species         setosa
    dtype: object




```python
df.max()
```




    sepal_length          7.9
    sepal_width           4.4
    petal_length          6.9
    petal_width           2.5
    species         virginica
    dtype: object




```python
df.var()
```

    <ipython-input-18-28ded241fd7c>:1: FutureWarning: Dropping of nuisance columns in DataFrame reductions (with 'numeric_only=None') is deprecated; in a future version this will raise TypeError.  Select only valid columns before calling the reduction.
      df.var()
    




    sepal_length    0.685694
    sepal_width     0.189979
    petal_length    3.116278
    petal_width     0.581006
    dtype: float64




```python
df.std()
```

    <ipython-input-19-ce97bb7eaef8>:1: FutureWarning: Dropping of nuisance columns in DataFrame reductions (with 'numeric_only=None') is deprecated; in a future version this will raise TypeError.  Select only valid columns before calling the reduction.
      df.std()
    




    sepal_length    0.828066
    sepal_width     0.435866
    petal_length    1.765298
    petal_width     0.762238
    dtype: float64



# apply(함수)


```python
def smp(x):
    #뒤에서 3번째 까지의 문자를 가져오는 함수
    x = x[-3:]
    return x
```


```python
df['species_3'] = df['species'].apply(lambda x : x[:3])
```


```python
df['species_3'] = df['species'].apply(smp)
```


```python
df
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
      <th>species_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>osa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>osa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>osa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>osa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
      <td>osa</td>
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
      <th>145</th>
      <td>6.7</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.3</td>
      <td>virginica</td>
      <td>ica</td>
    </tr>
    <tr>
      <th>146</th>
      <td>6.3</td>
      <td>2.5</td>
      <td>5.0</td>
      <td>1.9</td>
      <td>virginica</td>
      <td>ica</td>
    </tr>
    <tr>
      <th>147</th>
      <td>6.5</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.0</td>
      <td>virginica</td>
      <td>ica</td>
    </tr>
    <tr>
      <th>148</th>
      <td>6.2</td>
      <td>3.4</td>
      <td>5.4</td>
      <td>2.3</td>
      <td>virginica</td>
      <td>ica</td>
    </tr>
    <tr>
      <th>149</th>
      <td>5.9</td>
      <td>3.0</td>
      <td>5.1</td>
      <td>1.8</td>
      <td>virginica</td>
      <td>ica</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 6 columns</p>
</div>


