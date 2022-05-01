---
title: "AUTH_USER_MODEL 사용"
date: 2022-04-28
tags:
  - django
---

## django.contrib.auth.models

- django.contrib.auth.models를 사용해 이미 만들어진 User모델을 가져와 사용할 수 있다
- django.contrib.auth.models에서 직접 User model을 참조해오거나
- get_user_model () 을 사용해서 유저 모델을 가져오거나
- settings.py에 `AUTH_USER_MODEL`을 지정해서 유저 모델을 가져올 수 있다

> from django.contrib.auth.models import User
>
> 를 사용해 직접 User를 참조하는 방법은 이후 모델이 바뀌거나 코드를 수정해야할 일이 생길 때 유연성이 떨어진다

> get_user_model()은 객체 인스턴스를 리턴해서 Django 앱이 로드되는 그 순간에 실행되기 때문에 반드시 유효한 사용자 모델 객체를 리턴한다는 보장이 없다.
> 문자열이 아닌 유저 클래스 자체를 써야할 때 사용하도록한다.
>
> AUTH_USER_MODEL은 외래키 모델을 전달할 때 문자열로 전달한다. 외래키가 임포트될 때 모델 클래스 탐색에 실패하면 모든 앱이 로드될 때까지 실제 모델 클래스의 탐색을 미루기때문에 항상 올바른 사용자 모델을 얻을 수 있다

## AUTH_USER_MODEL 등록해보기

Django자체에 디폴트로

```
LOGIN_URL = '/accounts/login/'

LOGIN_REDIRECT_URL = 'accounts/profile/'
```

로 처리되어있다

django 공식 깃허브에 accounts앱이 있으므로

- accounts라는 이름을 가진 앱을 만든다
- accounts의 models.py에 `django.contrib.auth.models`의 AbstractUser를 상속한 User클래스를 만들어준다
- accounts라는 앱에서 User모델을 따로 만들었으므로 커스텀이 가능하다

\# accounts/models.py

```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

- config의 settings.py 에 AUTH_USER_MODEL을 등록한다

\# config/settings.py

```python
AUTH_USER_MODEL = 'accounts.User'
```
