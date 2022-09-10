---
title: "[c++] multiset 활용"
date: 2022-09-11
tags:
  - cote
  - cpp
---

## multiset

원소를 중복으로 추가할 수 있는 set 컨테이너이다. <br/>

- set과 마찬가지로 디폴트로 오름차순 정렬이 수행된다.
- 물론, 생성자에 인자로 비교 함수를 넣어 원하는 대로 정렬을 시킬 수도 있다.
  - set<int, greater<int>> s



## multiset 에서 원소 삭제하기

`set`과 마찬가지로 `erase()` 함수를 사용하면 된다.<br/>

그런데`multiset`은 `set`과 달리 원소를 중복되게 넣을 수 있었다. `multiset`에서 값이 똑같은 원소를 erase()하면 어떻게 될까? 중복인 원소들이 전부 삭제될까, 하나만 삭제될까? (이 주제가 오늘 포스팅을 쓰게 된 이유이다...)

< br/>

### 중복인 수를 삭제한다면?

정답은... 하나만 임의로 삭제될 줄 알았는데 중복인 수 모두가 사라진다.<br/>

* 아래는 multiset에서 최대값을 지우는 코드이다.

> * set은 자동으로 오름차순 정렬이 되므로, 가장 마지막에 있는 원소가 최대값이다.
> * `rbegin()` 함수로 set의 가장 마지막 원소의 위치를 알 수 있다.

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    multiset<int> set = {1,2,3,4,5,5};
	
	auto iter = set.rbegin();
	set.erase(*iter);

	for (auto s : set) {
		cout << s << " ";
	}
    // 출력 결과
    // 1 2 3 4
}
```

<br/>

그럼 중복인 수들 중 하나만 삭제하고 싶다면 어떻게 해야할까?<br/>

* `find()`로 지우고 싶은 수를 찾는다.
* `find()`로 찾은 `iterator`를 가지고 `erase()`에서 해당 위치의 원소를 삭제한다.

위 방법을 수행하면 중복인 수들 중 하나만 지울 수 있다.

<br/>

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    multiset<int> set = {1,2,3,4,5,5};
	
	auto iter = set.rbegin();
	auto item = set.find(*iter);
	set.erase(item);

	for (auto s : set) {
		cout << s << " ";
	}
    // 출력 결과
    // 1 2 3 4 5
}
```

* `find()`를 사용해서 원소를 가리키는 반복자를 얻은 뒤, 이를 `erase()`하면 가장 마지막의 원소 하나만 삭제할 수 있다.



