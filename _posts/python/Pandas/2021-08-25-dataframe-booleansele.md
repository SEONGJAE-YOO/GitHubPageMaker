---
date: 2021-08-25 19:23:00
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: DataFrame Boolean Selection으로 데이터 선택하기
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu  
sitemap:
lastmod: 2021-08-23
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---    
{% include python-table-of-contents.html %}

## 학습목표
1. Dataframe boolean selection 이해하기


```python
import pandas as pd
```


```python
# data 출처: https://www.kaggle.com/hesh97/titanicdataset-traincsv/data
train_data = pd.read_csv('./train.csv')
```


```python
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



#### **boolean selection으로 row 선택하기**
- numpy에서와 동일한 방식으로 해당 조건에 맞는 row만 선택

#### 30대이면서 1등석에 탄 사람 선택하기


```python
class_ = train_data['Pclass'] == 1
age_ = (train_data['Age'] >= 30) & (train_data['Age'] < 40)

train_data[class_ & age_]
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
      <th>61</th>
      <td>62</td>
      <td>1</td>
      <td>1</td>
      <td>Icard, Miss. Amelie</td>
      <td>female</td>
      <td>38.0</td>
      <td>0</td>
      <td>0</td>
      <td>113572</td>
      <td>80.0000</td>
      <td>B28</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>137</th>
      <td>138</td>
      <td>0</td>
      <td>1</td>
      <td>Futrelle, Mr. Jacques Heath</td>
      <td>male</td>
      <td>37.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>215</th>
      <td>216</td>
      <td>1</td>
      <td>1</td>
      <td>Newell, Miss. Madeleine</td>
      <td>female</td>
      <td>31.0</td>
      <td>1</td>
      <td>0</td>
      <td>35273</td>
      <td>113.2750</td>
      <td>D36</td>
      <td>C</td>
    </tr>
    <tr>
      <th>218</th>
      <td>219</td>
      <td>1</td>
      <td>1</td>
      <td>Bazzani, Miss. Albina</td>
      <td>female</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>11813</td>
      <td>76.2917</td>
      <td>D15</td>
      <td>C</td>
    </tr>
    <tr>
      <th>224</th>
      <td>225</td>
      <td>1</td>
      <td>1</td>
      <td>Hoyt, Mr. Frederick Maxfield</td>
      <td>male</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>19943</td>
      <td>90.0000</td>
      <td>C93</td>
      <td>S</td>
    </tr>
    <tr>
      <th>230</th>
      <td>231</td>
      <td>1</td>
      <td>1</td>
      <td>Harris, Mrs. Henry Birkhardt (Irene Wallach)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>36973</td>
      <td>83.4750</td>
      <td>C83</td>
      <td>S</td>
    </tr>
    <tr>
      <th>248</th>
      <td>249</td>
      <td>1</td>
      <td>1</td>
      <td>Beckwith, Mr. Richard Leonard</td>
      <td>male</td>
      <td>37.0</td>
      <td>1</td>
      <td>1</td>
      <td>11751</td>
      <td>52.5542</td>
      <td>D35</td>
      <td>S</td>
    </tr>
    <tr>
      <th>257</th>
      <td>258</td>
      <td>1</td>
      <td>1</td>
      <td>Cherry, Miss. Gladys</td>
      <td>female</td>
      <td>30.0</td>
      <td>0</td>
      <td>0</td>
      <td>110152</td>
      <td>86.5000</td>
      <td>B77</td>
      <td>S</td>
    </tr>
    <tr>
      <th>258</th>
      <td>259</td>
      <td>1</td>
      <td>1</td>
      <td>Ward, Miss. Anna</td>
      <td>female</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17755</td>
      <td>512.3292</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>269</th>
      <td>270</td>
      <td>1</td>
      <td>1</td>
      <td>Bissette, Miss. Amelia</td>
      <td>female</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17760</td>
      <td>135.6333</td>
      <td>C99</td>
      <td>S</td>
    </tr>
    <tr>
      <th>273</th>
      <td>274</td>
      <td>0</td>
      <td>1</td>
      <td>Natsch, Mr. Charles H</td>
      <td>male</td>
      <td>37.0</td>
      <td>0</td>
      <td>1</td>
      <td>PC 17596</td>
      <td>29.7000</td>
      <td>C118</td>
      <td>C</td>
    </tr>
    <tr>
      <th>309</th>
      <td>310</td>
      <td>1</td>
      <td>1</td>
      <td>Francatelli, Miss. Laura Mabel</td>
      <td>female</td>
      <td>30.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17485</td>
      <td>56.9292</td>
      <td>E36</td>
      <td>C</td>
    </tr>
    <tr>
      <th>318</th>
      <td>319</td>
      <td>1</td>
      <td>1</td>
      <td>Wick, Miss. Mary Natalie</td>
      <td>female</td>
      <td>31.0</td>
      <td>0</td>
      <td>2</td>
      <td>36928</td>
      <td>164.8667</td>
      <td>C7</td>
      <td>S</td>
    </tr>
    <tr>
      <th>325</th>
      <td>326</td>
      <td>1</td>
      <td>1</td>
      <td>Young, Miss. Marie Grice</td>
      <td>female</td>
      <td>36.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17760</td>
      <td>135.6333</td>
      <td>C32</td>
      <td>C</td>
    </tr>
    <tr>
      <th>332</th>
      <td>333</td>
      <td>0</td>
      <td>1</td>
      <td>Graham, Mr. George Edward</td>
      <td>male</td>
      <td>38.0</td>
      <td>0</td>
      <td>1</td>
      <td>PC 17582</td>
      <td>153.4625</td>
      <td>C91</td>
      <td>S</td>
    </tr>
    <tr>
      <th>383</th>
      <td>384</td>
      <td>1</td>
      <td>1</td>
      <td>Holverson, Mrs. Alexander Oskar (Mary Aline To...</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113789</td>
      <td>52.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>390</th>
      <td>391</td>
      <td>1</td>
      <td>1</td>
      <td>Carter, Mr. William Ernest</td>
      <td>male</td>
      <td>36.0</td>
      <td>1</td>
      <td>2</td>
      <td>113760</td>
      <td>120.0000</td>
      <td>B96 B98</td>
      <td>S</td>
    </tr>
    <tr>
      <th>412</th>
      <td>413</td>
      <td>1</td>
      <td>1</td>
      <td>Minahan, Miss. Daisy E</td>
      <td>female</td>
      <td>33.0</td>
      <td>1</td>
      <td>0</td>
      <td>19928</td>
      <td>90.0000</td>
      <td>C78</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>447</th>
      <td>448</td>
      <td>1</td>
      <td>1</td>
      <td>Seward, Mr. Frederic Kimber</td>
      <td>male</td>
      <td>34.0</td>
      <td>0</td>
      <td>0</td>
      <td>113794</td>
      <td>26.5500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>452</th>
      <td>453</td>
      <td>0</td>
      <td>1</td>
      <td>Foreman, Mr. Benjamin Laventall</td>
      <td>male</td>
      <td>30.0</td>
      <td>0</td>
      <td>0</td>
      <td>113051</td>
      <td>27.7500</td>
      <td>C111</td>
      <td>C</td>
    </tr>
    <tr>
      <th>486</th>
      <td>487</td>
      <td>1</td>
      <td>1</td>
      <td>Hoyt, Mrs. Frederick Maxfield (Jane Anne Forby)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>19943</td>
      <td>90.0000</td>
      <td>C93</td>
      <td>S</td>
    </tr>
    <tr>
      <th>512</th>
      <td>513</td>
      <td>1</td>
      <td>1</td>
      <td>McGough, Mr. James Robert</td>
      <td>male</td>
      <td>36.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17473</td>
      <td>26.2875</td>
      <td>E25</td>
      <td>S</td>
    </tr>
    <tr>
      <th>520</th>
      <td>521</td>
      <td>1</td>
      <td>1</td>
      <td>Perreault, Miss. Anne</td>
      <td>female</td>
      <td>30.0</td>
      <td>0</td>
      <td>0</td>
      <td>12749</td>
      <td>93.5000</td>
      <td>B73</td>
      <td>S</td>
    </tr>
    <tr>
      <th>537</th>
      <td>538</td>
      <td>1</td>
      <td>1</td>
      <td>LeRoy, Miss. Bertha</td>
      <td>female</td>
      <td>30.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17761</td>
      <td>106.4250</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>540</th>
      <td>541</td>
      <td>1</td>
      <td>1</td>
      <td>Crosby, Miss. Harriet R</td>
      <td>female</td>
      <td>36.0</td>
      <td>0</td>
      <td>2</td>
      <td>WE/P 5735</td>
      <td>71.0000</td>
      <td>B22</td>
      <td>S</td>
    </tr>
    <tr>
      <th>558</th>
      <td>559</td>
      <td>1</td>
      <td>1</td>
      <td>Taussig, Mrs. Emil (Tillie Mandelbaum)</td>
      <td>female</td>
      <td>39.0</td>
      <td>1</td>
      <td>1</td>
      <td>110413</td>
      <td>79.6500</td>
      <td>E67</td>
      <td>S</td>
    </tr>
    <tr>
      <th>572</th>
      <td>573</td>
      <td>1</td>
      <td>1</td>
      <td>Flynn, Mr. John Irwin ("Irving")</td>
      <td>male</td>
      <td>36.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17474</td>
      <td>26.3875</td>
      <td>E25</td>
      <td>S</td>
    </tr>
    <tr>
      <th>577</th>
      <td>578</td>
      <td>1</td>
      <td>1</td>
      <td>Silvey, Mrs. William Baird (Alice Munger)</td>
      <td>female</td>
      <td>39.0</td>
      <td>1</td>
      <td>0</td>
      <td>13507</td>
      <td>55.9000</td>
      <td>E44</td>
      <td>S</td>
    </tr>
    <tr>
      <th>581</th>
      <td>582</td>
      <td>1</td>
      <td>1</td>
      <td>Thayer, Mrs. John Borland (Marian Longstreth M...</td>
      <td>female</td>
      <td>39.0</td>
      <td>1</td>
      <td>1</td>
      <td>17421</td>
      <td>110.8833</td>
      <td>C68</td>
      <td>C</td>
    </tr>
    <tr>
      <th>583</th>
      <td>584</td>
      <td>0</td>
      <td>1</td>
      <td>Ross, Mr. John Hugo</td>
      <td>male</td>
      <td>36.0</td>
      <td>0</td>
      <td>0</td>
      <td>13049</td>
      <td>40.1250</td>
      <td>A10</td>
      <td>C</td>
    </tr>
    <tr>
      <th>604</th>
      <td>605</td>
      <td>1</td>
      <td>1</td>
      <td>Homer, Mr. Harry ("Mr E Haven")</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>111426</td>
      <td>26.5500</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>632</th>
      <td>633</td>
      <td>1</td>
      <td>1</td>
      <td>Stahelin-Maeglin, Dr. Max</td>
      <td>male</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>13214</td>
      <td>30.5000</td>
      <td>B50</td>
      <td>C</td>
    </tr>
    <tr>
      <th>671</th>
      <td>672</td>
      <td>0</td>
      <td>1</td>
      <td>Davidson, Mr. Thornton</td>
      <td>male</td>
      <td>31.0</td>
      <td>1</td>
      <td>0</td>
      <td>F.C. 12750</td>
      <td>52.0000</td>
      <td>B71</td>
      <td>S</td>
    </tr>
    <tr>
      <th>679</th>
      <td>680</td>
      <td>1</td>
      <td>1</td>
      <td>Cardeza, Mr. Thomas Drake Martinez</td>
      <td>male</td>
      <td>36.0</td>
      <td>0</td>
      <td>1</td>
      <td>PC 17755</td>
      <td>512.3292</td>
      <td>B51 B53 B55</td>
      <td>C</td>
    </tr>
    <tr>
      <th>690</th>
      <td>691</td>
      <td>1</td>
      <td>1</td>
      <td>Dick, Mr. Albert Adrian</td>
      <td>male</td>
      <td>31.0</td>
      <td>1</td>
      <td>0</td>
      <td>17474</td>
      <td>57.0000</td>
      <td>B20</td>
      <td>S</td>
    </tr>
    <tr>
      <th>701</th>
      <td>702</td>
      <td>1</td>
      <td>1</td>
      <td>Silverthorne, Mr. Spencer Victor</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17475</td>
      <td>26.2875</td>
      <td>E24</td>
      <td>S</td>
    </tr>
    <tr>
      <th>716</th>
      <td>717</td>
      <td>1</td>
      <td>1</td>
      <td>Endres, Miss. Caroline Louise</td>
      <td>female</td>
      <td>38.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17757</td>
      <td>227.5250</td>
      <td>C45</td>
      <td>C</td>
    </tr>
    <tr>
      <th>737</th>
      <td>738</td>
      <td>1</td>
      <td>1</td>
      <td>Lesurer, Mr. Gustave J</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17755</td>
      <td>512.3292</td>
      <td>B101</td>
      <td>C</td>
    </tr>
    <tr>
      <th>741</th>
      <td>742</td>
      <td>0</td>
      <td>1</td>
      <td>Cavendish, Mr. Tyrell William</td>
      <td>male</td>
      <td>36.0</td>
      <td>1</td>
      <td>0</td>
      <td>19877</td>
      <td>78.8500</td>
      <td>C46</td>
      <td>S</td>
    </tr>
    <tr>
      <th>759</th>
      <td>760</td>
      <td>1</td>
      <td>1</td>
      <td>Rothes, the Countess. of (Lucy Noel Martha Dye...</td>
      <td>female</td>
      <td>33.0</td>
      <td>0</td>
      <td>0</td>
      <td>110152</td>
      <td>86.5000</td>
      <td>B77</td>
      <td>S</td>
    </tr>
    <tr>
      <th>763</th>
      <td>764</td>
      <td>1</td>
      <td>1</td>
      <td>Carter, Mrs. William Ernest (Lucile Polk)</td>
      <td>female</td>
      <td>36.0</td>
      <td>1</td>
      <td>2</td>
      <td>113760</td>
      <td>120.0000</td>
      <td>B96 B98</td>
      <td>S</td>
    </tr>
    <tr>
      <th>806</th>
      <td>807</td>
      <td>0</td>
      <td>1</td>
      <td>Andrews, Mr. Thomas Jr</td>
      <td>male</td>
      <td>39.0</td>
      <td>0</td>
      <td>0</td>
      <td>112050</td>
      <td>0.0000</td>
      <td>A36</td>
      <td>S</td>
    </tr>
    <tr>
      <th>809</th>
      <td>810</td>
      <td>1</td>
      <td>1</td>
      <td>Chambers, Mrs. Norman Campbell (Bertha Griggs)</td>
      <td>female</td>
      <td>33.0</td>
      <td>1</td>
      <td>0</td>
      <td>113806</td>
      <td>53.1000</td>
      <td>E8</td>
      <td>S</td>
    </tr>
    <tr>
      <th>822</th>
      <td>823</td>
      <td>0</td>
      <td>1</td>
      <td>Reuchlin, Jonkheer. John George</td>
      <td>male</td>
      <td>38.0</td>
      <td>0</td>
      <td>0</td>
      <td>19972</td>
      <td>0.0000</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>835</th>
      <td>836</td>
      <td>1</td>
      <td>1</td>
      <td>Compton, Miss. Sara Rebecca</td>
      <td>female</td>
      <td>39.0</td>
      <td>1</td>
      <td>1</td>
      <td>PC 17756</td>
      <td>83.1583</td>
      <td>E49</td>
      <td>C</td>
    </tr>
    <tr>
      <th>842</th>
      <td>843</td>
      <td>1</td>
      <td>1</td>
      <td>Serepeca, Miss. Augusta</td>
      <td>female</td>
      <td>30.0</td>
      <td>0</td>
      <td>0</td>
      <td>113798</td>
      <td>31.0000</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>867</th>
      <td>868</td>
      <td>0</td>
      <td>1</td>
      <td>Roebling, Mr. Washington Augustus II</td>
      <td>male</td>
      <td>31.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17590</td>
      <td>50.4958</td>
      <td>A24</td>
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
  </tbody>
</table>
</div>



#### 남자이면서 1등석에 탄 사람 선택하기


```python
class_ = train_data['Pclass'] == 1
age_ = train_data['Sex'] == 'male'

train_data[class_ & age_]
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
      <th>23</th>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>Sloper, Mr. William Thompson</td>
      <td>male</td>
      <td>28.0</td>
      <td>0</td>
      <td>0</td>
      <td>113788</td>
      <td>35.5000</td>
      <td>A6</td>
      <td>S</td>
    </tr>
    <tr>
      <th>27</th>
      <td>28</td>
      <td>0</td>
      <td>1</td>
      <td>Fortune, Mr. Charles Alexander</td>
      <td>male</td>
      <td>19.0</td>
      <td>3</td>
      <td>2</td>
      <td>19950</td>
      <td>263.0000</td>
      <td>C23 C25 C27</td>
      <td>S</td>
    </tr>
    <tr>
      <th>30</th>
      <td>31</td>
      <td>0</td>
      <td>1</td>
      <td>Uruchurtu, Don. Manuel E</td>
      <td>male</td>
      <td>40.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17601</td>
      <td>27.7208</td>
      <td>NaN</td>
      <td>C</td>
    </tr>
    <tr>
      <th>34</th>
      <td>35</td>
      <td>0</td>
      <td>1</td>
      <td>Meyer, Mr. Edgar Joseph</td>
      <td>male</td>
      <td>28.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17604</td>
      <td>82.1708</td>
      <td>NaN</td>
      <td>C</td>
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
      <th>839</th>
      <td>840</td>
      <td>1</td>
      <td>1</td>
      <td>Marechal, Mr. Pierre</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>11774</td>
      <td>29.7000</td>
      <td>C47</td>
      <td>C</td>
    </tr>
    <tr>
      <th>857</th>
      <td>858</td>
      <td>1</td>
      <td>1</td>
      <td>Daly, Mr. Peter Denis</td>
      <td>male</td>
      <td>51.0</td>
      <td>0</td>
      <td>0</td>
      <td>113055</td>
      <td>26.5500</td>
      <td>E17</td>
      <td>S</td>
    </tr>
    <tr>
      <th>867</th>
      <td>868</td>
      <td>0</td>
      <td>1</td>
      <td>Roebling, Mr. Washington Augustus II</td>
      <td>male</td>
      <td>31.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17590</td>
      <td>50.4958</td>
      <td>A24</td>
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
<p>122 rows × 12 columns</p>
</div>


