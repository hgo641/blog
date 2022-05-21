---
title: "HTTP 상태 코드"
date: 2022-04-10
tags:
  - django
  - askcompany
series: "askcompany"
---

### HTTP 상태코드

---

웹서버는 적절한 상태코드로서 응당해야한다

각 HttpResponse 클래스마다 고유한 status_code가 할당

REST API를 만들 때 유용

\# response.py

```python
from django.http import HttpResponse

def test_view(request):
    return HttpResponse(status = 201)
```

```python
class HttpResponseRedirect(HttpResponseRedirectBase): #HttpResponse를 상속받은 클래스
    status_code = 302
```

### HTTP METHOD별 다양한 상태 코드

---

#### GET

일반적으로 200(OK) 응답, 리소스를 못 찾을 경우 404(Not Found) 응답

#### POST

201(Created) 응답. 새 리소스의 URI는 응답의 로케이션 헤더에

새 리소스를 만들지 않은 경우, 200응답하고 리소스와 관련된 내용을 응답 본문에 포함. 반환할 결과가 없으면 204(내용없음)을 반환할 수도

400(잘못된 요청)

#### PUT

성공- 200(OK) 204(내용없음)

실패 - 409(충돌)

#### DELETE

성공시 204, 리소스가 없으면 404

#### 비동기작업

202(수락됨) - 요청은 수락되었지만 아직 완료되지않음

비동기 요청의 상태를 반환하는 URI를 로케이션 헤더로 변환

로케이션 헤더를 통해 비동기작업의 진행상황을 알 수 있음
