---
title: "docker 개념"
date: 2022-05-17
tags:
  - docker
---





## docker란

>  어떤 서비스를 개발할 때 필요한 패키지, 라이브러리들을 다른 서버에서 재설치를 하지않고 충돌없이 사용할 수 있게 해주는 서비스



### docker image

> 리눅스 환경에서 사용가능한 패키지, 라이브러리들을 컴퓨터로 다운받을 수 있게 저장해둔 것



### docker component

> docker image를 가지고 컴퓨터에 생성한 독립적인 작업 공간

`docker image`를 가지고 여러 개의 `component`를 생성할 수 있다



### docker volume

> 컨테이너와 내 컴퓨터의 특정 폴더를 공유하는걸 의미



### docker compose

> 각각의 component들을 하나로 묶음



### 간단한 docker 실습

#### docker 기본 명령어

* 도커 버전 확인

```bash
docker -v
```



* 도커 이미지 다운만 받기

```bash
docker pull {img_name}:{tag}
# 태그는 필수 요소가 아님
```



* 컴퓨터 내 도커 이미지들 보기

```bash
docker images
```



* 이미지로 컨테이너 생성하기

```bash
docker create {option} {img_name}:{tag}
# ex) docker crate -it python
```



* 만들어진 컨테이너 시작하기 (이미지에 CMD로 지정해놓은 작업 시키기)

```bash
docker start {container_id or name}
```



* 컨테이너로 들어가기 (컨테이너 내 CLI 이용하기)

```bash
docker attach {container_id or name}
```



* (해당 이미지가 없다면) 이미지를 다운받아 바로 컨테이너 실행하여 진입하기

```bash
docker run {img_name}:{tag}
# ex) docker -it run python:3
# pull, create, start, attach를 한 번에 실행하는 것과 같다
```



| 옵션                                  | 설명                                                         |
| :------------------------------------ | ------------------------------------------------------------ |
| -d                                    | 데몬으로 실행(백그라운드에서 알아서 돌라고 하기)             |
| -it                                   | 컨테이너로 들어갔을 때 bash로 CLI 입출력을 사용할 수 있도록 해줌 |
| --name {container_name}               | 컨테이너 이름 지정                                           |
| -p {port of host}:{port of container} | 호스트와 컨테이너의 포트를 연결                              |
| --rm                                  | 컨테이너가 종료되면 컨테이너 제거                            |
| -v {dir of host}:{dir of container}   | 호스트와 컨테이너의 디렉토리를 연결                          |



* 동작중인 컨테이너 재시작

```bash
docker restart {container_id or name}
```



* 도커 컨테이너 내부 쉘에서 빠져나오기 (컨테이너를 종료)

```bash
exit
# 또는  ctrl + d
```



* 도커 컨테이너 내부 쉘에서 빠져나오기 (컨테이너를 종료하지 않음)

  * ctrl + P + Q

    

* 동작중인 컨테이너 목록보기

```bash
docker ps
# 동작하지않는 컨테이너까지 모두 보려면 docker ps -a
```



* 컨테이너 삭제

```bash
docker rm {container_id or name}

# 모든 컨테이너 삭제
# docker rm `docker ps -a -q`
```



* 이미지 삭제

```bash
docker rm {option} {img_id}
# 컨테이너가 있을시 강제 삭제 : 옵션 -f 사용
```



* 모든 컨테이너와 이미지 등 도커 요소 중지 및 삭제

```bash
# 모든 컨테이너 중지
docker stop $(docker ps -aq)

# 사용되지 않는 모든 도커 요소(컨테이너, 이미지, 네트워크, 볼륨 등) 삭제
docker system prune -a
```



* 도커 파일로 이미지 생성

```bash
# Dockerfile 파일이 있는 디렉토리 기준.  마지막의 . 이 상대주소
docker build -t {이미지명} .
```



* 도커 컴포즈 실행

```bash
# docker-compose 파일이 있는 디렉토리 기준
docker-compose up
# 백그라운드에서 데몬으로 돌게 하려면 옵션 -d를 붙임
```



#### Dockerfile



* 

