---
title: "c++로 vector 만들기"
date: 2022-06-24
tags:
  - cpp
---

## vector?

동적으로 요소를 할당할 수 있다.

> 동적할당이란?
>
> 실행 시간 동안 사용할 메모리 공간을 할당하는 것을 의미한다.
>
> 쉽게 말해서 정적 배열인 array의 경우 미리 얼만큼 사용할 것인지 크기를 결정하고 사용한다.
>
> 그러나 동적으로 할당한다면 상황에 따라 원하는 크기만큼의 메모리가 할당되고 이미 할당된 메모리라도 언제든지 크기 조정이 가능하다.
>
> **즉, 어떤 배열이 필요한데 그 배열의 크기를 정확히 알 수 없을 때 사용할 수 있다.**
>
> 그렇다면 동적 할당이 정적 할당보다 더 좋은게 아닌가?
>
> - 꼭 그렇다고 할 수 만은 없다!
> - 동적할당은 메모리 누수가 일어날 수 있기 때문에 사용하지 않을 땐 메모리를 해제해줘야한다.

## vector 인터페이스

| 기능                       | 형식                      | 예시                                               |
| -------------------------- | ------------------------- | -------------------------------------------------- |
| 생성                       | vector<자료형> name       | vector<int> v                                      |
| n만큼 할당 후 0으로 초기화 | vector<자료형> name(n)    | vector<int> v(5)                                   |
| 값 미리 초기화             | vector<자료형> name = {}  | vector<int> v = {1, 2, 3}                          |
| 벡터 시작점 주소값         | v.begin()                 |                                                    |
| 벡터 끝부분+1 주소값       | v.end()                   |                                                    |
| i번째 요소 접근            | v.at(i)                   | v.at(0)                                            |
| 첫 번째 요소 접근          | v.front()                 |                                                    |
| 마지막 요소 접근           | v.back()                  |                                                    |
| 맨 뒤에 요소 추가          | v.push_back(값)           | v.push_back(2)                                     |
| 맨 뒤에 요소 제거          | v.pop_back()              |                                                    |
| 요소 삽입                  | v.insert(위치, 값)        | v.insert(v.begin()+1, 100)                         |
| 요소 삭제                  | v.erase(시작위치, 끝위치) | v.erase(v.begin), v.erase(v.begin, v.begin+5)      |
| 모든 요소 삭제             | v.clear()                 |                                                    |
| 벡터 사이즈 조정           | v.resize(사이즈)          | v.resize(10) : 사이즈가 커지면 0으로 값이 추가된다 |
| 벡터 swap                  | v.swap(벡터)              | v1.swap(v2)                                        |
| 벡터가 비었는지 확인       | v.empty()                 |                                                    |
| 벡터 사이즈 반환           | v.size()                  |                                                    |

## vector 사용 예시

```cpp
#include <bits/stdc++.h>
using namespace std;

// vector선언 : 꺽쇠안에 자료형을 명시
vector<int> v;
int main(){
    // vector 값 할당 - push_back
    // : vector의 맨 뒤에 요소를 추가한다.
    v.push_back(0);
    v.push_back(1);
    for(int i = 1; i <= 10; i++)v.push_back(i);

    // vector 값 삭제1 - pop_back
    // : vector의 맨 뒤에 값을 삭제한다.
    v.pop_back();

    // vector 값 삭제2 - erase
    v.erase(v.begin(), v.begin()+2);

    // vector 값 출력
    for (int a:v) cout << a << " ";
    cout << "\n";

    // vector 값 탐색 - find
    auto a = find(v.begin(), v.end(), 100);
    if (a == v.end()) cout << "100을 찾을 수 없음." << "\n";

    // vector 값 채우기 - fill
    fill(v.begin(), v.end(), 10);
}
```
