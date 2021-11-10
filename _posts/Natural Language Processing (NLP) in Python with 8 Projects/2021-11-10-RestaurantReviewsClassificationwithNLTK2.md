---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Natural Language Processing (NLP) in Python with 8 Projects - Restaurant Reviews Classification with NLTK 응용해보기
tags: [udemy]  
class: post-template
subclass: 'post tag-udemy'  
author: SeongJae Yu  
sitemap:
lastmod: 2021-11-10
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include udemy.html %}

# Restaurant_Reviews_Classification_with_NLTK 응용해보기


```python
import numpy as np
import pandas as pd

import matplotlib.pyplot as plt
import seaborn as sns
```


```python
dataset = pd.read_csv("data/Restaurant_Reviews.tsv", delimiter='\t')
```


```python
dataset
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
      <th>Review</th>
      <th>Liked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wow... Loved this place.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Crust is not good.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Not tasty and the texture was just nasty.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Stopped by during the late May bank holiday of...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The selection on the menu was great and so wer...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>I think food should have flavor and texture an...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>996</th>
      <td>Appetite instantly gone.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>997</th>
      <td>Overall I was not impressed and would not go b...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>998</th>
      <td>The whole experience was underwhelming, and I ...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>999</th>
      <td>Then, as if I hadn't wasted enough of my life ...</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 2 columns</p>
</div>




```python
dataset.head()
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
      <th>Review</th>
      <th>Liked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wow... Loved this place.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Crust is not good.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Not tasty and the texture was just nasty.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Stopped by during the late May bank holiday of...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The selection on the menu was great and so wer...</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
dataset.describe()
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
      <th>Liked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1000.00000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.50000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.50025</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.50000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.00000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.00000</td>
    </tr>
  </tbody>
</table>
</div>




```python
dataset.info() # dataframe 확인 하기 
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000 entries, 0 to 999
    Data columns (total 2 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   Review  1000 non-null   object
     1   Liked   1000 non-null   int64 
    dtypes: int64(1), object(1)
    memory usage: 15.8+ KB


# Checking for null values


```python
dataset.isnull().sum()
```




    Review    0
    Liked     0
    dtype: int64




```python
sns.countplot(x = dataset['Liked'],data= dataset)
```




    <AxesSubplot:xlabel='Liked', ylabel='count'>





![output_9_1](./img/udemy/output_9_1.png)




```python
dataset[dataset['Liked']==1]['Liked'].count()
```




    500




```python
dataset[dataset['Liked']==0]['Liked'].count()
```




    500




```python
from nltk.corpus import stopwords
from nltk.stem.snowball import SnowballStemmer
```


```python
import re
```

# Data Preprocessing


```python
stemmer = SnowballStemmer('english')
```


```python
corpus = []
```


```python
for i in range(0,1000):
    review = re.sub('[^a-zA-Z]',' ',dataset['Review'][i])
    review = review.lower()
    review = review.split()
    review = [stemmer.stem(word) for word in review if word not in set(stopwords.words('english'))]
    review = ' '.join(review)
    corpus.append(review)
    
```


```python
corpus[1] # is , not 불용어 제거됨
```




    'crust good'




```python
len(corpus)
```




    1000




```python
corpus[999]
```




    'wast enough life pour salt wound draw time took bring check'



# Creating Bag of Words Model


```python
from sklearn.feature_extraction.text import CountVectorizer
```


```python
cv = CountVectorizer(max_features=1500)
```


```python
x = cv.fit_transform(corpus).toarray()
```


```python
x.shape
```




    (1000, 1500)




```python
y =dataset['Liked'].values
```


```python
from sklearn.model_selection import train_test_split
```


```python
x_train,x_test,y_train,y_test = train_test_split(x,y,test_size=0.2,random_state=17)
```

# Naive Baye's Classifier(MultinomialNB)



```python
from sklearn.naive_bayes import MultinomialNB
```


```python
classifier = MultinomialNB()
```

# Training the classifier


```python
classifier.fit(x_train,y_train)
```




    MultinomialNB()



# making Predictions


```python
y_pred = classifier.predict(x_test)
```


```python
y_train_pred = classifier.predict(x_train)
```

# Evaluating the classifier


```python
from sklearn.metrics import classification_report,confusion_matrix,accuracy_score
```


```python
print(classification_report(y_test,y_pred))
```

                  precision    recall  f1-score   support
    
               0       0.74      0.77      0.75        95
               1       0.78      0.75      0.77       105
    
        accuracy                           0.76       200
       macro avg       0.76      0.76      0.76       200
    weighted avg       0.76      0.76      0.76       200




```python
#https://www.kaggle.com/satheeshrsm/restaurant-review-classification/notebook
```
