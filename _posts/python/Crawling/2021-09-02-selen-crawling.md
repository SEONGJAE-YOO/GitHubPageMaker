---
layout: page
current: page
cover:  assets/built/images/logo-python.png
navigation: True
title: selenium 모듈 - 01.사이트에 로그인하여 데이터 크롤링하기
tags: [python]  
class: post-template
subclass: 'post tag-python'
author: SeongJae Yu
sitemap:
lastmod: 2021-09-02
priority: 0.7
changefreq: 'monthly'
exclude: 'yes'
---

### 학습목표
1. 다음 뉴스 댓글 개수 크롤링
2. 로그인 하여 크롤링 하기


```python
import requests
```

#### HTTP 상태 코드
- 1xx (정보): 요청을 받았으며 프로세스를 계속한다
- 2xx (성공): 요청을 성공적으로 받았으며 인식했고 수용하였다
- 3xx (리다이렉션): 요청 완료를 위해 추가 작업 조치가 필요하다
- 4xx (클라이언트 오류): 요청의 문법이 잘못되었거나 요청을 처리할 수 없다
- 5xx (서버 오류): 서버가 명백히 유효한 요청에 대해 충족을 실패했다

[출처: 위키피디아](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)


```python
url = 'https://comment.daum.net/apis/v1/ui/single/main/@20190728165812603'

headers = {
    'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb3J1bV9rZXkiOiJuZXdzIiwiZ3JhbnRfdHlwZSI6ImFsZXhfY3JlZGVudGlhbHMiLCJzY29wZSI6W10sImV4cCI6MTYyMDA2MDQ5MCwiYXV0aG9yaXRpZXMiOlsiUk9MRV9DTElFTlQiXSwianRpIjoiYzc2OWUzZjgtYWM4YS00ZjRlLWI2MmItY2M2YmI3MDg3MTIwIiwiZm9ydW1faWQiOi05OSwiY2xpZW50X2lkIjoiMjZCWEF2S255NVdGNVowOWxyNWs3N1k4In0.d3LldE7jAmpShlwhNh17irQ14a1qHkZVRCcXsTNgmaM',
    'Origin': 'https://news.v.daum.net',
    'Referer': 'https://news.v.daum.net/v/20190728165812603',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36'
}
resp = requests.get(url, headers=headers)
resp.json()

```




    {'post': {'id': 133493400,
      'forumId': -99,
      'postKey': '20190728165812603',
      'flags': 0,
      'title': '일론머스크 "테슬라에서 넷플릭스·유튜브 즐길 날 온다"',
      'url': 'https://news.v.daum.net/v/NHT9NtZWBe',
      'icon': 'https://img1.daumcdn.net/thumb/S1200x630/?fname=https://t1.daumcdn.net/news/201907/28/akn/20190728165813230vjsq.jpg',
      'commentCount': 42,
      'childCount': 9,
      'popularOpened': True}}



#### 로그인하여 데이터 크롤링하기
- 특정한 경우, 로그인을 해서 크롤링을 해야만 하는 경우가 존재
- 예) 쇼핑몰에서 주문한 아이템 목록, 마일리지 조회 등
- 이 경우, 로그인을 자동화 하고 로그인에 사용한 세션을 유지하여 크롤링을 진행

#### 로그인 후 데이터 크롤링 하기
1. endpoint 찾기 (개발자 도구의 network를 활용)
2. id와 password가 전달되는 form data찾기
3. session 객체 생성하여 login 진행
4. 이후 session 객체로 원하는 페이지로 이동하여 크롤링



```python
import requests
from bs4 import BeautifulSoup
```

* endpoint 찾기


```python
url = 'https://www.kangcom.com/member/member_check.asp'
```

* id, password로 구성된 form data 생성하기


```python
data = {
    'id': 'macmath22',
    'pwd': 'Test1357!'
}
```

* login
- endpoint(url)과 data를 구성하여 post 요청
- login의 경우 post로 구성하는 것이 정상적인 웹사이트!


```python
s = requests.Session()

resp = s.post(url, data=data)
print(resp.status_code)
```

    200


* crawling
- login 시 사용했던 session을 다시 사용하여 요청


```python
mypage = 'https://www.kangcom.com/mypage/'
resp = s.get(mypage)

soup = BeautifulSoup(resp.text)
mileage = soup.select_one('td.a_bbslist55:nth-child(3)')
print(mileage.get_text())
```

    5,040원 
    
