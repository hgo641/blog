---
title: "docker env설정(작성중)"
date: 2022-05-26
tags:
  - docker
---



## docker에 환경변수 설정



* danmer 서비스 배포시 aws 계정 비밀번호등이 담긴 환경변수가 필요한데 설정을 어떻게 해줘야할지 몰라서 이것 저것 찾던중



\# docker-entrypoint.sh

```bash
if [[-z "${DJANGO_USERNAME}"]];
then
    python manage.py createsuperuser \
        --noinput \
        --username $DJANGO_USERNAME \
        --email $DJANGO_EMAIL \
        --password $DJANGO_PASSWORD
fi

$@

```

* 아래와 같이 쓰면 환경변수 사용을 할 수 있다는 스택오버플로우 글을 발견함
*  -z 옵션은 환경변수 DJANGO_USERNAME의 length가 0이면 true, 아니면 false를 반환함
  * -z외에 -n등 여러가지가 있음



**그치만 이렇게 바로 해결이 될리가 없었다**

* 근데 termius에서 컨테이너 생성 후 로그보니까 -z 명령어 같은건 없다고 수행못함
* 여러 방법을 수행하던 중 김어쩌고씨에게서 아주 쉬운 방법을 배워버림(허무)

> ```bash
> docker run --env-file .env 
> ```
>
> .env파일 생성후 위 명령어도 실행해봤지만 오류남
>
> 어째서...?

/+ 오류가 난 이유는 나중에 찾아서 추가할 예정



### docker run -env

바로 docker run을 할 때 -env 옵션을 줘서 환경변수 설정을 해주면 된다...

ex)

```bash
docker run -d -e  DJANGO_USERNAME=myname -e DJANGO_PASSWORD=mypw
```

* 하나하나 설정해줘야한다는 단점이 있지만 굉장히 쉬운 방법이었다.
* 더 효율적인 방법이 분명 존재할것이므로 더 탐구해볼예정