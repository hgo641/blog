---
title: "[c++] unique를 사용해 vector에서 중복 원소 제거하기"
date: 2022-09-15
tags:
  - cote
  - cpp
---

## vector에서 중복 원소 제거하기

[프로그래머스 - 신고 결과 받기](https://school.programmers.co.kr/learn/courses/30/lessons/92334) 문제를 푸면서 vector에서 중복 원소를 제거하는 알고리즘이 필요했다.

`vector<string> report = {"muzi neo", "frodo neo", "apeach frodo", "frodo neo"}`와 같이 문자열을 담은 벡터가 있다고 해보자. 여기서`"frodo neo"` 문자열이 중복되므로 하나를 제거하는게 내 목표이다.

```cpp
map<string, bool> m;
int idx = 0;
while(idx < report.size()) {
    bool state = m[report[idx]];
    if (state) {
        report.erase(report.begin()+idx);
        continue;
    }
    m[report[idx]] = true;
    idx++;
}
```

처음 생각한 방법은 vector의 모든 요소들을 순회하면서 카운트를 세는 방법이었다. 

* map에 문자열을 key로 사용해 저장한다. (value는 true로!)
* 순회를 돌다가 m[key]가 true이면 이미 동일한 원소가 있다는 의미이므로, 해당 원소를 삭제한다.

<br/>

### 좋은 방법일까?

너무 구리다... 중복 체크를 위해 map과 같이 여분의 컨테이너가 추가적으로 필요하다. 코드를 쓰면서도 더 좋은 방법이 있을거라고 생각하긴 했지만, 다른 방법이 떠오르지 않아 저 방식으로 풀었다. <br/>

역시 다른 분들의 정답 코드를 보니 훨씬 가독성있고 좋은 코드가 있었다. `unique`를 사용하는 방법이었는데 아래에서 사용해보도록 하자!



## unique

* vector에서 중복이 아닌 원소들을 앞에서부터 채워나가는 함수이다. 때문에 중복인 원소들은 뒤로 밀려나게 된다.
* `unique` 함수를 적용하기 전에, vector의 정렬이 필요하다.
* `unique`함수는 중복 원소의 시작부분 위치를 반환한다.

<br/>

`unique`의 반환값을 사용해서 중복이 아닌 원소들만 남겨보자!

```cpp
vector<string> report = { "muzi neo", "frodo neo", "apeach frodo", "frodo neo"};

sort(report.begin(), report.end());
report.erase(unique(report.begin(), report.end()), report.begin);

for (string s : report) {
    cout << s << "\n";
}

// 출력 결과
// apeach frodo
// frodo neo
// muzi neo
```

`unique`는 중복이 아닌 원소들은 vector의 앞 부분에, 중복인 원소들은 vector의 뒷 부분에 배치하고, 중복 원소들의 시작 위치를 반환한다. `erase`를 사용해 해당 위치부터 vector의 마지막 위치까지 삭제하면 vector안의 모든 중복 원소들을 삭제할 수 있다.

<br/>

내림차순 정렬이어도 괜찮다. 

```cpp
vector<string> report = { "muzi neo", "frodo neo", "apeach frodo", "frodo neo" };
sort(report.begin(), report.end(), greater<>());
report.erase(unique(report.begin(), report.end()), report.end());
for (string s : report) {
    cout << s << "\n";
}

// 출력 결과
// muzi neo
// frodo neo
// apeach frodo
```





