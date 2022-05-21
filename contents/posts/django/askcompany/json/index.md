---
title: "작성중 : json 응답뷰 만들기"
date: 2022-04-07
tags:
  - django
  - DRF
series: "askcompany"
---

### JSON 응답뷰 만들기

---

```python
class PostViewSet(ModelViewSet):
    queryset = Post.objects.all() # 다룰 데이터의 범위
    serializer_class = PostSerializer
```

router를 사용해 두 개의 url생성

> urls.py

```python
from rest_framework.routers import DefaultRouter
from . import views

router = DefaultRouter()
router.register('post', views.PostViewSet) #2개의 URL을 만들어줌
#router.urls에 리스트 형태로 2개의 패턴이 생김 url pattern list

urlpatterns = [
    path('', include(router.urls)),
]
```

+HTTPie

`pip install httpie`

### JSON 직렬화

---

모든 프로그래밍 언어의 통신에서 데이터는 필히 문자열로 표현되어야함.

송신자 : 객체를 문자열로 반환하여 데이터 전송 - 직렬화

수신자: 수신한 문자열을 다시 객체로 변환하여 활용 - 비직렬화

쿼리셋타입의 객체 를 json으로 직렬화한다면?

에러 - 룰추가 필요

list/dick/set comprehension

1. 직접 변환

```python
data =[
    {'id':post.id, 'title':post.title, 'content':post.content}
    for post in Post.objects.all()
]

json.dumps(data, ensure_ascii=False)
```

번거로움

2. 직접변환 룰 지정
