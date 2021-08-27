---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: beautifulsoup 모듈 - 01.beautifulsoup 모듈 사용하기
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-08-27
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}


### 학습목표
1. beautifulsoup 모듈 사용하기


```python
from bs4 import BeautifulSoup
```

#### html 문자열 파싱
- 문자열로 정의된 html 데이터 파싱하기


```python
html = '''
<html>
  <head>
    <title>BeautifulSoup test</title>
  </head>
  <body>
    <div id='upper' class='test' custom='good'>
      <h3 title='Good Content Title'>Contents Title</h3>
      <p>Test contents</p>
    </div>
    <div id='lower' class='test' custom='nice'>
      <p>Test Test Test 1</p>
      <p>Test Test Test 2</p>
      <p>Test Test Test 3</p>
    </div>
  </body>
</html>'''
```

#### find 함수
- 특정 html tag를 검색
- 검색 조건을 명시하여 찾고자하는 tag를 검색


```python
soup = BeautifulSoup(html)
```


```python
soup.find('h3')
```




    <h3 title="Good Content Title">Contents Title</h3>




```python
soup.find('p') #첫번째 p 찾아짐
```




    <p>Test contents</p>




```python
soup.find('div', class_='test') #첫번째 div 찾아짐
```




    <div class="test" custom="good" id="upper">
    <h3 title="Good Content Title">Contents Title</h3>
    <p>Test contents</p>
    </div>




```python
attrs = {'id': 'upper', 'class': 'test'}
soup.find('div', attrs=attrs)
```




    <div class="test" custom="good" id="upper">
    <h3 title="Good Content Title">Contents Title</h3>
    <p>Test contents</p>
    </div>




```python
soup.find('div', id='lower')
```




    <div class="test" custom="nice" id="lower">
    <p>Test Test Test 1</p>
    <p>Test Test Test 2</p>
    <p>Test Test Test 3</p>
    </div>



#### find_all 함수
- find가 조건에 만족하는 하나의 tag만 검색한다면, find_all은 조건에 맞는 모든 tag를 리스트로 반환


```python
soup.find_all('div', class_='test')
```




    [<div class="test" custom="good" id="upper">
     <h3 title="Good Content Title">Contents Title</h3>
     <p>Test contents</p>
     </div>,
     <div class="test" custom="nice" id="lower">
     <p>Test Test Test 1</p>
     <p>Test Test Test 2</p>
     <p>Test Test Test 3</p>
     </div>]



#### get_text 함수
- tag안의 value를 추출
- 부모tag의 경우, 모든 자식 tag의 value를 추출


```python
tag = soup.find('h3')
print(tag)
tag.get_text()
```

    <h3 title="Good Content Title">Contents Title</h3>
    




    'Contents Title'




```python
tag = soup.find('p')
print(tag)
tag.get_text()
```

    <p>Test contents</p>
    




    'Test contents'




```python
tag = soup.find('div', id='upper')
print(tag)
tag.get_text().strip() #불필요한 문자열 제거 :strip()
```

    <div class="test" custom="good" id="upper">
    <h3 title="Good Content Title">Contents Title</h3>
    <p>Test contents</p>
    </div>
    




    'Contents Title\nTest contents'



#### attribute 값 추출하기
- 경우에 따라 추출하고자 하는 값이 attribute에도 존재함
- 이 경우에는 검색한 tag에 attribute 이름을 [ ]연산을 통해 추출가능
- 예) div.find('h3')['title']


```python
tag = soup.find('h3')
print(tag)
tag['title']
```

    <h3 title="Good Content Title">Contents Title</h3>
    




    'Good Content Title'


