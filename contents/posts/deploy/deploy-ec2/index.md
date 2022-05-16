---
title: "django 프로젝트 ec2로 배포하기"
date: 2022-05-08
tags:
  - DRF
  - AWS
  - EC2
series: "종설"
---



## EC2 인스턴스 할당하기

#### Security Group > Add Rule

1. HTTP
2. HTTPS
3. Custom -> TCP -> 8000 -> 0.0.0.0/0, ::/0



#### Review and Launch > Launch

1. Create a new key pair
2. 원하는 이름 설정
3. Download Key Pair
4. Launch Instance



#### Elastic IP 받기

Network & Security > Elastic IPs

Allocate Elastic IP address

Allocate

Associate Elastic IP address

Instance > 아까 생성된 인스턴스 선택

Associate



#### SSH 연결하기

ssh ubuntu@아까_받은_Elastic_IP -i 다운받은_PEM_FILE



## Django 프로젝트의 settings.py 수정

1. DEBUG = (os.environ.get('DEBUG', 'True') != 'False')
2. ALLOWED_HOSTS = ['*']
3. pip freeze > requirements.txt
4. git add -A, commit 해서 git repo에 push



## Termius

1. python3 -m venv venv #가상 환경 생성
2. git clone {내_GitHub_레포지토리} {django-app}
3. cd {django-app}
4. source ../venv/**bin**/activate
5. pip install -r requirements.txt
6. python manage.py runserver 0.0.0.0:8000

하면 http:// {나의 Elastic_IP:8000} 으로 접속 가능
