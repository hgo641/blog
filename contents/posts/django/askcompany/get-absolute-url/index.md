---
title: "detail url을 간단하게 하고싶다면, get_absolute_url"
date: 2022-04-10
tags:
  - django
  - askcompany
series: "askcompany"
---

#### get_absolute_url

---

{% raw %}
`{% url 'blog:post_detail' post.pk %}` 식으로 사용하던 것을

`redirect(post)` , `{{ post.get_absolute_url }}`와 같이 사용 가능

모델 클래스에 `get_absolute_url()`을 구현한다.
{% endraw %}

```python
from django.urls import reverse

class Post(models.Model):
    ...
    def get_absolute_url(self):
        return reverse('blog:post_detail', args=[self.pk])
```

장고 템플릿 태그 주석 : {# #}
