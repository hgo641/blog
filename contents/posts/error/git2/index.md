---
title: "git error: cannot lock ref 'refs/remotes/origin/main': unable to resolve reference 'refs/remotes/origin/main': reference broken 문제 해결"
date: 2022-06-28
tags:
  - ERROR
---

### 에러내용

git 명령어를 쳤는데 `error: cannot lock ref 'refs/remotes/origin/main': unable to resolve reference 'refs/remotes/origin/main': reference broken `에러가 나옴

> git repository에서 뭔가 손상된 것 같다...
>
> 자세한 이유는 모르겠다 왤까?



### 해결방법 1

```
   rm .git/refs/remotes/origin/main
   git fetch
```

1. 문제가 발생한 `.git/refs/remotes/origin/main` 파일을 지운다.

2. `git fetch`를 한다

이후 다시 git 명령어를 치면 에러없이 잘 동작한다!





### 해결방법 2

시도해보진않았지만 로컬 저장소를 정리하면 해결될수도 있다고 한다...<br/>

나중에 똑같은 에러가 발생하면 한 번 해봐야겠다 ㅎㅎ

```
git gc --prune=now
gir remote prune origin
```

