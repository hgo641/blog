---
title: "decorator를 사용해서 Django view 꾸며보기"
date: 2022-04-10
tags:
  - django
  - python
series: "askcompany"
---

### 장식자(Decorators)

---

어떤 함수를 감싸는 (Wrapping) 함수

#### django.contrib.auth.decorators

---

`login_required ` : 로그아웃일 때 login_url로 redirect

ex 1)

```python
from django.contrib.auth.decorators import login_required

@login_required
def protected_view(request):
    return render(request, 'myapp/secret.html')
```

ex 2)

```python
from django.contrib.auth.decorators import login_required

def protected_view(request):
    return render(request, 'myapp/secret.html')

protected_view = loginrequired(protected_view)
```

`user_passes_test` : 지정 함수가 False를 반환하면 login_url로 redirect

`permission_required` : 지정 퍼미션이 없을 때 login_url로 redirect

#### django.views.decorator.http

---

`required_http_methods` : 데코레이터에 인자로 넣어준 메소드만 사용가능함

`required_GET`

`required_POST`

`require_safe` : HEAD 요청이나 GET요청 허용

### @method_decorator()

---

`@method_decorator()`를 통해 특정 메소드에만 데코레이터를 지정할 수 있다.

ex) `@method_decorator(login_required, name ='dispatch')`

해당 함수 바로 위에 쓴다면 name인자 생략 가능

근데 번거로운 방법이라 잘 사용안하는듯?

```python
class PostListView(LoginRequiredMixin, ListView):
    ...
```

이런 방법도 있음
