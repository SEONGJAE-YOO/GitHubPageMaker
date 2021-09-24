---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 머신러닝 프로그래밍 3주차 응용하기
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-09-24
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}

```python
#함수선언
def test(a,b,c=1,d=2):
    return a+b+c+d

print(test(5,4))
```

    12



```python
def test(a,b,c=1,d=2):
    return a+b+c+d
print(test(5,4,5,5))
```

    19



```python
def add(a,b):
    return a +b
```


```python
print(add(3,4))
```

    7



```python
# 결과값이 없는 함수
def add(a,b):
    print("%d, %d의 합은 %d입니다."%(a,b,a+b))
```


```python
add(2,2)
```

    2, 2의 합은 4입니다.



```python
 def add_many(*args): 
        result = 0 
        for i in args: 
            result = result + i 
        return result 
```


```python
print(add_many(5,5,5))
```

    15



```python
def func(a1, a2, *args, **params):  #*은 튜플, ** 딕셔너리
    return a1,a2,args,params
```


```python
func('A','B','C','D',k1='k1',k2='k2')
```




    ('A', 'B', ('C', 'D'), {'k1': 'k1', 'k2': 'k2'})




```python
args =('c','d')
params = {'k1' :'k1', 'k2' : 'k2'}
func('a','b',*args,**params)
```




    ('a', 'b', ('c', 'd'), {'k1': 'k1', 'k2': 'k2'})




```python
def test(*val): #인자값이 얼마나 올지 모를때 이렇게 사용한다
    for i in val:
        print(i)
    
test(1,2)
print('')
test(11,33,33,'asd')
```

    1
    2
    
    11
    33
    33
    asd



```python
def sum_mul(choice, *val):
    if choice =="sum":
        result =0
        for i in val:
            result =result +i
    elif choice == "mul":
        result = 1
        for i in val:
            result = result * i
    return result

print(sum_mul("sum",1,2,3))
print(sum_mul("mul",1,2,3,4,5))
```

    6
    120



```python
def say_myself(name,age,man=True):
    print("이름은: %s"%name)
    print("나이는: %d" %age)
    if man:
        print("남자")
    else:
        print("여자")
        
say_myself("수진",33) #초기값은 true이다
say_myself("수진",34,False)
```

    이름은: 수진
    나이는: 33
    남자
    이름은: 수진
    나이는: 34
    여자



```python
# 람다식 : 이름 없는 작은 함수
myfunc = lambda x,y : x +y
print(myfunc(3,5))
```

    8



```python
# class
class Human:
    pass #비어있는 클래스 정의
```


```python
#클래스의 인스턴스를 생성하고 이를 areum 변수로 바인딩 해보자

class Human:
    pass

areum = Human()
```


```python
#Human 클래스에 "응애응애"를 출력하는 생성자를 추가해보세요

class Human:
    def __init__(self): #init 은 class Human 의 객체가 만들어질 때 기본으로 실행되는 메서드로, 초기화함수라고도 한다.
        print("응애응애")
        
areum = Human()

```

    응애응애



```python
#Human 클래스에 이름, 나이, 성별 을 받는 생성자를 추가해보세요.

class Human:
    def __init__(self, name, age, sex):
        self.name=name
        self.age=age
        self.sex=sex
        
areum = Human("아름",25,"여자")
print(areum.name)
print(areum.age)
print(areum.sex)
```

    아름
    25
    여자


https://blog.naver.com/suy_k07/222376004345
