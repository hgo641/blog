---
title: "CBV api 사용해보기2 : crud"
date: 2022-04-13
tags:
  - django
  - python
series: "askcompany"
---

## CBV api 사용해보기2 : crud

---

#### CreateView

```python
class PostCreateView(CreateView):
    model = Post
    form_class = PostForm

    def form_valid(self, form):
        self.object = form.save(commit = False)
        self.object.author = self.request.user
        messages.success(self.request, "포스트 저장 완료")
        return super().form_valid(form) #CreateView함수의 form_valid()

post_new = PostCreateView.as_view()
```

#### DeleteView

```python
class PostDeleteView(LoginRequiredMixin, DeleteView):
    model = POST

    # 성공하면 이동할 url
    #success_url = reverse('instagram:post_list')
    # success_url은 프로젝트 로딩전 호출, 그러나 instagram:post_list가 적힌 프로젝트.urls는 프로젝트 로딩이 되면서 읽어오기때문에 오류가 생긴다.

    # 그렇다면 어떻게 success_url을 설정할까?
    # 방법1) 함수로 구현
    def get_success_url(self):
        return reverse("instagram:post_list")
    # 함수는 실제로 success_url이 필요할 때 실행된다.

post_delete = PostDeleteView.as_view()
```

```python
class PostDeleteView(LoginRequiredMixin, DeleteView):
    model = POST

    # 방법1) reverse_lazy 사용
	success_url = reverse_lazy('instagram:post_list')
    # reverse_lazy는 실제 success_url이 필요할 때 사용한다.
post_delete = PostDeleteView.as_view()
```
