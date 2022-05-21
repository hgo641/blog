---
title: "django ORM"
date: 2022-04-05
tags:
  - django
series: "askcompany"
---

## Django admin

---

#### admin 등록 방법 (1)

---

```python
admin.site.register(모델명)
```

기본 ModelAdmin으로 동작

#### admin 등록 방법 (2)

---

```py
class ModelAdmin(admin.ModelAdmin):
    pass

admin.site.register(모델명, ModelAdmin)
```

지정한 ModelAdmin으로 동작

#### admin 등록 방법 (3)

---

```py
@admin.register(모델명)

class ModelAdmin(admin.ModelAdmin):
    pass
```

**python wrapping**

파이썬의 장식자 문법

감싼 대상의 기능을 변경할 수 있다.

### admin에서 표기되는 모델 객체명 바꾸기

---

**admin**에서 모델 객체의 이름은 **모델명 object (n)**의 형식으로 나온다.

ex) Post object (1)

이 이름을 바꾸고 싶다면 model선언에서` def __str__(Self):`를 사용한다.

##### `def __str__(Self):`

어떤 객체에 대한 문자열 표현을 할 수 있다.

ex)

```py
class Post(models.Model):
    message = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return f"({self.id})번째 포스트"
    	#return "({})번째 포스트".format(self.id)
        #return self.message
```

### list_display 속성 정의

---

admin 테이블에서` list_display`에서 적은 속성명을 표기할 수 있다.

```python
from django.contrib import admin

@admin.register(모델명)
class ModelAdmin(admin.ModelAdmin):
  list_display = ['id', 'message']
```

#### 1) 모델 필드 외 속성 추가하기

꼭 모델 필드가 아니더라도 list_display에 추가할 수 있다.

> Post의 메세지 길이도 list_play속성에 넣고싶다면?

```python
class Post(models.Model):
    message = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return f"({self.id})번째 포스트"
    	#return "({})번째 포스트".format(self.id)
    	#return self.message

    def message_length(self):
    	return len(self.message)
```

```python
from django.contrib import admin

@admin.register(모델명)
class ModelAdmin(admin.ModelAdmin):
  list_display = ['id', 'message','message_length']
```

만약 `message_length`가 자주 쓰이는 로직이라면 모델단에 정의하는게 좋으나 어드민에서만 사용한다면 어드민에서 정의해도 상관없다.

```python
from django.contrib import admin

@admin.register(모델명)
class ModelAdmin(admin.ModelAdmin):
  list_display = ['id', 'message','message_length']

  def message_length(self, post): #post객체는 admin이 알아서 넘겨줌
        return f"{len(post.message)} 글자"
```

#### 2) list_display_link

해당 list_display 필드를 누르면 model 객체 상세로 이동한다.

```python
from django.contrib import admin

@admin.register(모델명)
class ModelAdmin(admin.ModelAdmin):
  list_display = ['id', 'message','message_length']
  list_display_links = ['message']
```

#### 3) search_fields

admin 테이블에서 검색 가능

```python
from django.contrib import admin

@admin.register(모델명)
class ModelAdmin(admin.ModelAdmin):
  list_display = ['id', 'message','message_length']
  list_display_links = ['message']
  search_fields = ['message']
```

#### 4) list_filter

admin 메인에서 필터 기능 제공

```python
from django.contrib import admin

@admin.register(모델명)
class ModelAdmin(admin.ModelAdmin):
  list_display = ['id', 'message','message_length']
  list_display_links = ['message']
  search_fields = ['message']
  list_filter - ['created_at']
```
