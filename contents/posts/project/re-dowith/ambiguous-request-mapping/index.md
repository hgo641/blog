---
title: "[2] @RequestMapping 매핑주소가 중복될 때 어떻게 해야할까?"
date: 2022-12-29
tags:
  - spring
series: "re-dowith"
---

## 문제 상황

아래 두 가지 API가 있다.

| method | url                       | description             |
| ------ | ------------------------- | ----------------------- |
| GET    | ~/challenge               | 모든 챌린지 조회        |
| GET    | ~/challenge?type={{type}} | 특정 타입의 챌린지 조회 |

<br/>

첫 번째 API는 모든 챌린지들을 조회하고, 두 번째 API는 쿼리 파라미터를 사용해 `type` 속성을 가지는 챌린지들을 필터링해서 던져준다. 쿼리 스트링이 있는 부분을 제외하면 두 API의 url주소는 `~/challenge`로 동일하다.

스프링 어린이인 나는 두 API를 다음과 같이 작성했다.

```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/challenge")
public class ChallengeController {
    private final ChallengeService challengeService;

	@GetMapping
    public ResponseEntity<List<ChallengeResponse>> findAll(){
        return ResponseEntity.ok(challengeService.findAll());
    }


    @GetMapping
    public ResponseEntity<List<ChallengeResponse>> findAllByChallengeType(@RequestParam(value = "type", required = true) String type){
        return ResponseEntity.ok(challengeService.findAllByChallengeType(ChallengeType.findByName(type)));
    }
}
```

실행 결과는 `IllegalStateException`로 두 API에 매핑되는 url주소가 같아 에러가 발생했다.

## 해결방법

스택오버 플로우에 올려진 글을 보고 문제를 해결할 수 있었다. [참고 링크](https://stackoverflow.com/questions/44429656/how-to-resolve-request-mapping-ambiguous-situation-in-spring-controller) <br/>

`@RequestMapping` 어노테이션에 `params`라는 인자를 넣어주면 동일한 url주소에 여러 개의 쿼리 스트링을 덧붙일 수 있다. 위 코드의 경우 `findByChallengeType`메소드의 `@GetMapping`을 `@GetMapping(params = "type")`으로 변경하면 문제가 해결된다.

### 예시

```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/challenge")
public class ChallengeController {
    private final ChallengeService challengeService;

	@GetMapping
    public ResponseEntity<List<ChallengeResponse>> findAll(){
        return ResponseEntity.ok(challengeService.findAll());
    }


    @GetMapping(params = "type") // param 인자 명시
    public ResponseEntity<List<ChallengeResponse>> findAllByChallengeType(@RequestParam(value = "type", required = true) String type){
        return ResponseEntity.ok(challengeService.findAllByChallengeType(ChallengeType.findByName(type)));
    }

    @GetMapping(params = "name") // param 인자 명시
    public ResponseEntity<List<ChallengeResponse>> findAllByName(@RequestParam(value = "name", required = true) String name){
        return ResponseEntity.ok(challengeService.findAllByName(name));
    }
}
```
