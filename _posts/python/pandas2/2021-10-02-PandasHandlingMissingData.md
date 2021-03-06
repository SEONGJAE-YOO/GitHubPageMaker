---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Pandas_Handling_Missing_Data
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-10-02
priority: 0.7  
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}


```python
import pandas as pd
import numpy as np
```


```python
df = pd.DataFrame([[np.nan, 2, np.nan, 0], [3, 4, np.nan, 1],
                   [np.nan, np.nan, np.nan, 5]],
                  columns=list('ABCD'))
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dropna(axis=1, how='all')
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
      <th>A</th>
      <th>B</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>2.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



![20210920_164126_1.png](attachment:20210920_164126_1.png)



dropna??? syntax??? ????????? ????????????.

DataFrame.dropna(axis=0/1, how='any'/'all', subset=[col1, col2, ...], inplace=True/False)
dropna??? ????????? ??? ?????? parameter?????? ??? ????????? ?????? ???????????? ????????? ???????????????.



axis = 0/1 or 'index'/'columns'

0 or 'index' -> NaN ?????? ????????? row??? drop (default ????????????.)

1 or 'columns' -> NaN ?????? ????????? column??? drop





how = 'any'/'all'

any -> row ?????? column??? NaN?????? 1?????? ????????? drop (default ????????????.)

all -> row ?????? column??? ?????? ?????? ?????? NaN????????? drop





inplace = True/False

True -> dropna??? ????????? DataFrame ????????? dropna??? ??????

False -> dropna??? ????????? DataFrame??? ????????? ?????? dropna??? ????????? DataFrame??? return





subset = [col1, col2, ...]

subset??? ???????????? ????????? DataFrame ??????(?????? column & ?????? row)??? ?????? dropna??? ??????

subset??? ???????????? subset??? ?????? column?????? ???????????? dropna??? ??????


# https://cosmosproject.tistory.com/308   ??????


```python
df.dropna(axis=1, how='any')
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
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.fillna(0) #NaN ?????? 0?????? ?????????
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
values = {'A': 0, 'B': 1, 'C': 2, 'D': 3}
df.fillna(value=values) # NaN?????? values ????????? ?????????
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
fill_na_value = df['D'].max()
fill_na_value

```




    5




```python
df.fillna(fill_na_value)

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.isnull().sum()

```




    A    2
    B    1
    C    3
    D    0
    dtype: int64




```python
df.notnull().sum()
```




    A    1
    B    2
    C    0
    D    3
    dtype: int64


