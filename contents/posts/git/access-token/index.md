---
title: "git access token 발급 잔디 테스트"
date: 2022-05-16
tags:
  - git
---

- termius에서 git clone {repo_name} 중 git access tokens 문제 발생
- 원래 id와 pw를 치고 인증하는데 pw가 아니라 access token을 입력하는 방법으로 바뀜

## Access token 발급 방법

1. github > settings

2. 메뉴의 가장 아래에 있는 `Developer settings` 클릭

3. 메뉴의 Personal access tokens 클릭해서 token 생성

4. 생성한 후 보여주는 token값을 저장해서 github인증시 pw대신 token을 입력하면 성공!

   > token값은 처음 생성할 때 한 번만 보여주므로 주의
   >
   > 물론 까먹어도 다시 발급하면됨
