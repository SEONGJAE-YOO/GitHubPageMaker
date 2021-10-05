---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: pandas_Subset_Observations_(Rows)
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

```python
import pandas as pd
```


```python
import numpy as np
df = pd.DataFrame(
        {"a" : [4 ,5, 6, 6, np.nan],
        "b" : [7, 8, np.nan, 9, 9],
        "c" : [10, 11, 12, np.nan, 12]},
        index = pd.MultiIndex.from_tuples(
        [('d',1),('d',2),('e',2),('e',3),('e',4)],
        names=['n','v']))
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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>



# Subset Observations (Rows)
컬럼명에서 원하는 행 가져오기


```python
df[df['a'] <= 5]
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df['c'] >= 7]

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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = df.drop_duplicates(keep='last') #중복된 행 제거해줌
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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df["b"] != 7] #7과 같지 않는 것만 가져온다
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>d</th>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['a'].isin([5]) #isin은 리스트 형태로 들어가야한다
```




    n  v
    d  1    False
       2     True
    e  2    False
       3    False
       4    False
    Name: a, dtype: bool




```python
df.a.isin?
```


```python
pd.isnull(df) #null 값 확인
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['a'].isnull().sum()
```




    1




```python
pd.notnull(df) #널값 아닌 값 확인
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>True</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>True</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>True</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>True</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.notnull().sum()
```




    a    4
    b    4
    c    4
    dtype: int64




```python
df.a.notnull()
```




    n  v
    d  1     True
       2     True
    e  2     True
       3     True
       4    False
    Name: a, dtype: bool



# Logic in Python (and pandas)

- &,|,~,^,df.any(),df.all()
- and, or, not, xor, any, all


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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[(df.b == 8) & (df.a == 5)]
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>d</th>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.sample(n=1) #특정 개수만큼 랜덤하게 샘플링 해옴
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>e</th>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">d</th>
      <th>1</th>
      <td>4.0</td>
      <td>7.0</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>8.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">e</th>
      <th>2</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[-2:]

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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
    <tr>
      <th>n</th>
      <th>v</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">e</th>
      <th>3</th>
      <td>6.0</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>9.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.DataFrame({'a':[1,10,8,11,-1],
                  'b': list('abdce'),
                  'c' : [1.0,2.0,np.nan,3.0,4.0]})
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>b</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>d</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>c</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1</td>
      <td>e</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.nlargest(1,'a')
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>c</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.nsmallest(3,'a')
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>-1</td>
      <td>e</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>a</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>d</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>


