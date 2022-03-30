---
title: "git pull 오류 : fatal: refusing to merge unrelated histories"
date: 2021-03-27
tags:
  - ERROR
---

### 에러내용

git push 전 pull 했는데 **fatal: refusing to merge unrelated histories** 오류가 날 때

> 깃허브 레파지토리를 먼저 만들고 로컬에서 프로젝트 생성 후 푸쉬하려고 할 때 두 프로젝트를 다른 것으로 인식해서 나는 에러

### 해결방법

```
    git pull origin [브런치] --allow-unrelated-histories
```

--allow-unrelated-histories

독립적인 두 프로젝트를 병합하는 명령어
