---
title: "장고 폼의 유효성 검사 : clean과 validators"
date: 2022-04-12
tags:
  - django
  - askcompany
series: "askcompany"
---

### 필드 별로 유효성 검사 함수 추가 적용

---

```python
from django import forms

def min_length_3_validator(value):
    if len(value) < 3:
        raise form.ValidationError("3글자 이상 입력해주세요")

class PostForm(forms.Form):
    title = forms.CharField(validators=[min_length_3_validators])
```

_MinLengthValidator가 이미 존재하긴함_

```python
from django.core.validators import MinLengthValidator

min_length_3_validator = MinLengthValidator(3)
```

#### 모델단에도 설정 가능~

---

**models.py에서 설정해 ModelForm에서 사용할수도있다**

#models.py

```python
from django import models

def min_length_3_validator(value):
    if len(value) < 3:
        raise form.ValidationError("3글자 이상 입력해주세요")

class Post(forms.Model):
    title = models.CharField(max_length=100, validators=[min_length_3_validator])
    content = models.TextField()
```

#forms.py

```python
from django import forms
from .models import Post

class PostForm(form.ModelForm):
    class Meta:
        model = Post
        fields = '__all__'
        #fields = ['title', 'content']
```

모델단에 지정한 validator를 ModelForm이 알아서 가져감

### 유효성 검사 호출 로직

---

**form.is_valid() 가 호출되었을 때**

#### 1. form.full_clean() 호출

​ **1. 각 필드 객체 별로 수행**

​ 각 필드객체.clean() 호출을 통해 각 필드 Type에 맞춰 유효성 검사

​ ex) email field가 email 형식인지

​ `clean()`은 `is_valid()`가 호출되면 내부적으로 수행된다.

​ **2. Form 객체 내에서**

​ 필드 이름 별로 Formr객체.clean\_필드명() 함수가 있다면 호출해서 유효성 검사

​ Form객체.clean() 함수가 있다면 호출해서 유횽성 검사

​ # ex)

```python
import re

class PostForm(form.ModelForm):
    class Meta:
        model = Post
        field = ['message', 'photo', 'tag_set']

    def clean_message(self):
        message = self.cleaned_data('message')
        if message:
            message = re.sub(r'[a-zA-z]+','',message)
            #영어 제거
        return message
```

drf 시리얼라이저에서는 clean대신 validator

### Form에서 수행하는 2가지 유효성 검사

---

**1. Validator 함수를 통한 유효성 검사**

값이 원하는 조건에 맞지 않을 때 ValidationError 예외를 발생

- 리턴 값은 사용되지 않는다
- 인자 하나를 받는다

**2. Form 클래스 내 clean, clean\_멤버함수를 통한 유효성 검사 및 값 변경 **

값이 원하는 조건에 맞지 않을 때 ValidationError 예외를 발생

- 리턴값을 통해 값 반환

### 함수형/클래스형 Validator

---

#### 함수형

​ 유효성 검사를 수행할 값 인자를 1개 받은 Callable Object

#### 클래스형

​ 클래스의 인스턴스가 Callable Object : \_\_call\_\_함수 존재

#### but 빌트인 Validator를 잘 사용하자!

### 언제 validators를 쓰고, 언제 clean을 사용할까?

---

**가급적이면 모든 validators는 모델에 정의하고 ModelForm을 통해 모델의 validators 정보를 가져오기!**

**clean이 필요할 때는**

특정 Form에서 1회성 유효성 검사 루틴이 필요할 때

다수 필드값에 걸쳐서, 유효성 검사가 필요할 때

필드 값을 변경할 필요가 있을 때 (validator는 값만 체크할 뿐, 값을 변경할 수는 없음)
