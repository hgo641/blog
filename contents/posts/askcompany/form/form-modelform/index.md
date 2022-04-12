---
title: "Django의 Form과 ModelForm"
date: 2022-04-12
tags:
  - django
  - askcompany
series: "askcompany"
---

`Form`은 **HTML Form Field들을 파이썬 클래스화**

`Model`은 **Database Field들을 파이썬 클래스화** 시킨다

## Form

---

`Form` 은 `Model`과 달리 textfield가 없음

문자열은 그냥 문자열일뿐 ...

`content = forms.CharField(widget=form.Textarea)`

이런식으로 사용

## ModelForm

---

`Model`을 지정하면 해당 `Model`의 field정보를 가져와서 `Form` filed를 구성해준다.

`Model`과 연결되어있기때문에 모델 세이브도 가능

#Form

```python
from django import forms

class PostForm(forms.Form):
    title = forms.CharField()
    content = forms.CharField(widget=form.Textarea)
```

#ModelForm

```python
from django import forms
from .models import Post

class PostForm(form.ModelForm):
    class Meta:
        model = Post
        fields = '__all__'
        #fields = ['title', 'content']
```

form의 유효성 검사는 fields에 한해서 검사한다.

#### View에서 ModelForm 활용

---

```python
def post_new(request):
    if request.method == "POST":
        form = PostForm(request.POST, request.FILES)
        #파일은 받지않는다면 request.FILES 없어도 됨
        if form.is_valid():
            post = form.save() #모델폼은 바로 모델save가능
            return redirect(post)
    else:
        form = PostForm()
    return render(request, 'myapp/post_form.html', {'form' : form,})

```
