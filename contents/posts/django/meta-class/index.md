---
title: "Django의 Meta 클래스 활용"
date: 2022-04-03
tags:
  - django
---

## Meta 클래스란?

모델 내부에 정의되는 클래스로 Django모델 자체에 대한 정보, 속성을 추가하는 클래스이다.

```python
from django.db import models

#모델 내부에 정의
class MyModel(models.Model):
    ...
    class Meta:
		...
```

Meta 클래스는 권한, 데이터베이스의 이름, 단 복수 이름, 추상화, 순서 지정등과 같은 모델에 대한 다양한 사항을 정의하는데 사용할 수 있다.

다음은 Meta 클래스 활용 예시이다.

### 1. abstract

모델이 추상적인지를 결정한다. 추상 클래스는 인스턴스화 할 수 없고 확장 또는 상속만 가능하다.

```python
 class Meta:
        abstract = True
```

### 2. db_table

데이터베이스 테이블 이름을 설정한다.

```python
class Meta:
    db_table = "comment"
```

기본적으로 Django에서 db table의 이름은 앱이름\_모델이름으로 정의된다.

ex) _board앱의 Post모델의 경우: board_post로 정의됨_

MySQL등 장고 외부 데이터베이스와 연동할 때 사용하는듯?

MySQL 백엔드의 경우 테이블 이름은 소문자로 지정해야하고 Oracle은 테이블 이름에 30자 제한이 있음. 이럴 때 사용하는듯

### 3. ordering

모델 필드명을 기준으로 모델 객체의 순서를 정의한다.

```python
from django.db import models

class JobPosting(models.Model):
    dateTimeOfPosting = models.DateTimeField(auto_now_add = True)


    class Meta:
        ordering = ["-dateTimeOfPosting"]
```

### 4. verbose_name

Django 모델을 admin페이지에서 조회할 때 모델 표기 방법을 변경한다.

```py
class Meta:
    verbose_name = "포스트"
```
