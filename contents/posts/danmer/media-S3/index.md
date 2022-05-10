---
title: "django media파일 S3서버랑 연동하기"
date: 2022-05-08
tags:
  - DRF
  - AWS
  - S3
series: "종설"
---



## S3 버킷 생성





## Django setting 설정

#### 패키지 & 라이브러리 설치

```python
pip install boto3 # python에서 AWS에 접근가능한 패키지
pip install django-storages # 커스텀 스토리지 
```



#### settings.py - INSTALLED_APP



```python
# media url 설정
MEDIA_URL = "/media/"
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

# for AWS S3
DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
AWS_S3_SECURE_URLS = False # use http instead of https
AWS_QUERYSTRING_AUTH = False # don't add complex authentication-related query parameters for requests

load_dotenv()

# AWS IAM 유저 key설정
AWS_S3_ACCESS_KEY_ID = os.getenv('ACCESS_KEY')
AWS_S3_SECRET_ACCESS_KEY =os.getenv('SECRET_ACCESS_KEY')

# S3 버킷 이름
AWS_STORAGE_BUCKET_NAME = 'danmer-videos'
```

