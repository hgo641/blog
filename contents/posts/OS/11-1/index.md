## Dining-Philosophers Problem

각 철학자들은 밥을 먹어야할 때 소통

두 명이 젓가락 한 짝씩 공유

옆에 있는 젓가락만 사용가능 ( i번째 사람은 i또는 i+1만)



* semaphore chapstic[5]; # binary semaphore(=mutex lock)

* signal()  # release 밥을 다 먹음



### Deadlock

내가 원하는 자원을 상대방이 점유하고있어서 다음으로 넘어갈 수 없음

ex. 모든 사람들이 다 자신의 왼쪽에 있는 젓가락 한 짝을 소유중

모두가 계속 기다림

#### 해결방안 1

다섯명의 철학자가 있지만 책상에 앉을 수 있는 사람을 4명으로 제한

* 적어도 한 명은 젓가락 한 쌍을 얻을 수 있다



#### 해결방안 2

젓가락 두 개가 동시에 사용 가능할때만 젓가락을 집게한다

* 두 줄의 코드가 atomic하게 
* 크리티컬 섹션으로. 내가 젓가락을 집을 때 다른 사람이 집을 수 없음

-> 모니터를 사용해서 구현

state hungry 추가

thinking

hungry

eating : 두 이웃이 젓가락을 안쓰고있을때



condition : semaphore + waiting queue + wait signal 함수



#### 해결방안 3

asymmetric한 구조를 만든다

wait(i+1)

wait(i)

순으로 하게 변경

모두가 동시에 왼쪽 젓가락을 집지않는다





데드락의 조건

* Mutual exclusion
  * 한 명이 리소스를 차지하고있으면 다른 사람이 리소스를 차지할 수 없다
* No premmption
  * 한 번 자원을 얻으면 할 일이 끝날 때까지 자원을 양보하지않는다
* Hold and wait
* Circular wait
  * 원을 그리면서 서로가 서로의 것을 원하는... 상태

위 조건 중 하나라도 만족하지않으면 문제가 생기지않음