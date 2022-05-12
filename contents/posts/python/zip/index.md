---
title: "zip"
date: 2022-05-12
tags:
  - python
---

## zip

- iterable 자료형의 요소를 순서대로 묶어서 새로운 iterable 자료형을 생성한다.

  > iterable : 문자열, 리스트, 튜플과 같이 반복 가능한 자료형

- 조건 : 요소 개수가 같아야 함

- class 'zip'을 바로 print할 수 없고 dict이나 list등으로 형변환을 시켜서 출력한다

\# zip으로 list생성

```python
order ="123"
animals = ['lion','cat','dog']
fruit = ['mandarin','strawberry','apple']
zip_list = zip(order, animals, fruits)
print(list(zip_list))
```

\# zip으로 dict생성

```python
order = "123"
animals = ['lion', 'cat', 'dog']
zip_dict = zip(order, animals)
print(dict(zip_dict))
```
