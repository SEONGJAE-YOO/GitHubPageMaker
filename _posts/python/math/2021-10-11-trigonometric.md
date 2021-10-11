---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: 삼각함수 (trigonometric functions) 
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-10-11
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---
{% include python-table-of-contents.html %}


# 삼각함수 개념


```python

# 각 방정식을 설정하기 위한 NumPy를 임포트
import numpy as np
# matplotlib의 pyplot를 plt로 임포트
import matplotlib.pyplot as plt

# 노트북 안에 그래프가 표시되도록
%matplotlib inline

# 한글폰트 사용 시 그래프에서 마이너스 폰트 깨지는 문제에 대한 대처
plt.rcParams['axes.unicode_minus'] = False
```

# 삼각함수 (trigonometric functions)
# https://rfriend.tistory.com/296

(1-4-1) 삼각함수 (trigonometric functions) : np.sin(), np.cos(), np.tan()



In [1]: import numpy as np



In [2]: np.sin(np.array((0., 30., 45., 60., 90.))*np.pi / 180.)

Out[2]: array([ 0.        ,  0.5       ,  0.70710678,  0.8660254 ,  1.        ])



In [3]: np.cos(np.array((0., 30., 45., 60., 90.))*np.pi / 180.)

Out[3]:

array([  1.00000000e+00,   8.66025404e-01,   7.07106781e-01,
5.00000000e-01,   6.12323400e-17])



In [4]: np.tan(np.array((0., 30., 45., 60., 90.))*np.pi / 180.)

Out[4]:

array([  0.00000000e+00,   5.77350269e-01,   1.00000000e+00,
1.73205081e+00,   1.63312394e+16])



출처: https://rfriend.tistory.com/296 [R, Python 분석과 프로그래밍의 친구 (by R Friend)]


```python
# x축의 영역과 정밀도를 설정하고 x 값을 준비
x = np.arange( -3, 3, 0.1 )
# 각 방정식의 y 값을 준비
y_sin = np.sin( x )
x_rand = np.random.rand(100) * 6 - 3
y_rand = np.random.rand(100) * 6 - 3
print(x_rand.shape)
print(y_rand.shape)
print(x_rand[:5])
print(y_rand[:5])
```

    (100,)
    (100,)
    [ 1.24034598 -1.12492699 -1.50293278 -0.36973249 -2.21762864]
    [ 0.61401993  1.65863263 -0.60985629 -1.20281078  1.23937135]



```python
# figure 객체를 생성
plt.figure()

# 1개의 그래프로 표시하는 설정
plt.subplot( 1, 1, 1 )

# 각 방정식의 선형과 마커, 라벨을 설정하고 플롯
## 선형도
plt.plot( x, y_sin, marker='o', markersize=5, label='line' )

## 산포도
plt.scatter( x_rand, y_rand, label='scatter' )

# 범례 표시를 설정
plt.legend()
# 그리드 라인을 표시
plt.grid( True )

# 그래프 표시
plt.show()
```



![output_4_0](./img/math/output_4_0.png)
    

