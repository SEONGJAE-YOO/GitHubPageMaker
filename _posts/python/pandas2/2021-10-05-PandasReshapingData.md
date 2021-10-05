---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Pandas_Reshaping_Data
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
import seaborn as sns
```


```python
df = sns.load_dataset("mpg")
df.shape
```




    (398, 9)




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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>amc rebel sst</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>usa</td>
      <td>ford torino</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.sort_values('mpg',ascending=False)
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>322</th>
      <td>46.6</td>
      <td>4</td>
      <td>86.0</td>
      <td>65.0</td>
      <td>2110</td>
      <td>17.9</td>
      <td>80</td>
      <td>japan</td>
      <td>mazda glc</td>
    </tr>
    <tr>
      <th>329</th>
      <td>44.6</td>
      <td>4</td>
      <td>91.0</td>
      <td>67.0</td>
      <td>1850</td>
      <td>13.8</td>
      <td>80</td>
      <td>japan</td>
      <td>honda civic 1500 gl</td>
    </tr>
    <tr>
      <th>325</th>
      <td>44.3</td>
      <td>4</td>
      <td>90.0</td>
      <td>48.0</td>
      <td>2085</td>
      <td>21.7</td>
      <td>80</td>
      <td>europe</td>
      <td>vw rabbit c (diesel)</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
    </tr>
    <tr>
      <th>326</th>
      <td>43.4</td>
      <td>4</td>
      <td>90.0</td>
      <td>48.0</td>
      <td>2335</td>
      <td>23.7</td>
      <td>80</td>
      <td>europe</td>
      <td>vw dasher (diesel)</td>
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
      <th>103</th>
      <td>11.0</td>
      <td>8</td>
      <td>400.0</td>
      <td>150.0</td>
      <td>4997</td>
      <td>14.0</td>
      <td>73</td>
      <td>usa</td>
      <td>chevrolet impala</td>
    </tr>
    <tr>
      <th>67</th>
      <td>11.0</td>
      <td>8</td>
      <td>429.0</td>
      <td>208.0</td>
      <td>4633</td>
      <td>11.0</td>
      <td>72</td>
      <td>usa</td>
      <td>mercury marquis</td>
    </tr>
    <tr>
      <th>25</th>
      <td>10.0</td>
      <td>8</td>
      <td>360.0</td>
      <td>215.0</td>
      <td>4615</td>
      <td>14.0</td>
      <td>70</td>
      <td>usa</td>
      <td>ford f250</td>
    </tr>
    <tr>
      <th>26</th>
      <td>10.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>200.0</td>
      <td>4376</td>
      <td>15.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevy c20</td>
    </tr>
    <tr>
      <th>28</th>
      <td>9.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>193.0</td>
      <td>4732</td>
      <td>18.5</td>
      <td>70</td>
      <td>usa</td>
      <td>hi 1200d</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 9 columns</p>
</div>




```python
df.sort_values?
```


```python
df = df.rename(columns = {'model_year':'year'}) #칼럼 이름 변경하기( model_year 에서 year으로 이름 변경)
```


```python
df.sort_index()
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>amc rebel sst</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>usa</td>
      <td>ford torino</td>
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
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford mustang gl</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>usa</td>
      <td>dodge rampage</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford ranger</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>usa</td>
      <td>chevy s-10</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 9 columns</p>
</div>




```python
df.reset_index?
```


```python
df = df.drop(columns=['mpg','year', 'name'])
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
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>usa</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.melt?
```

    Object `pd.melt` not found.



```python
df = pd.DataFrame({'A': {0: 'a', 1: 'b', 2: 'c'},
                   'B': {0: 1, 1: 3, 2: 5},
                   'C': {0: 2, 1: 4, 2: 6}})
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>c</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



# melt 함수 참고 사이트
https://blog.naver.com/PostView.nhn?blogId=youji4ever&logNo=222048943354


```python
pd.melt(df, id_vars=['A'], value_vars=['B'])
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
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
      <td>B</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b</td>
      <td>B</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>c</td>
      <td>B</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.melt(df, id_vars=['A'], value_vars=['B', 'C'])
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
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
      <td>B</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b</td>
      <td>B</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>c</td>
      <td>B</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a</td>
      <td>C</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
      <td>C</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>c</td>
      <td>C</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby("variable").sum()
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
      <th>value</th>
    </tr>
    <tr>
      <th>variable</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>B</th>
      <td>9</td>
    </tr>
    <tr>
      <th>C</th>
      <td>12</td>
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
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>a</td>
      <td>B</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b</td>
      <td>B</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>c</td>
      <td>B</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a</td>
      <td>C</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b</td>
      <td>C</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>c</td>
      <td>C</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.pivot(columns='variable', values='value')
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
      <th>variable</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = pd.DataFrame({'foo': ['one','one','one','two','two','two'],
                       'bar': ['A', 'B', 'C', 'A', 'B', 'C'],
                       'baz': [1, 2, 3, 4, 5, 6]})
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
      <th>foo</th>
      <th>bar</th>
      <th>baz</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>A</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>one</td>
      <td>B</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>one</td>
      <td>C</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>A</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>two</td>
      <td>B</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>two</td>
      <td>C</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df2 = df.pivot(index='foo', columns='bar', values='baz')
```


```python
df.pivot(columns='bar')['baz']
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
      <th>bar</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>5.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3 = df2.reset_index()
df3
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
      <th>bar</th>
      <th>foo</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>two</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
df3.melt(id_vars=['foo'], value_vars=['A', 'B', 'C']).sort_values(by=['foo', 'bar'])
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
      <th>foo</th>
      <th>bar</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>one</td>
      <td>A</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>one</td>
      <td>B</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>one</td>
      <td>C</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>two</td>
      <td>A</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>two</td>
      <td>B</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>two</td>
      <td>C</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>


