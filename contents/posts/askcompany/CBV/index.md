---
title: "클래스 기반 View"
date: 2022-04-03
tags:
  - computer protection
---

view는 무조건 HttpResponse객체를 반환해야한다.

### CBV

---

**Class Based View** : View 함수를 만들어주는 클래스

- `as_view()` 클래스 함수를 통해, View 함수를 생성

- 상속을 통해 여러 기능들을 믹스인

#### CBV API

---

- **Base views**

  `View`, `TemplateView`, `RedirectView`

- **Generic display views**

​ `DetailView`, `ListView`,

- **Generic date views**

- **Generic editing views**

  `FormView`, `CreateView`, `UpdateView`, `DeleteView`

### CBV 활용 예시(Generic display views)

---

```python
class DetailView:
    def __init__(self, model):
        self.model = model

    def get_object(self, *args, **kwargs):
        return get_object_or_404(self.model, id = kwargs['id']) #get_object_or_404를 통해 detail을 보고자하는 모델 인스턴스를 찾아 반환한다.

    def get_template_name(self):
        return '{}/{}_detail.html'.format(
        	self.model._meta.app_label, #앱이름
            self.model._meta.model_name #모델명
        )
        #ex) instagram앱의 post모델을 가져올경우instagram/post_detail.html 을 반환해준다

    def dispatch(self, request, *args, **kwargs):
        object = self.get_object(*args, **kwargs)
        return render(request, self.get_template_name(),{
            self.model._meta.model_name: object,
        })# post: 찾고자하는 post객체

    @classmethod
    def as_view(cls, model):#cls 는 알아서 들어옴
        def view(request, *args, **kwargs):
            self = cls(model)
            return self.dispatch(request, *args, **kwargs)
        return view
```

#### 1. ListView

---

_view.py_

```python
from django.view.generic import ListView
from shop.models import Item

item_list = ListView.as_view(model=Item)
```

_urls.py_

```python
from django.urls import path

urlpatterns = [
    path('items/', item_list, name='item_list'),
]
```

단순 리스트 기능만 제공. 검색 기능등 다른 기능은 직접 추가해야함

#### 2. Detail view

---

views.py

```py
from django.views.generic import DetailView

post_detail = DetailView.as_view(model = Post, pk_url_kwarg = 'id')

#pk_url_kwarg는 urls.py에서 적은 url에 들어갈 인자이다.
#만약 urls.py에 path('post/<int:pk>/',post_detail)
#이라고 적었다면 pk_url_kwarg는 생략 가능하다
#post_detail = DetailView.as_view(model = Post)
```

urls.py

```python
urlpatterns = [
    path('post/<int:id>/', post_detail)
]

```

상속을 통한 CBV 속성정리

```python
class PostDetailView(DetailView):
    model = Post
    pk_url_kwarg = 'id'

post_detail = PostDetailView.as_view()
```
