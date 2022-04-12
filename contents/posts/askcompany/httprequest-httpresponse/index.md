---
title: "HTTPRequest와 HttpResponse"
date: 2022-04-10
tags:
  - django
  - askcompany
series: "askcompany"
---

**HttpRequest를 받아 HttpResponse로 응답한다**

## HttpRequest 객체

---

**클라이언트로부터의 모든 요청 내용을 담고 있으며 **

함수 기반 뷰 : 매 요청시마다 뷰 함수의 첫번째 인자 `request`로 전달

클래스 기반 뷰 : 매 요청시마다 `self.request`를 통해 접근

패킷

[헤더 : get data는 헤더에만 필요한 정보가 있음. 바디 사용 x]

공백

[바디 : post 관련 data]

### Form 처리 관련 속성들

---

request. ( )

.method : 요청의 종류 "GET" 또는 "POST"

.GET : GET 인자 목록 (QueryDict 타입)

.POST : POST 인자 목록 (QueryDict 타입)

.FILES : POST 인자 중에서 파일 목록 (MultiValueDict 타입)

### MultiValueDict

---

> dict을 상속받은 클래스
>
> 동일 key의 다수 value를 지원하는 사전

http 요청에서는 하나의 key에 대해서 여러 값을 전달받을 수 있어야 함.

URL의 QueryString같은 Key로서 다수 Value지정을 지원

ex) name=TOM&name=Steve&name=Hongo

## HttpResponse

---

어떠한 뷰 함수에 응답 반환값은 무조건 HttpResponse임

(미들웨어 이미지 캡처)

## JsonResponse

---

`HttpResponse`를 상속받아서 사용 가능

직렬화 : Json 객체를 문자열로 변환

장고에는 `Django JSONEncoder`라는게 존재.

파이썬의 json.JsonEncoder를 상속받아서 DJango 만의 특징에 맞게 변환

## StreamingHttpResponse

---

효율적인 큰 응답을 위함. & 메모리를 많이 먹는 응답

but Django는 short-lived 요청에 맞게 디자인!

​ 큰 응답시에는 극심한 성능 저하로 이어질 수 있음

​ : 이럴 때는 별도의 웹 서버등 다른 서비스를 이용하는게 좋음

## FileResponse

---

`StreamingHttpResponse`를 상속받음  
파일 내용 응답에 최적화
Content-Length, Content-Type, Content-Disposion 헤더 자동 지정  
Content-Disposion : 파일 다운로드 다이로그

인자인 `as_attachment`를 True로 주면 Content-Disposion 헤더 지정
