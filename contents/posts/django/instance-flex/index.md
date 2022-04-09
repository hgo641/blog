---
title: "장고하다가 모델 인스턴스 뻥튀기 하고싶을때"
date: 2022-04-10
tags:
  - django
---

python manage.py shell등을 사용해서  
뻥튀기할 모델의 인스턴스를 아무거나 가져온 후  
`post = Post.objects.first()`  
가져온 인스턴스의 pk를 None으로 만든다  
post.pk = None  
그리고 인스턴스를 save 한다면?  
post.save()

짜잔~ 가져온 인스턴스와 내용이 똑같지만 pk는 다른 모델이 생성됩니다~

물론 for문을 사용해서 한 번에 만들수도있음.

```python
post_list = list(Post.objects.all())
for i in range(100):
    post = random.choice(post_list)
    post.pk = None
    post.save()
```
