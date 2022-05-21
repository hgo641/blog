---
title: "Django REST Framework와 React 연동 첫걸음! 초기세팅을 해보자!"
date: 2022-03-26
tags:
  - DRF
  - React
series: "미오새"
---

### 1. 패키지 다운로드

```
    pip install django
    pip install djangorestframework
    pip install django-cors-headers
```

**CORS란?**
동일한 출처의 리소스에만 접근하도록 제한하는 것. 프로토콜, 호스트명, 포트번호가 모두 같아야함.
Django 서버는 127.0.0.1:8000에서 실행되고 React서버는 127.0.0.1:3000에서 실행된다.
두 서버의 포트번호가 다르기때문에 CORS 에러가 발생!

### 2. Django project의 settings.py 에 추가

```python

  ALLOWED_HOSTS = ['*']

  INSTALLED_APPS = [

    #   DRF
    'rest_framework',

    #CORS
    'corsheaders',

]

# 사이트 간 HTTP 요청을 할 수 있는 권한이 있는 출처 목록
CORS_ALLOWED_ORIGINS = [
    'http://127.0.0.1:8000',
    'http://localhost:3000',
]

# True이면 사이트 간 HTTP 요청에 쿠키를 포함할 수 있다. default값은 false.
CORS_ALLOW_CREDENTIALS = True


MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
]

TEMPLATES = [
    {
        'DIRS': [
            os.path.join(BASE_DIR, 'frontend', 'build'), #리액트 템플릿 연결
        ],
    },
]

STATICFILES_DIR = [
    os.path.join(BASE_DIR, 'frontend','build','static'),
]
```

**CORS_ALLOWED_ORIGINS**
접근 가능한 출처명을 적는 리스트 CORS_ALLOWED_ORIGIN_REGEXES 를 사용해서 정규식으로 표현할수도 있다.

```python
CORS_ALLOWED_ORIGIN_REGEXES  =  [
    r "^https://\w+\.example\.com$" ,
]
```

### 3. manage.py 수정

```python

def main():
    """Run administrative tasks."""
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')
    try:
        from django.core.management import execute_from_command_line
    except ImportError as exc:
        raise ImportError(
            "Couldn't import Django. Are you sure it's installed and "
            "available on your PYTHONPATH environment variable? Did you "
            "forget to activate a virtual environment?"
        ) from exc

    try: #try문 하나 더 추가
        if sys.argv[2] == 'react': #python [0]:manage.py [1]:runserver [2]:react
            project_root = os.getcwd()
            os.chdir(os.path.join(project_root, "frontend"))
            os.system("npm run build")
            os.chdir(project_root)
            sys.argv.pop(2)
    except IndexError:
        execute_from_command_line(sys.argv)
    else:
        execute_from_command_line(sys.argv)


if __name__ == '__main__':
    main()

```

추가한 try문으로 python manage.py runserver react로 리액트 서버까지 연결된 상태로 실행이 가능해진다.
