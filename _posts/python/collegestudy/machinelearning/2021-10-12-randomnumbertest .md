---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: Inverse transformation 구현
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-10-12
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---

# Random number 생성하기

Uniform random number로부터 y=ax+b 형태의 random number를 두 가지 방법으로 구현 하세요.

구현한 소스코드와 결과를 histogram에 채워 원하는 결과가 나오는지 검증도 함께 해 주세요.

이번 과제는 파이썬 코드 작성 연습을 겸하므로, 가능한 한 파이썬 기본 기능만 사용해 구현해 주세요.

(np.random 을 포함한 기능은 이번에는 사용하지 않음)



1. Inverse transformation 방법을 이용하기.

(cumulative distribution function을 구하고, inverse를 구해 [0-1) 구간의 random number를 넣어 만들기

유도 과정도 함께 보이세요.



2. Acceptance-Rejection방법을 이용하기

# 방법1
사이파이(SciPy)는 수치해석기능을 제공하는 파이썬 패키지다. 여러 서브패키지로 구성되어 있는데 그중 stats 서브패키지는 확률분포 분석을 위한 다양한 기능을 제공한다. 다음 코드로 임포트한다.


```python
import scipy as sp
import scipy.stats
```

# 확률분포 클래스

사이파이에서 확률분포 기능을 사용하려면 우선 해당 확률분포에 대한 확률분포 클래스 객체를 생성한 후에 이 객체의 메서드를 호출해야 한다.

확률분포 객체를 생성하는 명령에는 다음과 같은 것들이 있다.


```python
from IPython.display import Image  # 주피터 노트북에 이미지 삽입
Image("C://Users/MyCom/jupyter-tutorial/수업자료/응용/data/20211009_035431_1.png")

```





![output_4_0](./img/machinelearning/06/output_4_0.png)





```python
Image("C://Users/MyCom/jupyter-tutorial/수업자료/응용/data/20211009_035431_2.png")

```





![output_5_0](./img/machinelearning/06/output_5_0.png)




이 명령들은 모두 stats 서브패키지에 포함되어 있다. 예를 들어 정규분포 객체는 다음과 같이 생성한다.


```python
rv = sp.stats.norm()

```

# 모수 지정

확률분포 객체를 생성할 때는 분포의 형상을 구체적으로 지정하는 **모수(parameter)**를 인수로 주어야 한다. 각 확률분포마다 설정할 모수가 다르므로 자세한 설명은 사이파이 문서를 참조한다. 하지만 대부분 다음과 같은 모수를 공통적으로 가진다.



loc

일반적으로 분포의 기댓값

scale

일반적으로 분포의 표준편차

예를 들어 기댓값이 1이고 표준 편차가 2인 정규분포 객체는 다음과 같이 생성한다.


```python
rv = sp.stats.norm(loc=1, scale=2)

```

# 확률분포 메서드

확률분포 객체가 가지는 메서드는 다음과 같다.


```python
Image("C://Users/MyCom/jupyter-tutorial/수업자료/응용/data/20211009_040306_1.png")

```





![output_13_0](./img/machinelearning/06/output_13_0.png)




각 메서드 사용법은 다음과 같다.

# 확률밀도함수

pdf 메서드는 연속확률변수의 확률밀도함수의 역할을 한다. 표본 값을 입력하면 해당 표본 값에 대한 확률밀도를 출력한다.


linspace() 함수는 파이썬의 numpy 모듈에 포함된 함수로서 1차원 배열 만들기에 사용할 수 있는 함수이다.

x=np.linspace(start,stop,num)

start는 배열의 시작값, stop은 배열의 끝값이고, num은 start와 stop 사이를 몇개의 일정한 간격으로 요소를 만들 것인지를 나타내는 것이다.
만일 num을 생략하면 디폴트로 50개의 수열,1차원 배열을 만들어줍니다.

ex) x=np.linspace(0,10,11)

print(x)

결과값

[0,1,2,3,4,5,6,7,8,9,10]

참고
https://m.blog.naver.com/choi_s_h/221730568009


```python
# matplotlib 한글 폰트 출력코드
# 출처 : 데이터공방( https://kiddwannabe.blog.me)

import matplotlib
from matplotlib import font_manager, rc
import platform

try : 
    if platform.system() == 'Windows':
    # 윈도우인 경우
        font_name = font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
        rc('font', family=font_name)
    else:    
    # Mac 인 경우
        rc('font', family='AppleGothic')
except : 
    pass
matplotlib.rcParams['axes.unicode_minus'] = False   
```


```python
import numpy as np
xx = np.linspace(-8, 8, 100)
pdf = rv.pdf(xx)
plt.plot(xx, pdf)
plt.title("확률밀도함수 ")
plt.xlabel("$x$")
plt.ylabel("$p(x)$")
plt.show()

```



![output_17_0](./img/machinelearning/06/output_17_0.png)



# 누적분포함수

cdf 메서드는 이산확률변수와 연속확률변수의 누적분포함수의 역할을 한다. 표본 값을 입력하면 해당 표본 값에 대한 누적확률을 출력한다.


```python
xx = np.linspace(-8, 8, 100)
cdf = rv.cdf(xx)
plt.plot(xx, cdf)
plt.title("누적분포함수 ")
plt.xlabel("$x$")
plt.ylabel("$F(x)$")
plt.show()
```



![output_19_0](./img/machinelearning/06/output_19_0.png)



# 무작위 표본 생성

무작위로 표본을 만들 때는 rvs(random value sampling) 메서드를 사용한다. 이 메서드에서 받는 인수는 다음과 같다.


```python
Image("C://Users/MyCom/jupyter-tutorial/수업자료/응용/data/20211009_043605_1.png")

```





![output_21_0](./img/machinelearning/06/output_21_0.png)





```python
rv.rvs(size=(3, 5), random_state=0)

```




    array([[ 4.52810469,  1.80031442,  2.95747597,  5.4817864 ,  4.73511598],
           [-0.95455576,  2.90017684,  0.69728558,  0.7935623 ,  1.821197  ],
           [ 1.28808714,  3.90854701,  2.52207545,  1.24335003,  1.88772647]])




```python
import seaborn as sns

sns.histplot(rv.rvs(size=1000, random_state=0))
plt.title("랜덤 표본 생성 결과")
plt.xlabel("표본값")
plt.ylabel("count")
plt.xlim(-8, 8)
plt.show()
```



![output_23_0](./img/machinelearning/06/output_23_0.png)



# 방법 2

# 확률 계산 - Inverse transform sampling


Inverse transform sampling(역변환 샘플링)은 확률 분포에서 샘플링하는 간단한 방법입니다. 표본을 독립적으로 추출하는 정확한 표본 추출 방법입니다.
이방법은 uniform U(0,1) 으로 부터 독립적인 샘플을 얻을 수 있도록 한다.
최신 컴퓨터는 의사 난수 생성기가 장착되어 있으며 U(0,1)으로 부터 샘플링을 하기 위해 numpy를 사용합니다.




- Algorithm

확률분포에서 샘플링한다고 가정하자. 누적 분포 함수는 다음과 같이 정의됩니다


```python
Image("C://Users/MyCom/jupyter-tutorial/수업자료/응용/20211012_161502_1.png")

```





![output_26_0](./img/machinelearning/06/output_26_0.png)




Inverse transform 샘플링은 이방법을 따른다




```python
Image("C://Users/MyCom/jupyter-tutorial/수업자료/응용/20211012_163948_1.png")

```





![output_28_0](./img/machinelearning/06/output_28_0.png)





```python
import numpy as np
def inv_cdf(p,lamb):
    return -np.log(1-p) / lamb
```

U(0,1)으로 부터 5000개 샘플링을 생성시켰습니다


```python
np.random.seed(42)
u = np.random.uniform(0,1,5000)
```

Inverse CDF을 사용하여 P으로 부터 샘플링을 얻을수 있다



```python
v = inv_cdf(u,5)# λ=5으로 가정하자
```

# Comparing the samples with np.random.exponential samples




```python
x = np.random.exponential(1/5,5000) # using scale =1 /5 , since scale = 1/ lambda

bins = np.linspace(0.0,1.7,20)
plt.figure(figsize=(8,6))
plt.hist(x, bins, alpha=0.4, label="numpy samples", color="green");
plt.hist(v, bins, alpha=0.4, label="our samples", color="blue");
plt.title("Samples obtained from numpy vs samples obtained with inversed transform sampling");
plt.show()

```



![output_35_0](./img/machinelearning/06/output_35_0.png)




```python

```


```python

```
