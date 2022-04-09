---
title: "CBV api 사용해보기"
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

### CBV api 활용 예시(Generic display views)

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

#### 1. SingleObjectMixin 클래스의 필드

**SingleObjectMixin** : 제네릭뷰의 리스트뷰나 디테일뷰에 상속되는 클래스

`model` : 어떤 모델 객체를 사용할 것 인지. 모델 객체를 통해 해당 모델이 소속된 앱이름도 알 수 있다. 나중에 app/model_detail.html등 template를 자동으로 불러오는데 사용된다.

`queryset ` : 어떤 쿼리셋을 적용할 것인지. 리스트 해줄 모델 인스턴스들을 정할 수 있다.

​ 쿼리셋이 없다면 default로 입력된 model명을 사용해서 `model.objects.all()`을 가져온다.

​ 해당 클래스의 함수를 보면 `self.model._default_manager.all()`이라 되어있는데 `_default_manager`이 `objects`이다.

물론 쿼리셋을 바꿀수도있다. ex) `queryset = Post.objects.filter(is_public = True)`

```python
class PostDetailView(DetailView):
    model = Post

    def get_queryset(self): #DetailView에 있는 get_queryset함수를 오버라이딩한다.
        qs = super().get_queryset() #defual qs 오버라이딩
        if not self.request.user.is_authenticated:
	        qs = qs.filter(is_public=True)
        return qs
```

`pk_url_kwarg` : default값은 pk

​ url에서 `<int:id>` 형태로 받는 인자를 나타낸다. 예시와 같이 id의 경우 `pk_url_kwarg = 'id' `로 바꿔준다.

`context_object_name` : render등으로 {'post' : post} 식으로 넘겨줄때 key이름을 정함. default로 `모델명`과 `'object'` 사용 가능

#### 2. ListView

---

1개의 모델에 대한 List 템플릿 처리  
모델명소문자\_list 이름의 쿼리셋을 템플릿에 전달(object_list도 같이 전달해준다)  
페이징 처리 지원 가능

페이징 예시

```python
ListView.as_view(model=Post, paginate_by = 10)
```

필드인 `page_obj`를 통해 현재 페이지를 확인할 수 있다
`<Page 3 of 11>` 로 표기됨

django-bootstrap4를 설치해서 꾸미기가능

```html
{% load bootstrap4%} {% bootstrap_pagination page_obj size = "small"
justify_content = "center" %}
```

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

#### 3. DetailView

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

#### 4. RedirectView

---

```python
urlpatterns = [
    path('', RedirectView.as_view(url = '/instagram/'))
]
# url 주소가 서버/instagram/인 곳으로 리다이렉트해줌

urlpatterns = [
    path('', RedirectView.as_view(pattern_name='instagram:post_list'))
]

# url path를 적을 때 설정한 name을 사용해서 이동시킴 앱:name
```
