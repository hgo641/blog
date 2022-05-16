---
title: "viewset url 등록"
date: 2022-05-16
tags:
  - django
  - DRF
series: "danmer"
---



## Viewset url등록

* 메소드별로 수동으로 지정
* router 사용



#### 메소드별로 수동 지정

* as_view에 인자를 넣어 설정한다

#urls.py

```python
from . import views

tutor_list = views.TutorVideoViewSet.as_view({'get' : 'list'})
# get 요청이 오면 viewset의 list함수 실행

tutor_detail = views.TutorVideoViewSet.as_view({'get':'retrieve'})
# get 요청이 오면 view set의 retrieve 함수 실행

urlpatterns = [
    path('tutor/', tutor_list, name = 'tutor_list'),
    path('tutor/<int:pk>/', tutor_detail, name = 'tutor_detail')
]
```

이처럼 메소드별로 수동으로 지정할 수 있다

> viewset에서 제공하는 기능
>
> * create
> * retrieve
> * update
> * partial_update
> * destroy
> * list



#### router 사용

```python
from rest_framework import routers

router = routers.DefaultRouter()
router.register('dancing',views.TutorVideoViewSet)
router.register('practice',views.TuteeVideoViewSet)

urlpatterns = [
    path('',include(router.urls)),
]
```

* `TutorVideoViewSet`의  url은 `~/dancing`으로 연결된다
* `TuteeVideoViewSet`의 url은 `~/practice`로 연결된다

