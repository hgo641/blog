---
title: "python 라이브러리 requests"
date: 2022-05-16
tags:
  - python
---



## Python 라이브러리 requests

method 4가지에 따라 api 제공

* GET : requests.get()
* POST : requests.post()
* PUT : requests.put()
* DELETE : requests.delete()



### status_code

```python
response = requests.get("http://localhost:8000")
response.status_code # 200, 404 ...
```



### content

```python
response.content
# 바이너리 원문을 가져옴
```



### text

```py
response.text
# UTF-8로 인코딩된 문자열
```



### json()

```py
response.json()
# 응답 데이터가 JSON포맷이라면 딕셔너리 객체 반환
```



### headers

```python
response.headers
# {'Date': 'Sat, 16 May 2022 21:10:49 GMT', 'Content-Type': 'application/json; charset=utf-8', ...}
# 헤더 정보를 딕셔너리 객체로 반환
```



### params

* get 방식으로 HTTP 요청을 할 때는 쿼리 스트링을 통해 응답받을 데이터를 필터링하는 경우가 많다
* params 옵션을 사용하면 쿼리 스트링을 사전의 형태로 넘길 수 있다

\# youtube data api 예제

```python
params = {
    "key": "my_secret_key",
    'chart': 'mostPopular',
    "part": ["snippet", "statistics"],
    "regionCode" : "KR",
    "maxResults": 10,
}

response = requests.get(url, params=params)
```



### url

```py
params = {
    "key": "my_secret_key",
    'chart': 'mostPopular',
    "part": ["snippet", "statistics"],
    "regionCode" : "KR",
    "maxResults": 10,
}

response = requests.get(url, params=params)
response.url
# https://www.googleapis.com/youtube/v3/videos?key={my_secret_key}&chart=mostPopular&part=snippet&part=statistics&regionCode=KR&maxResults=10
```





### data

* post나 put방식으로 HTTP 요청을 할 때는 request body에 데이터를 담아서 보낸다
* data 옵션을 사용하면 `HTML 양식(form)` 포맷의 데이터를 전송할 수 있으며 이 때 `Content-Type` 요청 헤더는 `application/x-www-form-urlencoded`로 자동 설정된다

```py
requests.post("https://localhost:8000/post", data={'title': 'title1', 'text':'hi~'})
```



### json

* json 옵션을 사용하면 REST API로 `JSON` 포맷의 데이터를 전송할 수 있으며 이때 `Content-Type` 요청 헤더는 `application/json`으로 자동 설정된다

```py
requests.post("https://localhost:8000/post", json={'title': 'title1', 'text':'hi~'})
```



### headers

* headers옵션을 사용해서 요청 헤더도 설정할 수 있다
* 인증 토큰을 보낼 때 유용하게 사용된다
