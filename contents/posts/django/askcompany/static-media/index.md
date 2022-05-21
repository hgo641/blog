---
title: "django의 Static & Media 파일"
date: 2022-04-05
tags:
  - django
series: "askcompany"
---

### 1. Static과 Media란?

---

#### - Static 파일

​ 개발 리소스로서의 정적인 파일(css, js, image등)

​ 앱/프로젝트 단위로 저장&서빙

#### - Media 파일

​ FileField/ImageField를 통해 저장한 모든 파일

​ DB필드에는 저장 경로를 저장하며 파일은 파일 스토리지에 저장

​ 프로젝트 단위로 저장&서빙

> static 파일은 배포 전 개발에 필요한 정적 파일들이고 Media 파일은 배포 후 사용자가 웹에 저장한 파일들이다 아마도

`pip install pillow` : 이미지 라이브러리

이미지 필드를 사용하려면 `pillow`를 다운로드해야함

```python
class Post(models.Model):
    image = models.ImageField()
```

### 2. Media 파일 처리 순서

---

1. HttpRequest.FILES를 통해 파일이 전달

2. 뷰 로직이나 폼 로직을 통해, 유효성 검증 수행

3. FileField/ImageField 필드에 "경로(문자열)"를 저장

4. settings.MEDIA_ROOT 경로에 파일이 저장됨

### 3. Media 파일을 위한 settings

---

##### `MEDIA_URL`

각 media 파일에 대한 URL Prefix

필드명.url 속성에 의해서 참조되는 설정

##### `MEDIA_ROOT `

파일 필드를 통한 저장시에 실제 파일을 저장할 ROOT 경로

```python
MEDIA_URL = "/media/"
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

+`BASE_DIR` ?

`seetings.py`에 선언되어있음.

`BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))`

현재 파일(settings.py)의 절대경로의 부모의 부모 : 워킹디렉토리, 장고 프로젝트의 루트를 가리킴

`MEDIA_ROOT = os.path.join(BASE_DIR, '..', 'public', 'media')`

이렇게 하면 워킹 디렉토리의 부모 dir에서 public이라는 폴더를 생성하고 그 안에 media폴더를 만들겠다는 의미

`MEDIA_ROOT` 설정을 안하면 디폴트로 워킹디렉토리 바로 아래에 이미지파일들이 쌓인다.

### 4. urls.py에 등록

---

urls.py

```python
from django.conf.urls.static import static

if settings.DEBUG: #안해도 되긴하는데 배포시에는 접근이 안되게?
    urlpatterns += static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)
```

### 5. Image 호출

---

이미지 객체에는 url이라는 속성이 있어 `img.url`을 통해 해당 이미지의 url을 가져올 수 있다.

이미지를 띄우고 싶을 때, `<img src = "{img.url}"/>`로 불러온다면

장고 보안에 의해 이미지가 안나타나고 <img ... /> 문자열 그 자체로 나타난다.

이미지를 나타내고 싶을 때는 `mark_safe()`를 사용한다.

```python
class PostAdmin(admin.ModelAdmin):
    def photo_tag(self,post):
        if post.photo:
            return mark_safe(f'<img src = "{post.photo.url}"/>')
        return None
```

### 6. upload_to

만약 서비스 크기가 커진다면 쌓이는 이미지 파일또한 많아진다.

이미지 파일이 많아질경우 파일들이 저장되는 위치를 폴더별로 나누려면 `upload_to`를 사용한다.

ex)

`photo = models.ImageField(blank = True, upload_to ='board/post/%Y/%m/%d')`

> 이미지 파일들이 작성한 year, month, day에 따라 분류된다.

문자열이 아니라 함수로도 설정할 수 있다.

- 문자열로 지정 : 파일을 저장할 중간 디렉토리 경로로서 활용

- 함수로 지정 : 중간 디렉토리 경로 및 파일명까지 결정 가능

### 6. FileField

---

**File Field**는 **File Storage API**를 통해 파일을 저장

해당 필드를 옵션 필드를 두고자 할 경우 null이 아닌 `blank = True`를 사용한다.
