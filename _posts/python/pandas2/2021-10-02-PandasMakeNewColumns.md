---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 파이썬 판다스 assign 으로 새로운 컬럼 만들기,qcut으로 binning, bucketing 하기
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

# 1.Pandas 기초 - 파이썬 판다스 assign 으로 새로운 컬럼 만들기, qcut으로 binning, bucketing 하기


```python
import pandas as pd
import numpy as np
```


```python
df = pd.DataFrame({'A': range(1, 11), 'B': np.random.randn(10)})
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>-2.173554</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.373508</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.305635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>-0.250364</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>-1.567004</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>-0.937419</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>-0.107459</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>-1.705911</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>-0.957453</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>0.289418</td>
    </tr>
  </tbody>
</table>
</div>



# assign()함수에 대한 키워드 인수. DataFrame에 할당 할 열 이름은 키워드 인수로 전달됩니다.


```python
df.assign?
```

# 예제 코드: numpy.log()
import numpy as np

arr = [1, np.e, np.e**2, np.e**3]

print(np.log(arr))

출력:

[0. 1. 2. 3.]


```python
df.assign(ln_A = lambda x: np.log(x.A)).head()
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
      <th>ln_A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>-2.173554</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.373508</td>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.305635</td>
      <td>1.098612</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>-0.250364</td>
      <td>1.386294</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>-1.567004</td>
      <td>1.609438</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.ln_A = np.log(df.A)
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>-2.173554</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.373508</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.305635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>-0.250364</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>-1.567004</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>-0.937419</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>-0.107459</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>-1.705911</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>-0.957453</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>0.289418</td>
    </tr>
  </tbody>
</table>
</div>



# cut 함수 / qcut 함수 비교 참고
# https://kimdingko-world.tistory.com/209


```python
pd.qcut(df.B, 2, labels=["good", "bad"])
```




    0    good
    1     bad
    2     bad
    3     bad
    4    good
    5    good
    6     bad
    7    good
    8    good
    9     bad
    Name: B, dtype: category
    Categories (2, object): ['good' < 'bad']



cut 함수와 다르게 qcut 함수는 동일한 갯수로 구간을 나누는 것을 확인할 수 있다.


```python
df.max(axis=0)
```




    A    10.000000
    B     2.305635
    dtype: float64




```python
df.min(axis=0) # axis=0은 열기준/ axis=1은 행기준 
```




    A    1.000000
    B   -2.173554
    dtype: float64



# numpy.clip() 사용법

numpy.clip(array, min, max)

    array 내의 element들에 대해서

    min 값 보다 작은 값들을 min값으로 바꿔주고

    max 값 보다 큰 값들을 max값으로 바꿔주는 함수.

예시

    import numpy as np

    arr1 = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

    arr2 = np.clip(arr1, 3, 7)

    print(arr2)

 

    # 결과

        [ 3 3 3 3 4 5 6 7 7 7]


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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>-2.173554</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.373508</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.305635</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>-0.250364</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>-1.567004</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>-0.937419</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>-0.107459</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>-1.705911</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>-0.957453</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>0.289418</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["B"].clip(lower=1,upper=3)
```




    0    1.000000
    1    1.000000
    2    2.305635
    3    1.000000
    4    1.000000
    5    1.000000
    6    1.000000
    7    1.000000
    8    1.000000
    9    1.000000
    Name: B, dtype: float64




```python
df["B"].abs()
```




    0    2.173554
    1    0.373508
    2    2.305635
    3    0.250364
    4    1.567004
    5    0.937419
    6    0.107459
    7    1.705911
    8    0.957453
    9    0.289418
    Name: B, dtype: float64



# >>> abs(number)



전달한 숫자의 절대값을 돌려준다.


예제)



정수

>>> abs(-1)

1




실수

>>> abs(-1.75)

1.75



