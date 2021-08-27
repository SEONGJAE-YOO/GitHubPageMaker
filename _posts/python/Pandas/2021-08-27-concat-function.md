---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Concat 함수로 데이터 프레임 병합하기 
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
1. concat 함수를 활용하여 dataframe 병합시키기


```python
import pandas as pd
import numpy as np
```

#### concat 함수 사용하여 dataframe 병합하기
- pandas.concat 함수
- 축을 따라 dataframe을 병합 가능
    - 기본 axis = 0 -> 행단위 병합

* column명이 같은 경우


```python
df1 = pd.DataFrame({'key1' : np.arange(10), 'value1' : np.random.randn(10)})
```


```python
df2 = pd.DataFrame({'key1' : np.arange(10), 'value1' : np.random.randn(10)})
```


```python
df2
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
      <th>key1</th>
      <th>value1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>-0.338849</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.688248</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.863890</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.431818</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>-0.345499</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>0.626425</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.639522</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>-0.677354</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>-0.778642</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>-0.600007</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([df1, df2], ignore_index=True)
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
      <th>key1</th>
      <th>value1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.157695</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.815835</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.512740</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>-0.575658</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>-0.713351</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>1.701762</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.296171</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>-0.018002</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>-1.302774</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>-2.626175</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0</td>
      <td>-0.338849</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1</td>
      <td>0.688248</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2</td>
      <td>0.863890</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3</td>
      <td>0.431818</td>
    </tr>
    <tr>
      <th>14</th>
      <td>4</td>
      <td>-0.345499</td>
    </tr>
    <tr>
      <th>15</th>
      <td>5</td>
      <td>0.626425</td>
    </tr>
    <tr>
      <th>16</th>
      <td>6</td>
      <td>0.639522</td>
    </tr>
    <tr>
      <th>17</th>
      <td>7</td>
      <td>-0.677354</td>
    </tr>
    <tr>
      <th>18</th>
      <td>8</td>
      <td>-0.778642</td>
    </tr>
    <tr>
      <th>19</th>
      <td>9</td>
      <td>-0.600007</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([df1, df2], axis=0) #기본값
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
      <th>key1</th>
      <th>value1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.157695</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.815835</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.512740</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>-0.575658</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>-0.713351</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>1.701762</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.296171</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>-0.018002</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>-1.302774</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>-2.626175</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>-0.338849</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.688248</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.863890</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.431818</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>-0.345499</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>0.626425</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.639522</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>-0.677354</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>-0.778642</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>-0.600007</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat([df1, df2], axis=1) 
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
      <th>key1</th>
      <th>value1</th>
      <th>key1</th>
      <th>value1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.157695</td>
      <td>0</td>
      <td>-0.338849</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.815835</td>
      <td>1</td>
      <td>0.688248</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.512740</td>
      <td>2</td>
      <td>0.863890</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>-0.575658</td>
      <td>3</td>
      <td>0.431818</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>-0.713351</td>
      <td>4</td>
      <td>-0.345499</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>1.701762</td>
      <td>5</td>
      <td>0.626425</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.296171</td>
      <td>6</td>
      <td>0.639522</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>-0.018002</td>
      <td>7</td>
      <td>-0.677354</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>-1.302774</td>
      <td>8</td>
      <td>-0.778642</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>-2.626175</td>
      <td>9</td>
      <td>-0.600007</td>
    </tr>
  </tbody>
</table>
</div>



* column 명이 다른 경우


```python
df3 = pd.DataFrame({'key2' : np.arange(10), 'value2' : np.random.randn(10)})
```


```python
pd.concat([df1, df3], axis=1)
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
      <th>key1</th>
      <th>value1</th>
      <th>key2</th>
      <th>value2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.157695</td>
      <td>0</td>
      <td>2.096654</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.815835</td>
      <td>1</td>
      <td>1.434691</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.512740</td>
      <td>2</td>
      <td>-0.211020</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>-0.575658</td>
      <td>3</td>
      <td>1.498715</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>-0.713351</td>
      <td>4</td>
      <td>-1.106296</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>1.701762</td>
      <td>5</td>
      <td>-0.678457</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.296171</td>
      <td>6</td>
      <td>-0.420552</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>-0.018002</td>
      <td>7</td>
      <td>-0.091809</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>-1.302774</td>
      <td>8</td>
      <td>0.603147</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>-2.626175</td>
      <td>9</td>
      <td>-0.918178</td>
    </tr>
  </tbody>
</table>
</div>


