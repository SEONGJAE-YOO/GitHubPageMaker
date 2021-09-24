---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 머신러닝 프로그래밍 2주차 응용하기
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
print('hee dd','s','asd',sep='..')
```

    hee dd..s..asd



```python
print('heed','asd','asd',sep='0',end='\naa')
```

    heed0asd0asd
    aa


```python
help(print)
```

    Help on built-in function print in module builtins:
    
    print(...)
        print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
        
        Prints the values to a stream, or to sys.stdout by default.
        Optional keyword arguments:
        file:  a file-like object (stream); defaults to the current sys.stdout.
        sep:   string inserted between values, default a space.
        end:   string appended after the last value, default a newline.
        flush: whether to forcibly flush the stream.




```python
print("안녕하세요.\n만나서\t\t반갑습니다.")
```

    안녕하세요.
    만나서		반갑습니다.



```python
# naver;kakao;sk;samsung
print("naver","kakao","sk","samsung",sep=";")
```

    naver;kakao;sk;samsung



```python
# naver/kakao/sk/samsung
print("naver", "kakao", "samsung", sep="/")


```

    naver/kakao/samsung



```python
print("first", end=" ")
print("second")
```

    first second



```python
a="hello"

b="python"

c=a+b


print(c)
```

    hellopython



```python
print(5//3)
print(5/3)

```

    1
    1.6666666666666667



```python
시가총액 = 298000000000000
현재가 = 5000
PER = 15.79
print(시가총액, type(시가총액))
print(현재가, type(현재가))
print(PER, type(PER))
```

    298000000000000 <class 'int'>
    5000 <class 'int'>
    15.79 <class 'float'>



```python
s = "hello"
b = "python"
print(type(s))
print(s+"!",b)
```

    <class 'str'>
    hello! python



```python
num_str = "720"  #형변환
num_int = int(num_str)
print(num_int+1, type(num_int))

```

    721 <class 'int'>



```python
num = 100
result = str(num)
print(result, type(result))
```

    100 <class 'str'>



```python
#year라는 변수가 문자열 타입의 연도를 바인딩하고 있습니다. 이를 정수로 변환한 후 최근 3년의 연도를 화면에 출력해보세요.

year = "2020"
print(int(year)-3)  # 2017
print(int(year)-2)  # 2018
print(int(year)-1)  # 2019
```

    2017
    2018
    2019



```python
a= [1,"33",2]
b = ["123","dd"]
print(a,b)
```

    [1, '33', 2] ['123', 'dd']



```python
lang = 'python'
print(lang[0], lang[2])
```

    p t



```python
#자동차 번호가 다음과 같을 때 뒤에 4자리만 출력하세요.

#>> license_plate = "24가 2210"
#문자열에서 여러 글자를 가져오는 것을 슬라이싱이라고 부릅니다
license_plate = "24가 2210"
print(license_plate[-4:])
```

    2210



```python
#슬라이싱할 때 시작인덱스:끝인덱스:오프셋을 지정할 수 있습니다.



string = "홀짝홀짝홀짝"
print(string[::2])
```

    홀홀홀



```python
#문자열을 거꾸로 뒤집어 출력하세요.


string = "PYTHON"
print(string[::-1])
```

    NOHTYP



```python
import sys

sys.stdout.write('test')
```

    test


```python
print('right'.rjust(10))
```

         right



```python
for x in range(1,6):
    print(x,'*',x,'=',str(x*x).zfill(3)) #int, float -> str 변환

```

    1 * 1 = 001
    2 * 2 = 004
    3 * 3 = 009
    4 * 4 = 016
    5 * 5 = 025



```python
# >>> str(396)
# '396'
# >>> str(5.52)
# '5.52'
# >>> str(6.02e10)
# '60200000000.0'
# >>> str(6.02e20)
# '6.02e+20'
```


```python
# 포맷팅
# - 문자열 내에서 어떤 값이 들어가길 원하는 곳은 {}로 표시

print("{0} is {1}".format("apple","red"))
print("{0} and {1} and {2}".format("apple","red","green"))
```

    apple is red
    apple and red and green



```python
# format의 인자로 키,값을 주어 {} 안의 값을 지정
print("{item} is {color}".format(item='apple',color='red'))
```

    apple is red



```python
"apple".endswith("e")
```




    True




```python
"333".isnumeric() #문자열이 숫자로 구성되어 있는지 확인해주는 함수
```




    True




```python
2**3
```




    8



Operator

    +	더하기	a + b = 30

    -	빼기	a - b = -10

    *	곱하기	a * b = 200

    /	나누기	b / a = 2.0

    %	나머지	b % a = 0

    **	제곱	a ** c = 1000

    //	몫	a // c = 3


```python
"wdwd"*2
```




    'wdwdwdwd'




```python
"awdws".upper()
```




    'AWDWS'




```python
"TTSD".lower()
```




    'ttsd'




```python
#변수의 값을 문자열에 포함하기
a="도윤"

b="축구"

c=3

d=99.5

e="나는 어제 %s이랑 %s를 했어."

f="%s이는 축구공을 %d개 가지고 있어."

g="%s이의 이번 시험 평균 점수는 %5.1f점이야."


print(e %(a, b))

print(f %(a, c))

print(g %(a, d))


```

    나는 어제 도윤이랑 축구를 했어.
    도윤이는 축구공을 3개 가지고 있어.
    도윤이의 이번 시험 평균 점수는  99.5점이야.



```python
a="Hello python"
b=a[:5] #앞에서부터 1이다(문자열일경우, 리스트는 앞에서부터 0이다)

c=a[6:]

d=a[1]


print(b)

print(c)

print(d)
```

    Hello
    python
    e



```python
aa=[1, 2, 3, 4, 5]

bb=[1, 2, 3, 4, 5]

aa.append(6)

bb.insert(2, 10)


print(aa)

print(bb)
```

    [1, 2, 3, 4, 5, 6]
    [1, 2, 10, 3, 4, 5]



```python
aa={'성별':'남', '번호':15, '이름':'홍길동'}


print(aa['성별'])

print(aa['번호'])
```

    남
    15



```python
#정수형 변수 c와 d를 입력받아 “(입력한 수 중 큰 수)가 (입력한 수 중 작은 수)보다 큽니다.”라고 출력되고, 
#두 수가 같으면 “두 수는 같습니다.”라고 출력되는 프로그램이 
#되도록 코딩해 보자.

c=int(input("정수를 입력해 주세요.:"))

d=int(input("정수를 입력해 주세요.:"))


if c>d:

    print("%d이(가) %d보다 큽니다." % (c, d))

else:

    if d>c:

        print("%d이(가) %d보다 큽니다." % (d, c))

    else:

        print("두 수는 같습니다.")
```

    정수를 입력해 주세요.:1
    정수를 입력해 주세요.:3
    3이(가) 1보다 큽니다.
    
