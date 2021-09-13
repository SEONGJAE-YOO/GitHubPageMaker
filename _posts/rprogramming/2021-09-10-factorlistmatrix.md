---
date: 2021-09-06 15:23:00
layout: page
current: page
cover:  assets/built/images/R-logo.png
navigation: True
title: 팩터와 리스트 그리고 행렬
tags: [rprogramming]  
class: post-template
subclass: 'post tag-rprogramming'
author: SeongJae Yu
sitemap:  
lastmod: 2021-09-10
priority: 0.7  
changefreq: 'monthly'
exclude: 'yes'

---
{% include python-table-of-contents.html %}


# 팩터란?
## 연속형 데이터 : 키,몸무게와 같이 숫자형으로 표현 가능한 데이터
## 범주형 데이터 : 성별,혈액형과 같이 범주(카테고리)로만 표현 가능한 데이터
    -범주형 데이터는 산술 연산과 논리 연산이 불가하므로 팩터로 다룸 
    - vector(벡터), 팩터, 리스트는 1차원
    - 데이터프레임, 행렬(matrix)는 2차원
    - array는 3차원,N차원 


```R
v.1 <- c('M','F','M','F')
```


```R
v.1
```


<ol class=list-inline>
	<li>'M'</li>
	<li>'F'</li>
	<li>'M'</li>
	<li>'F'</li>
</ol>




```R
f.1 <- factor(v.1)
```


```R
f.1
```


<ol class=list-inline>
	<li>M</li>
	<li>F</li>
	<li>M</li>
	<li>F</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'F'</li>
		<li>'M'</li>
	</ol>
</details>



```R
class(v.1)
```


'character'



```R
class(f.1)
```


'factor'



```R
str(v.1)
```

     chr [1:4] "M" "F" "M" "F"



```R
str(f.1)
```

     Factor w/ 2 levels "F","M": 2 1 2 1



```R
# 혈액형 팩터

v.2 <- c('A','B','AB','O')
v.2
```


<ol class=list-inline>
	<li>'A'</li>
	<li>'B'</li>
	<li>'AB'</li>
	<li>'O'</li>
</ol>




```R
f.2 <- factor(c('A','B','O','A','B'),levels = v.2)
```


```R
f.2
```


<ol class=list-inline>
	<li>A</li>
	<li>B</li>
	<li>O</li>
	<li>A</li>
	<li>B</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'A'</li>
		<li>'B'</li>
		<li>'AB'</li>
		<li>'O'</li>
	</ol>
</details>



```R
f.2[length(f.2)+1] <- 'AB'
```


```R
f.2
```


<ol class=list-inline>
	<li>A</li>
	<li>B</li>
	<li>O</li>
	<li>A</li>
	<li>B</li>
	<li>AB</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'A'</li>
		<li>'B'</li>
		<li>'AB'</li>
		<li>'O'</li>
	</ol>
</details>



```R
f.2[1] <- 'O'
```


```R
f.2
```


<ol class=list-inline>
	<li>O</li>
	<li>B</li>
	<li>O</li>
	<li>A</li>
	<li>B</li>
	<li>AB</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'A'</li>
		<li>'B'</li>
		<li>'AB'</li>
		<li>'O'</li>
	</ol>
</details>



```R
f.2[2] <- 'x' #없는 levels으로 지정하면 오류남
```

    Warning message in `[<-.factor`(`*tmp*`, 2, value = "x"):
    "invalid factor level, NA generated"


```R
f.2
```


<ol class=list-inline>
	<li>O</li>
	<li>&lt;NA&gt;</li>
	<li>O</li>
	<li>A</li>
	<li>B</li>
	<li>AB</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'A'</li>
		<li>'B'</li>
		<li>'AB'</li>
		<li>'O'</li>
	</ol>
</details>


- factor은 vector의 한종류로 범주형데이터를 levels으로 지정할때 사용함

# 리스트란?


## 변수 : 통계학에서 측정하고자 하는 데이터를 의미
## 리스트: 원소의 자료형이 서로 다른 벡터들의 집합


```R
lst <- list(name='주니온', gender='남', age=20, hansome=T)
```


```R
lst
```


<dl>
	<dt>$name</dt>
		<dd>'주니온'</dd>
	<dt>$gender</dt>
		<dd>'남'</dd>
	<dt>$age</dt>
		<dd>20</dd>
	<dt>$hansome</dt>
		<dd>TRUE</dd>
</dl>




```R
typeof(lst)
```


'list'



```R
class(lst)
```


'list'



```R
str(lst)
```

    List of 4
     $ name   : chr "주니온"
     $ gender : chr "남"
     $ age    : num 20
     $ hansome: logi TRUE



```R
v.1 <- c('홍길동','전우치','로미오','줄리엣')
v.2 <- c('남','남','남','여')
v.3 <- c(18,19,17,15)
v.4 <- c(T,T,F,F)
```


```R
lst.2 <- list(name=v.1,gender=v.2,age=v.3,oriental=v.4)
```


```R
lst.2
```


<dl>
	<dt>$name</dt>
		<dd><ol class=list-inline>
	<li>'홍길동'</li>
	<li>'전우치'</li>
	<li>'로미오'</li>
	<li>'줄리엣'</li>
</ol>
</dd>
	<dt>$gender</dt>
		<dd><ol class=list-inline>
	<li>'남'</li>
	<li>'남'</li>
	<li>'남'</li>
	<li>'여'</li>
</ol>
</dd>
	<dt>$age</dt>
		<dd><ol class=list-inline>
	<li>18</li>
	<li>19</li>
	<li>17</li>
	<li>15</li>
</ol>
</dd>
	<dt>$oriental</dt>
		<dd><ol class=list-inline>
	<li>TRUE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>
</dd>
</dl>




```R
names(lst.2)
```


<ol class=list-inline>
	<li>'name'</li>
	<li>'gender'</li>
	<li>'age'</li>
	<li>'oriental'</li>
</ol>




```R
lst.2$name
```


<ol class=list-inline>
	<li>'홍길동'</li>
	<li>'전우치'</li>
	<li>'로미오'</li>
	<li>'줄리엣'</li>
</ol>




```R
lst.2[1]  #LIST를 출력해줌
```


<strong>$name</strong> = <ol class=list-inline>
<li>'홍길동'</li>
<li>'전우치'</li>
<li>'로미오'</li>
<li>'줄리엣'</li>
</ol>




```R
lst.2[[1]] #벡터를 출력해줌
```


<ol class=list-inline>
	<li>'홍길동'</li>
	<li>'전우치'</li>
	<li>'로미오'</li>
	<li>'줄리엣'</li>
</ol>




```R
typeof(lst.2[1])
```


'list'



```R
class(lst.2[1])
```


'list'



```R
typeof(lst.2[[1]]) #벡터가 character임
```


'character'



```R
lst.2[1:3]
```


<dl>
	<dt>$name</dt>
		<dd><ol class=list-inline>
	<li>'홍길동'</li>
	<li>'전우치'</li>
	<li>'로미오'</li>
	<li>'줄리엣'</li>
</ol>
</dd>
	<dt>$gender</dt>
		<dd><ol class=list-inline>
	<li>'남'</li>
	<li>'남'</li>
	<li>'남'</li>
	<li>'여'</li>
</ol>
</dd>
	<dt>$age</dt>
		<dd><ol class=list-inline>
	<li>18</li>
	<li>19</li>
	<li>17</li>
	<li>15</li>
</ol>
</dd>
</dl>




```R
lst.2[[1]]
```


<ol class=list-inline>
	<li>'홍길동'</li>
	<li>'전우치'</li>
	<li>'로미오'</li>
	<li>'줄리엣'</li>
</ol>




```R
lst.2[1:3][3]
```


<strong>$age</strong> = <ol class=list-inline>
<li>18</li>
<li>19</li>
<li>17</li>
<li>15</li>
</ol>




```R
lst.2[[1]][3]
```


'로미오'


# 행렬이란?
## 2차원 데이터
## 행,열로 구성된 벡터의 집합
## 행렬의 모든 원소는 같은 자료형
## 행렬의 생성: matrix() 함수
- matrix(x, nrow, ncol, byrow)


```R
m <- matrix(1:12, nrow=3, ncol=4)
```


```R
m
```


<table>
<tbody>
	<tr><td>1 </td><td>4 </td><td>7 </td><td>10</td></tr>
	<tr><td>2 </td><td>5 </td><td>8 </td><td>11</td></tr>
	<tr><td>3 </td><td>6 </td><td>9 </td><td>12</td></tr>
</tbody>
</table>




```R
m <- matrix(1:12, nrow=4, ncol=3, byrow=T)
```


```R
m
```


<table>
<tbody>
	<tr><td> 1</td><td> 2</td><td> 3</td></tr>
	<tr><td> 4</td><td> 5</td><td> 6</td></tr>
	<tr><td> 7</td><td> 8</td><td> 9</td></tr>
	<tr><td>10</td><td>11</td><td>12</td></tr>
</tbody>
</table>




```R
v.1 <- 1:3
```


```R
v.1
```


<ol class=list-inline>
	<li>1</li>
	<li>2</li>
	<li>3</li>
</ol>




```R
v.2 <- 4:6
```


```R
v.2
```


<ol class=list-inline>
	<li>4</li>
	<li>5</li>
	<li>6</li>
</ol>




```R
m.1 <-rbind(v.1,v.2)
```


```R
m.1
```


<table>
<tbody>
	<tr><th scope=row>v.1</th><td>1</td><td>2</td><td>3</td></tr>
	<tr><th scope=row>v.2</th><td>4</td><td>5</td><td>6</td></tr>
</tbody>
</table>




```R
m.2 <- cbind(v.1,v.2)
```


```R
m.2
```


<table>
<thead><tr><th scope=col>v.1</th><th scope=col>v.2</th></tr></thead>
<tbody>
	<tr><td>1</td><td>4</td></tr>
	<tr><td>2</td><td>5</td></tr>
	<tr><td>3</td><td>6</td></tr>
</tbody>
</table>




```R
m.3 <- rbind(m.1,v.1)
```


```R
m.3
```


<table>
<tbody>
	<tr><th scope=row>v.1</th><td>1</td><td>2</td><td>3</td></tr>
	<tr><th scope=row>v.2</th><td>4</td><td>5</td><td>6</td></tr>
	<tr><th scope=row>v.1</th><td>1</td><td>2</td><td>3</td></tr>
</tbody>
</table>




```R
m.4 <-cbind(m.2,v.2)
```


```R
m.4
```


<table>
<thead><tr><th scope=col>v.1</th><th scope=col>v.2</th><th scope=col>v.2</th></tr></thead>
<tbody>
	<tr><td>1</td><td>4</td><td>4</td></tr>
	<tr><td>2</td><td>5</td><td>5</td></tr>
	<tr><td>3</td><td>6</td><td>6</td></tr>
</tbody>
</table>




```R
str(m.1)
```

     int [1:2, 1:3] 1 4 2 5 3 6
     - attr(*, "dimnames")=List of 2
      ..$ : chr [1:2] "v.1" "v.2"
      ..$ : NULL



```R
names(m.1)
```


    NULL



```R
rownames(m.1)
```


<ol class=list-inline>
	<li>'v.1'</li>
	<li>'v.2'</li>
</ol>




```R
dim(m.1) #행렬 row,col 숫자반환
```


<ol class=list-inline>
	<li>2</li>
	<li>3</li>
</ol>




```R
#행렬의 인덱싱
m <- matrix(1:25,nrow=5,ncol=5,byrow=T)
```


```R
m
```


<table>
<tbody>
	<tr><td> 1</td><td> 2</td><td> 3</td><td> 4</td><td> 5</td></tr>
	<tr><td> 6</td><td> 7</td><td> 8</td><td> 9</td><td>10</td></tr>
	<tr><td>11</td><td>12</td><td>13</td><td>14</td><td>15</td></tr>
	<tr><td>16</td><td>17</td><td>18</td><td>19</td><td>20</td></tr>
	<tr><td>21</td><td>22</td><td>23</td><td>24</td><td>25</td></tr>
</tbody>
</table>




```R
#m[row,col]
m[1,1]
```


1



```R
m[,3]
```


<ol class=list-inline>
	<li>3</li>
	<li>8</li>
	<li>13</li>
	<li>18</li>
	<li>23</li>
</ol>




```R
m[1:3,4:5]
```


<table>
<tbody>
	<tr><td> 4</td><td> 5</td></tr>
	<tr><td> 9</td><td>10</td></tr>
	<tr><td>14</td><td>15</td></tr>
</tbody>
</table>




```R
class(m[1,])
```


'integer'



```R
class(m[1:3])
```


'integer'



```R
m[1:3]
```


<ol class=list-inline>
	<li>1</li>
	<li>6</li>
	<li>11</li>
</ol>




```R
class(m[1:3,4:5])
```


'matrix'

