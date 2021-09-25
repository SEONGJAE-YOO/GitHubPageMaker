---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 머신러닝 프로그래밍 3주차 응용하기 - factorial
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-09-25
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}

```python
def factorial(n):
    if n > 1:
        return n * factorial(n-1)
    else:
        return 1
    
result0 = factorial(0)
result1 = factorial(1)
result2 = factorial(2)
result3 = factorial(3)
```


```python
# f 문자열 포맷
print(f'factorial(0) : {result0}')

print(f"factorial(1) : {result1}") 

print(f'factorial(2) : {result2}') 

print(f'factorial(3) : {result3}')
```

    factorial(0) : 1
    factorial(1) : 1
    factorial(2) : 2
    factorial(3) : 6



```python
# 반복문 사용하기
def factorial_range(n):
    result = 1
    for val in range(1, n+1): 
        result *= val
    return result
```


```python
result0 = factorial_range(1)
result2 = factorial_range(2)
result4 = factorial_range(4)
```


```python
print(f"factorial(0) : {result0}")
print(f"factorial(2) : {result2}")
print(f"factorial(4) : {result4}")
```

    factorial(0) : 1
    factorial(2) : 2
    factorial(4) : 24
    
