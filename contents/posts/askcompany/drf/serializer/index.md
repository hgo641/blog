---
title: "REST"
date: 2022-04-18
tags:
  - django
  - DRF
series: "askcompany"
---

# APIView를 활용한 뷰 만들기

## Serializer 정의

```python
class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post
        fields = '__all__'

serializer = PostSerializer(data=request.POST)

if serializer.is_valid():
    return JsonResponse(serializer.data, status=201)
#form의 __clean_data처럼 serializer.data를 통해 유효성 검사를 거친 데이터를 가져올 수 있다.
#이건 장고 restframework의 기능을 안쓰고 form의 기능만 쓴 것
#drf를 적용하려면 APIView 클래스 혹은 @api_view 장식자를 사용한다
return JsonResponse(serializer.errors, status=400)
```

- 첫 번째 인자로 인스턴스를 받음

- 두 번째 인자로 데이터를 받음

> Form 에서 첫 번째 인자는 data지만 시리얼라이저의 생성자의 첫 번째 인자는 instance이다

```python
PostSerializer(Post.objects.first()) # 인스턴스 객체를 넘겨준다
PostSerializer(Post.objects.all(), many = True) # 다수도 가능
PostSerializer(Post.objects.first()).data # 직렬화된 데이터가 나온다
```

## APIView
