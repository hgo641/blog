---
title: "��� shell ��� �����Ϳ��� �� �� �ʱ⼼��"
date: 2022-05-27
tags:
  - django
---

�����Ϳ��� �Ʒ��� ���� �ۼ��ϸ� �ȴ�

```python
import os

# �� ��� ������Ʈ�� ȯ�溯�� �ε�
os.environ['DJANGO_SETTINGS_MODULE'] = 'projectname.settings'
import django
django.setup()

```

#### os.environ['DJANGO_SETTINGS_MODULE'] = 'projectname.settings'

<br/>
![](./wsgi.png)
<br/>
wsgi���Ͽ��� ������ ���۵ɶ����� �ʿ��� ȯ�溯������ �ڵ����� set���ش�
�ܺ� ��ũ��Ʈ���� ��� ������Ʈ�� settings���� ��ġ�� ����Ű�� ȯ�溯���� �����ϰ� `django.setup()`��ɾ�� ��� ȯ���� �ε��ϸ� ��� ������Ʈ�� ���� ��ҵ��� �ܺ� ��ũ��Ʈ���� ����� �� �ִ�
