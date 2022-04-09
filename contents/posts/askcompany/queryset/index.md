---
title: "작성중 : 쓸만해보이는 쿼리셋팁"
date: 2022-04-10
tags:
  - django
  - askcompany
series: "askcompany"
---

### 1. values_list

필드값을 인자로 넣어 생성된 모델 인스턴스들의 해당 필드값을 나열한다.

`Post.objects.all().values_list('created_at__year')`

`Post.objects.all().values_list('created_at__year', flat = True)`

원래 튜플 형태로 출력되는데 **flat = True** 를 사용해서 튜플을 없앨 수 있다.

####

### 2. \_

이전 출력 결과를 다음 줄에서 \_를 사용해 표현할 수 있다.
