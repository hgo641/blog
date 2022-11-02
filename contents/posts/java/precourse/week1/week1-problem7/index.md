---
title: "우테코5기 프리코스 1주차 Problem7"
date: 2022-11-02
tags:
  - java
series: "wooteco-precourse"
---

## Problem7 : 친구 추천 서비스

## 📗 기능 목록

```
* 추천 친구 목록 정렬 기능
* 모든 유저들의 친구 관계 저장 기능
* 타임 라인을 방문한 유저의 추천 포인트 증가 기능
* 함께 아는 친구가 있는 유저의 추천 포인트 증가 기능
* 최대 5명의 추천 친구 리스트 반환 기능
```



## 📑 클래스별 상세 기능 목록

```
📌 class `RecommendedFriendComparator`

추천 친구 목록을 정렬하기 위한 클래스

`int compare (String leftUser, String rightUser)`

  * 추천 친구 목록 정렬 수행


📌 class `UserManager`

모든 유저들의 친구 관계를 관리하는 클래스

`void addFriend(String sourceUser, String destUser)`
  * `friendsMap`에 유저의 친구 관계를 저장. `sourceUser`의 친구 리스트에 `destUser`를 추가.

*`Map<String, List<String>> getFriendsMap(List<List<String>> friends)`
  * 모든 유저들의 친구 관계를 저장하는 `friendMap` 반환

  
📌 class `User`

사용자의 이름, 타임 라인 방문자 리스트, 친구 리스트, 추천 친구 리스트를 클래스

`void setRecommendPoint(List<String> candidates, int extraPoint)`
  * 추천 친구 후보인 candidate가 user 본인이 아니고, 이미 등록된 친구가 아닐경우에만 `extraPoint`만큼 추천 점수를 올림
  
`void addVisitPoint`
  * 타임라인을 방문한 유저의 추천 포인트 추가
  
`void addFriendPoint`
  * 함께 아는 친구가 있는 유저의 추천 포인트 추가
  
`List<String> getSortedKeySet()`
  * 정렬된 추천 친구 리스트의 key 리스트 반환
  
`List<String> getTopFiveRecommendedFriend`
  * 최대 5명의 추천 친구 리스트 반환
```





## 📌 코드

[전체 코드가 있는 깃허브 링크](https://github.com/hgo641/java-onboarding/blob/hgo641/src/main/java/onboarding/Problem7.java)

```java
class UserManager{
 	static final int VISIT_POINT = 1;
    static final int FRIEND_POINT = 10;   
    private static Map<String, List<String>> friendsMap = Collections.EMPTY_MAP;
    
    public Map<String, List<String>> getFriendsMap(List<List<String>> friends){
        for(List<String> friend : friends){
            String firstUser = friend.get(0);
            String secondUser = friend.get(1);

            addFriend(firstUser, secondUser);
            addFriend(secondUser, firstUser);
        }
        return friendsMap;
    }
}
```

* UserManager로 모든 user들에 대한 친구 관계 해시맵을 생성한다.

<br/>



```java
class User{
	private List<String> visitors;
    private List<String> friends;
    private Map<String, Integer> recommendedFriend = new HashMap<String, Integer>();
    
     void addVisitPoint(){
        setRecommendPoint(visitors, UserManager.VISIT_POINT);
    }

   
    void addFriendPoint(Map<String, List<String>> friendsMap){
        for(String friend : this.friends){
            List<String> candidates = friendsMap.get(friend);
            setRecommendPoint(candidates, UserManager.FRIEND_POINT);
        }
    }
}
```

* 추천 친구의 추천 점수를 올리는 함수를 만든다.

<br/>

```java
private void setRecommendPoint(List<String> candidates, int extraPoint){
        if(candidates.isEmpty()){
            return;
        }
        for(String candidate : candidates){
            if(candidate == this.userName){
                continue;
            }
            if(!this.friends.isEmpty() && this.friends.contains(candidate)){
                continue;
            }
            Integer recommendPoint = this.recommendedFriend.getOrDefault(candidate, 0);
            this.recommendedFriend.put(candidate, recommendPoint + extraPoint);
        }
    }
```

* 추천 점수를 적용하는 `setRecommendPoint` 함수이다.
* 추천 점수를 set하려는 대상이 user본인이거나, 이미 친구라면 점수를 올리지 않는다.

<br/>

```java
List<String> getSortedKeySet(){
        List<String> keySet = new ArrayList<>(this.recommendedFriend.keySet());
        RecommendedFriendComparator recommendedFriendComparator = new RecommendedFriendComparator(this.recommendedFriend);
        keySet.sort(recommendedFriendComparator);
        return keySet;
    }

    /** 최대 5명의 추천 친구 리스트 반환 */
    List<String> getTopFiveRecommendedFriend(){
        List<String> topRecommendedFriend = new ArrayList<String>();
        List<String> SortedKeySet = getSortedKeySet();
        int maxIndex = min(5, SortedKeySet.size());
        for(int i =0; i<maxIndex; i++){
            topRecommendedFriend.add(SortedKeySet.get(i));
        }
        return topRecommendedFriend;
    }
```

* 이후 문제에서 요구하는 대로 `getSortedKeySet `함수를 사용해 추천 친구를 정렬한다.
* `getTopFiveRecommendedFriend`함수를 사용해 최대 5명의 추천친구리스트를 반환한다.

<br/>



### ✏ 다른 분의 풀이

[mihye126](https://potatosprout.tistory.com/13)님의 허락을 받고 슬랙에 공유해주신 풀이를 가져왔다.

```java
//userScore filtering
List<Map.Entry<String, Integer>> entries = userScore.entrySet().stream()
    .filter(h -> !h.getKey().equals(user) & !friendsGraph.get(user).contains(h.getKey())) //user와 user 친구들 제외
    .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder())) // 상위 점수대로 정렬
    .collect(Collectors.toList());

return entries.subList(0, Math.min(entries.size(), 5)).stream().map(Map.Entry::getKey) //상위 5개 필터링
    .collect(Collectors.toList());
```

* `entrySet`을 사용해 `userScore` map 전체를 순회하며, 바로 filter, sorted를 적용하는 코드였다.
* stream을 활용하니 코드가 굉장히 줄어드는 것을 볼 수 있었다...!



