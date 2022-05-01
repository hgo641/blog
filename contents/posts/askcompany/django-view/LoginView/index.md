---
title: "Django LoginView 사용"
date: 2022-05-01
tags:
  - django
  - python
series: "askcompany"
---

## Django 로그인 처리

Django자체에 디폴트로

```
LOGIN_URL = '/accounts/login/'

LOGIN_REDIRECT_URL = 'accounts/profile/'
```

로 처리되어있다

장고의 `LoginView` 를 활용해서 설정

```python
form django.contrib.views import LoginView

urlpatterns=[
    path('login/', LoginView.as_view(template_name='accounts/login.html'), name = 'login'),
]
# LoginView의 template경로는 'registation/login.html'로 설정되어있다.
# template_name = 'registation/login.html'
# as_view 함수의 인자로 필드값을 넣어 인스턴스의 template_name을 변경시킬수있다.
```
