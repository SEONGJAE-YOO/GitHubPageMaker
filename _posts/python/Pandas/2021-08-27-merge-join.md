---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Merge_join 함수로 데이터 프레임 병합하기
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
1. merge & join 함수 활용하기


```python
import numpy as np
import pandas as pd
```

#### dataframe merge
- SQL의 join처럼 특정한 column을 기준으로 병합
  - join 방식: how 파라미터를 통해 명시
    - inner: 기본값, 일치하는 값이 있는 경우
    - left: left outer join
    - right: right outer join
    - outer: full outer join

- pandas.merge 함수가 사용됨


```python
customer = pd.DataFrame({'customer_id' : np.arange(6), 
                    'name' : ['철수'"", '영희', '길동', '영수', '수민', '동건'], 
                    '나이' : [40, 20, 21, 30, 31, 18]})

customer
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
      <th>customer_id</th>
      <th>name</th>
      <th>나이</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>철수</td>
      <td>40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>영수</td>
      <td>30</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>수민</td>
      <td>31</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>동건</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
orders = pd.DataFrame({'customer_id' : [1, 1, 2, 2, 2, 3, 3, 1, 4, 9], 
                    'item' : ['치약', '칫솔', '이어폰', '헤드셋', '수건', '생수', '수건', '치약', '생수', '케이스'], 
                    'quantity' : [1, 2, 1, 1, 3, 2, 2, 3, 2, 1]})
orders.head()
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
      <th>customer_id</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>치약</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>칫솔</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>이어폰</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>헤드셋</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>수건</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



* on
- join 대상이 되는 column 명시


```python
pd.merge(customer, orders, on='customer_id', how='inner') #(테이블명1, 테이블명2)
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
      <th>customer_id</th>
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
      <td>칫솔</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
      <td>이어폰</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
      <td>헤드셋</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
      <td>수건</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>3</td>
      <td>영수</td>
      <td>30</td>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3</td>
      <td>영수</td>
      <td>30</td>
      <td>수건</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4</td>
      <td>수민</td>
      <td>31</td>
      <td>생수</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(customer, orders, on='customer_id', how='left')
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
      <th>customer_id</th>
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>철수</td>
      <td>40</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
      <td>칫솔</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
      <td>이어폰</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
      <td>헤드셋</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>길동</td>
      <td>21</td>
      <td>수건</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3</td>
      <td>영수</td>
      <td>30</td>
      <td>생수</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>영수</td>
      <td>30</td>
      <td>수건</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
      <td>수민</td>
      <td>31</td>
      <td>생수</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5</td>
      <td>동건</td>
      <td>18</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(customer, orders, on='customer_id', how='right')
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
      <th>customer_id</th>
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>영희</td>
      <td>20.0</td>
      <td>치약</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>영희</td>
      <td>20.0</td>
      <td>칫솔</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>길동</td>
      <td>21.0</td>
      <td>이어폰</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>길동</td>
      <td>21.0</td>
      <td>헤드셋</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>길동</td>
      <td>21.0</td>
      <td>수건</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>3</td>
      <td>영수</td>
      <td>30.0</td>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>3</td>
      <td>영수</td>
      <td>30.0</td>
      <td>수건</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>영희</td>
      <td>20.0</td>
      <td>치약</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4</td>
      <td>수민</td>
      <td>31.0</td>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>케이스</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(customer, orders, on='customer_id', how='outer') #left + right 값 합친것 
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
      <th>customer_id</th>
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>철수</td>
      <td>40.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>영희</td>
      <td>20.0</td>
      <td>치약</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>영희</td>
      <td>20.0</td>
      <td>칫솔</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>영희</td>
      <td>20.0</td>
      <td>치약</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>길동</td>
      <td>21.0</td>
      <td>이어폰</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>길동</td>
      <td>21.0</td>
      <td>헤드셋</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>길동</td>
      <td>21.0</td>
      <td>수건</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3</td>
      <td>영수</td>
      <td>30.0</td>
      <td>생수</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>영수</td>
      <td>30.0</td>
      <td>수건</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
      <td>수민</td>
      <td>31.0</td>
      <td>생수</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5</td>
      <td>동건</td>
      <td>18.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11</th>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>케이스</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



* index 기준으로 join하기


```python
cust1 = customer.set_index('customer_id')
order1 = orders.set_index('customer_id')
```


```python
cust1
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
      <th>name</th>
      <th>나이</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>철수</td>
      <td>40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수민</td>
      <td>31</td>
    </tr>
    <tr>
      <th>5</th>
      <td>동건</td>
      <td>18</td>
    </tr>
  </tbody>
</table>
</div>




```python
order1
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
      <th>item</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>치약</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>칫솔</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>이어폰</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>헤드셋</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>수건</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>수건</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>치약</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>케이스</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(cust1, order1, left_index=True, right_index=True) #on='customer_id' 대신 사용가능
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
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>칫솔</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>이어폰</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>헤드셋</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>수건</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
      <td>수건</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수민</td>
      <td>31</td>
      <td>생수</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



#### 연습문제
1. 가장 많이 팔린 아이템은?
2. 영희가 가장 많이 구매한 아이템은?


```python
pd.merge(customer, orders, on='customer_id').groupby('item').sum().sort_values(by='quantity', ascending=False)
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
      <th>customer_id</th>
      <th>나이</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>수건</th>
      <td>5</td>
      <td>51</td>
      <td>5</td>
    </tr>
    <tr>
      <th>생수</th>
      <td>7</td>
      <td>61</td>
      <td>4</td>
    </tr>
    <tr>
      <th>치약</th>
      <td>2</td>
      <td>40</td>
      <td>4</td>
    </tr>
    <tr>
      <th>칫솔</th>
      <td>1</td>
      <td>20</td>
      <td>2</td>
    </tr>
    <tr>
      <th>이어폰</th>
      <td>2</td>
      <td>21</td>
      <td>1</td>
    </tr>
    <tr>
      <th>헤드셋</th>
      <td>2</td>
      <td>21</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(customer, orders, on='customer_id').groupby(['name', 'item']).sum()
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
      <th>customer_id</th>
      <th>나이</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>name</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">길동</th>
      <th>수건</th>
      <td>2</td>
      <td>21</td>
      <td>3</td>
    </tr>
    <tr>
      <th>이어폰</th>
      <td>2</td>
      <td>21</td>
      <td>1</td>
    </tr>
    <tr>
      <th>헤드셋</th>
      <td>2</td>
      <td>21</td>
      <td>1</td>
    </tr>
    <tr>
      <th>수민</th>
      <th>생수</th>
      <td>4</td>
      <td>31</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">영수</th>
      <th>생수</th>
      <td>3</td>
      <td>30</td>
      <td>2</td>
    </tr>
    <tr>
      <th>수건</th>
      <td>3</td>
      <td>30</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">영희</th>
      <th>치약</th>
      <td>2</td>
      <td>40</td>
      <td>4</td>
    </tr>
    <tr>
      <th>칫솔</th>
      <td>1</td>
      <td>20</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(customer, orders, on='customer_id').groupby(['name', 'item']).sum().loc['영희']
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
      <th>customer_id</th>
      <th>나이</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>치약</th>
      <td>2</td>
      <td>40</td>
      <td>4</td>
    </tr>
    <tr>
      <th>칫솔</th>
      <td>1</td>
      <td>20</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.merge(customer, orders, on='customer_id').groupby(['name', 'item']).sum().loc['영희', 'quantity']
```




    item
    치약    4
    칫솔    2
    Name: quantity, dtype: int64



#### join 함수
- 내부적으로 pandas.merge 함수 사용
- 기본적으로 index를 사용하여 left join


```python
cust1.join(order1)
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
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>철수</td>
      <td>40</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>칫솔</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>이어폰</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>헤드셋</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>수건</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
      <td>생수</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
      <td>수건</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수민</td>
      <td>31</td>
      <td>생수</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>동건</td>
      <td>18</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
cust1.join(order1, how='inner')
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
      <th>name</th>
      <th>나이</th>
      <th>item</th>
      <th>quantity</th>
    </tr>
    <tr>
      <th>customer_id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>칫솔</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>영희</td>
      <td>20</td>
      <td>치약</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>이어폰</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>헤드셋</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>길동</td>
      <td>21</td>
      <td>수건</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
      <td>생수</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>영수</td>
      <td>30</td>
      <td>수건</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수민</td>
      <td>31</td>
      <td>생수</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>


