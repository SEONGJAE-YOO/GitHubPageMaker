---
layout: page
current: page
cover:  assets/built/images/udemy.png
navigation: True
title: Natural Language Processing (NLP) in Python with 8 Projects - IMDB and Amazon Review Classification with SpaCy
tags: [udemy]  
class: post-template
subclass: 'post tag-udemy'  
author: SeongJae Yu  
sitemap:
lastmod: 2021-11-08
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include udemy.html %}

# 1) Importing the datasets


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```


```python
data_yelp = pd.read_csv('data/yelp_labelled.txt', sep='\t',header=None)
```

# yelp_labelled.txt 데이터는 아래 링크를 클릭하면 보실 수 있습니다.
# 링크 -[https://github.com/SEONGJAE-YOO/Natural-Language-Processing-NLP-in-Python-with-8-Projects/blob/main/data/yelp_labelled.txt](https://github.com/SEONGJAE-YOO/Natural-Language-Processing-NLP-in-Python-with-8-Projects/blob/main/data/yelp_labelled.txt)


```python
data_yelp.head()
# review and sentiment
# 0-Negative, 1-Positive for positive review
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
      <th>0</th>
      <th>1</th>
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
# Assign column names
columan_name = ['Review', 'Sentiment']
data_yelp.columns = columan_name
```


```python
data_yelp.head()
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
      <th>Sentiment</th>
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
data_yelp.shape
# 1000 rows (reviews), 2 columns (Sentiments)
```




    (1000, 2)




```python
data_amazon = pd.read_csv('data/amazon_cells_labelled.txt',sep='\t',header=None)
data_amazon.head()
# review and sentiment
# 0-Negative, 1-Positive for positive review
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
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>So there is no way for me to plug it in here i...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Good case, Excellent value.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Great for the jawbone.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tied to charger for conversations lasting more...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The mic is great.</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



# amazon_cells_labelled.txt 데이터는 아래 링크를 클릭하면 보실 수 있습니다.
# 링크-[https://github.com/SEONGJAE-YOO/Natural-Language-Processing-NLP-in-Python-with-8-Projects/blob/main/data/amazon_cells_labelled.txt](https://github.com/SEONGJAE-YOO/Natural-Language-Processing-NLP-in-Python-with-8-Projects/blob/main/data/amazon_cells_labelled.txt)


```python
data_amazon.columns = columan_name
```


```python
data_amazon.head()
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
      <th>Sentiment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>So there is no way for me to plug it in here i...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Good case, Excellent value.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Great for the jawbone.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tied to charger for conversations lasting more...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The mic is great.</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_amazon.shape
```




    (1000, 2)




```python
data_imdb = pd.read_csv('data/imdb_labelled.txt',sep='\t',header=None)
data_imdb.head()
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
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A very, very, very slow-moving, aimless movie ...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Not sure who was more lost - the flat characte...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Attempting artiness with black &amp; white and cle...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Very little music or anything to speak of.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The best scene in the movie was when Gerardo i...</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



# imdb_labelled.txt 데이터는 아래 링크를 클릭 하면 보실 수 있습니다.
# 링크 - [https://github.com/SEONGJAE-YOO/Natural-Language-Processing-NLP-in-Python-with-8-Projects/blob/main/data/imdb_labelled.txt](https://github.com/SEONGJAE-YOO/Natural-Language-Processing-NLP-in-Python-with-8-Projects/blob/main/data/imdb_labelled.txt)


```python
data_imdb.columns = columan_name
```


```python
data_imdb.head()
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
      <th>Sentiment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A very, very, very slow-moving, aimless movie ...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Not sure who was more lost - the flat characte...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Attempting artiness with black &amp; white and cle...</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Very little music or anything to speak of.</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>The best scene in the movie was when Gerardo i...</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_imdb.shape
```




    (748, 2)




```python
# Append all the data in a single dataframe
```


```python
data = data_yelp.append([data_amazon, data_imdb],ignore_index=True)
```


```python
data.shape
```




    (2748, 2)




```python
data.head()
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
      <th>Sentiment</th>
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
# check distribution of sentiments
```


```python
data['Sentiment'].value_counts()

# 1386 positive reviews
# 1362 Negative reviews
```




    1    1386
    0    1362
    Name: Sentiment, dtype: int64




```python
# check for null values
data.isnull().sum()

# no null values in the data
```




    Review       0
    Sentiment    0
    dtype: int64




```python
x = data['Review']
y = data['Sentiment']
```

# 2) Data Cleaning


```python
# here we will remove stopwords, punctuations
# as well as we will apply lemmatization
```

## Create a function to clean the data

# string.punctuation 은 무엇일까?

# 참고사이트 - [https://www.delftstack.com/ko/howto/python/how-to-strip-punctuation-from-a-string-in-python/](https://www.delftstack.com/ko/howto/python/how-to-strip-punctuation-from-a-string-in-python/)


```python
import string
```


```python
punct = string.punctuation
```


```python
punct
```




    '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'




```python
from spacy.lang.en.stop_words import STOP_WORDS
```


```python
stopwords = list(STOP_WORDS) # list of stopwords
```


```python
stopwords
```




    ['most',
     "'s",
     'doing',
     'but',
     'after',
     'ten',
     'seems',
     'them',
     'several',
     'hereby',
     'beside',
     'behind',
     'hers',
     'seem',
     'four',
     'none',
     'for',
     '‘m',
     'those',
     'down',
     'former',
     'each',
     'less',
     'its',
     'two',
     'top',
     'third',
     'across',
     'hereupon',
     '’d',
     'themselves',
     'my',
     'latter',
     'how',
     'used',
     'hence',
     'twenty',
     'take',
     'amount',
     'every',
     'same',
     'everyone',
     'thereby',
     'show',
     "'d",
     'ever',
     'therein',
     'whether',
     'of',
     'our',
     'thence',
     'some',
     'bottom',
     'ca',
     'except',
     'nobody',
     'whole',
     'has',
     'his',
     'although',
     '’ve',
     'may',
     'otherwise',
     'twelve',
     'seeming',
     'n‘t',
     'also',
     'to',
     'next',
     'nor',
     'more',
     'with',
     'enough',
     'whereafter',
     'hereafter',
     'whereas',
     'others',
     'fifteen',
     '’m',
     'ours',
     'onto',
     'becomes',
     'anything',
     'sometimes',
     'formerly',
     'above',
     'whom',
     "'ve",
     'first',
     'indeed',
     'get',
     '’s',
     'an',
     'eight',
     'nowhere',
     'the',
     'wherein',
     'still',
     'anyone',
     'amongst',
     'sometime',
     'up',
     'further',
     'nevertheless',
     'rather',
     'against',
     '‘d',
     'ourselves',
     'made',
     'must',
     'under',
     'could',
     'thus',
     'not',
     'had',
     'out',
     'least',
     'either',
     'do',
     'beyond',
     'nine',
     'alone',
     'please',
     "'ll",
     'she',
     '‘ll',
     'everything',
     '‘re',
     '’ll',
     'last',
     'via',
     'unless',
     'name',
     'so',
     'anyhow',
     'other',
     'one',
     'quite',
     'mostly',
     'really',
     'yet',
     'both',
     'too',
     'just',
     'whence',
     'their',
     'becoming',
     'than',
     'during',
     'these',
     'empty',
     'herself',
     'call',
     'whose',
     'now',
     'besides',
     'here',
     'any',
     'her',
     'am',
     'would',
     'have',
     'he',
     'whoever',
     'yourself',
     'until',
     'be',
     'always',
     'put',
     'elsewhere',
     'from',
     'been',
     'latterly',
     'it',
     'itself',
     'such',
     'if',
     'at',
     'namely',
     'being',
     'thereupon',
     'were',
     'a',
     'me',
     'five',
     'hundred',
     'whenever',
     'they',
     'afterwards',
     'front',
     'various',
     'did',
     'who',
     'in',
     'within',
     "'re",
     'over',
     'give',
     'many',
     'as',
     'thereafter',
     'into',
     "'m",
     'perhaps',
     'using',
     'move',
     'are',
     'well',
     'another',
     'whatever',
     'own',
     'i',
     'became',
     'while',
     '‘s',
     'before',
     "n't",
     'what',
     'never',
     'something',
     'can',
     'mine',
     'though',
     'full',
     'once',
     'anyway',
     'someone',
     'neither',
     'us',
     'since',
     're',
     'without',
     'will',
     'almost',
     'therefore',
     'somewhere',
     'was',
     'n’t',
     'whither',
     'off',
     'often',
     'six',
     'because',
     'does',
     'about',
     'beforehand',
     'might',
     'herein',
     'below',
     'then',
     'on',
     'that',
     'or',
     'very',
     'fifty',
     'see',
     'whereby',
     'nothing',
     'even',
     'make',
     'much',
     'together',
     'myself',
     'which',
     'due',
     'again',
     'few',
     'upon',
     'anywhere',
     'else',
     'per',
     'when',
     'where',
     'only',
     'moreover',
     'everywhere',
     'say',
     'cannot',
     'him',
     'three',
     '’re',
     'throughout',
     'your',
     'along',
     'side',
     'should',
     'done',
     'no',
     'all',
     'this',
     'toward',
     '‘ve',
     'there',
     'we',
     'towards',
     'sixty',
     'through',
     'you',
     'serious',
     'and',
     'already',
     'back',
     'wherever',
     'between',
     'around',
     'is',
     'however',
     'noone',
     'by',
     'become',
     'meanwhile',
     'whereupon',
     'among',
     'thru',
     'yours',
     'go',
     'yourselves',
     'seemed',
     'himself',
     'regarding',
     'somehow',
     'part',
     'why',
     'forty',
     'keep',
     'eleven']




```python
# creating a function for data cleaning
```


```python
import spacy 

nlp = spacy.load('en_core_web_sm')

def text_data_cleaning(sentence):
  doc = nlp(sentence)

  tokens = [] # list of tokens
  for token in doc:
    if token.lemma_ != "-PRON-":
      temp = token.lemma_.lower().strip()
    else:
      temp = token.lower_
    tokens.append(temp)
  
  cleaned_tokens = []
  for token in tokens:
    if token not in stopwords and token not in punct:
      cleaned_tokens.append(token)
  return cleaned_tokens
```


```python
# if root form of that word is not pronoun then it is going to convert that into lower form
# and if that word is a proper noun, then we are directly taking lower form, because there is no lemma for proper noun
```


```python
text_data_cleaning("Hello all, It's a beautiful day outside there!")
# stopwords and punctuations removed
```




    ['hello', 'beautiful', 'day', 'outside']



## Vectorization Feature Engineering (TF-IDF)


```python
from sklearn.svm import LinearSVC
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.pipeline import Pipeline
```


```python
tfidf = TfidfVectorizer(tokenizer=text_data_cleaning)
# tokenizer=text_data_cleaning, tokenization will be done according to this function
```


```python
classifier = LinearSVC()
```

# 3) Train the model

## Splitting the dataset into the Train and Test set


```python
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = 0.2, random_state = 0)
```


```python
x_train.shape, x_test.shape
# 2198 samples in training dataset and 550 in test dataset
```




    ((2198,), (550,))




```python
x_train.head()
```




    2572    An Italian reviewer called this "a small, grea...
    526                          And it was way to expensive.
    1509    As an earlier review noted, plug in this charg...
    144     Nice blanket of moz over top but i feel like t...
    2483    The film gives meaning to the phrase, "Never i...
    Name: Review, dtype: object



## Fit the x_train and y_train


```python
clf = Pipeline([('tfidf',tfidf), ('clf',classifier)])
# it will first do vectorization and then it will do classification
```


```python
clf.fit(x_train, y_train)
```




    Pipeline(steps=[('tfidf',
                     TfidfVectorizer(tokenizer=<function text_data_cleaning at 0x0000015BB7D451F0>)),
                    ('clf', LinearSVC())])




```python
# in this we don't need to prepare the dataset for testing(x_test)
```

# 4) Predict the Test set results


```python
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
```


```python
y_pred = clf.predict(x_test)
```


```python
# confusion_matrix
confusion_matrix(y_test, y_pred)
```




    array([[203,  76],
           [ 49, 222]], dtype=int64)




```python
# classification_report
print(classification_report(y_test, y_pred))
# we are getting almost 77% accuracy
```

                  precision    recall  f1-score   support
    
               0       0.81      0.73      0.76       279
               1       0.74      0.82      0.78       271
    
        accuracy                           0.77       550
       macro avg       0.78      0.77      0.77       550
    weighted avg       0.78      0.77      0.77       550




```python
accuracy_score(y_test, y_pred)
# 77% accuracy
```




    0.7727272727272727




```python
clf.predict(["Wow, I am learning Natural Language Processing in fun fashion!"])
# output is 1, that means review is positive
```




    array([1], dtype=int64)




```python
clf.predict(["It's hard to learn new things!"])
# output is 0, that means review is Negative
```




    array([0], dtype=int64)


