---
title: "작성중 : Object Mapper"
date: 2022-10-06
tags:
  - spring
---

## Object Mapper

스프링 부트에서는 다음과 같이 object를 return해도 json으로 변환시켜주는 기능이 있다.

```java
@PostMapping("/post")
public UserRequest post(@RequestBody UserRequest userRequest){
    System.out.println(userRequest.toString());
    return userRequest;
}
```

* controller로 들어오는 request의 json(text)은 object로,
* response로 나가는 object를 json(text)으로 변환시켜주는 것이 Object Mapper이다.

<br/>

```java
@SpringBootTest
public class TestApplicationTest {

    @Test
    void contextLoads() throws JsonProcessingException {
        var ObjectMapper = new ObjectMapper();

        var user = new User("hongo","hongo@gmail.com");
        var text = ObjectMapper.writeValueAsString(user);

        var object = ObjectMapper.readValue(text, User.class);
        System.out.println(object);
    }
}
```

* ObjectMapper의 `writeValueAsString()`를 사용해 object를 json(text)로 변환시킬 수 있다.

> 이 때 ObjectMapper는 getter 메서드를 활용한다.
>
> * 멤버변수와 관련이 없는데 `get`이 들어가는 함수가 클래스내에 있을 경우 에러가 발생하므로 주의한다.
>
> ```java
> // ex) User클래스안에 StudentNumber라는 필드가 없는데 get이 들어가는 함수가 존재.
> public class User {
>     ...
>         
>     public String getStudentNumber(){
>             return "1234";
>         }
> }
> ```

<br/>

* ObjectMapper의 `readValue()`를 사용해 json(text)를 object로 변환시킬 수 있다.

> 이 때, default 생성자가 필요하다.
>
> ```java
> public class User {
>     ...
>         
>     public User(String name, String email){
>             this.name= name;
>             this.email = email;
>         }
> }
> ```
>
> * 위와 같이 생성자를 오버라이딩해 디폴트 생성자가 없는 경우 에러가 발생한다.
>
> ```java
> public class User {
> 	public User(){}; // default 생성자 필요
> 
>     public User(String name){
>         this.name = name;
>     }
> }
> ```
>
> 
