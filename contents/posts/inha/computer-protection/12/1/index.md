버퍼 오버 플로우 방어 방법?



## Exploit Protection

CFG (Control Flow Guard) : 제어흐름보호

DEP (Data Execution Prevention) : 데이터 실행 방지 (데이터 영역에 있는건 실행시키지 않겠다)

ASLR



### DEP (Data Execution Prevention)

하드웨어에서도 지원

메모리에 이상한 일이 벌어지는지 체크

wmic 

* 2- optin : 디폴트값 - 검사대상을 내가 지정 (윈도우시스템즈, 중요기능에만 적용되어있음)
* 3-optout : 모든 프로세스에 dep적용 - 안하고싶은애는 따로 설정
* 1-alwayson : 항상키기
* 0-alwaysout



### ASLR (Address space layout randomization)

주소 공간의 배치를 랜덤화함

* 메모리를 분배시켜서 메모리를 공격하는 것을 막아줌
* 공격자가 메모리 상에 실행가능한 특정 자리쪽으로 튀는 것을 막기위해서 aslr적용
* 프로세스가 사용하는 중요한 영역의 랜덤으로 바꿈
* 스택 위치, 힙 위치, 동적 라이브러리 위치 다 바꿈
* 이걸 깨는 공격들이 또 나옴



### CFG (Control Flow Guard)

방금 나온 DEP, ASLR을 포함, 확장한 개념

최근에 나옴

OS만 가지고 되는게 아니고 Visual studio랑 협업해야함

* VS 2015에 중간중간 체크하는 코드가 들어가있음



컴파일하고 런타임서포트를 합침

컴파일은 컴파일러가 런타임 서포트는 윈도우즈 커널이 함

호출하는 instruction의 위치를 포인터로 받음 - 이런걸 포함하고 있는 소스코드가 있다면 컴파일 타임에 확인함, 

컴파일러는 간단한 체크 코드를 집어넣음 : Instrumentaion 

indirect call, 함수가 콜 됐을때 콜의 대상이 될 수 있는 위치를 다 적어둠 - 유효한 목표 주소점을 가지고있음

실제 실행시킬 때 위에서 컴파일러가 적어둔 곳으로 간다고 하면 okay,  컴파일러가 적어둔 곳이 아니면 이상함을 감지 - isValid target인가?



#### Instrumentation

> 동적으로 실행시키는 상황에서 분석

위에서 한거는 Source instrumentation

* 컴파일러가 소스 단계에서 뭔갈 집어넣음

Binary instrumentaion

* source를 안가지고있고 (컴파일러가 아닌 그냥 커널)
* 바이너리 코드(기계어) 상에서 이것 저것 집어넣음
  * intel Pin - 툴이라기보단 가상환경에 가까운데 컴퓨터 아키텍처를 분석하는 용도로 쓰였음. 메모리 분석하는 용도로도 쓰이는중
  * Valgrind - 메모리 디버깅용으로 나온 툴. 디폴트 : Memcheck

ex

동적할당해놓고 free 안해둠 - 메모리안에 갈비지값 생김 - Memcheck가 이런거 체크해줌

return 0; 하는 순간 400byte의 공간을 여전히 사용하고있음을 알려줌



## Format String Attack

Format string에서 %사용 가능 - 길이가 가변적인 파라미터를 받을 수 있음. 

ex) 

print(argv[1])하면

print(Hello World %p %p %p ...)가 되어버림

즉 %p의 개수만큼 인자가 있어야함. (%p는 정수값출력-보통 포인트값을 출력해줌 16진수8자리)

스택 주변에 있는 값들을 그냥 프린트해버림 - 주변의 스택값 출력 : 스택의 일부영역 노출

보통 폴맷스트링어택과 버퍼오버플로우가 같이 쓰임



## Security Systems

방어하기 위해 보통 사용하는 시스템

보안 기능을 제공하는 하드웨어 기능

방화벽

IDS, IPS



### Hardware Support for Security

#### TPM (Trusted Platform Module)

coprocessor - cpu를 도와주는 보조장치

암호와 관련된 기능을 도와줌

대부분의 상용화된 pc, 서버, 스마트폰에 다 들어있음

하드웨어 보안 관련

메인 기능

* 암호 키를 생성하거나 저장
* 디바이스 자체를 인증, 식별 (전자서명으로 확인)
* 플랫폼 무결성 확인 (secure boot같은거 사용)
  * 커널코드를 H함수로 돌리고 사인(개인키) - 서명
  * OS는 저 커널이 제대로 된 커널인지 확인하고 어플리케이션을 실행해야함
  * 덮어쓰기가 안되는 영역(ROM)에 서명된 결과와 공개키를 저장해둠
  * 공개키를 가지고 인증 확인
* 사용자가 쓰고있는 데이터 중 중요한 데이터를 저장 -NVRAM(비휘발성램) - 안전한 플래시메모리 영역
* 2.0버전을 주로 씀



#### TEE (Trusted Execution Environment)

신뢰 실행 환경

cpu안에 있는 논리적인 영역

cpu안에 구현된 기능

cpu안의 영역을 분리해서 특정 부분에 키를 할당에 놓음

normal world/ secure world

시큐어 월드에 있는 프로세스 상태나 데이터를 노멀월드에서는 볼 수 없음. 서로 다른 cpu처럼 작동

무결성, 기밀성

부트스트랩 코드 같은 특정 코드에 한정됨

remote attenstation : 외부에서 cpu가 잘 동작하고 있는지 확인 가능

ex) ARMCPU, SGX



#### HSM(Hardware Security Module)

밖에서 물리적으로 조작이 불가능(Tamper-resistant)

카드형태로 꽂던가 ... 

값이 비쌈

앞에 애들보다 고성능

어떤 연산을 안전하게 수행해줌



## Firewall

시스템 보호

로컬시스템보호, 시스템들이 연결된 네트워크 보호, 외부, 내부적 위험에서 보호

* 양방향 네트워크 서비스, 접근 제어
  * 바깥에서 오는 트래픽 필터링
  * 프록시 소프트웨어
  * 서버 자체로 작동하기도

* 사용자 제어
* 근접 제어
* 로그를 남기고 감사



### Firewall Types

대표적 종류 두 개

#### Packet filter

패킷을 필터링

access control list를 유지하면서 소스, 데스트의 IP주소, port, protocol을 필터

tcp, ip 네트워크 트랜스포트 계층에서 패킷의 모양만 가지고 함. 페이로드(내용)은 관심없음



#### Application Firewall

페이로드도 봄

프록시 형태로 제공되는 경우가 많음

Application level gateway라고 부르기도 함

프록시가 외부 서버에서 데이터 받고 정상적이면 로컬 네트워크의 pc에 전달. 정상적이지 않으면 전달하지않음

WAF에 따라 다르지만 DPI를 하는 경우도 있음

* DPI (Deep Packet Inspection)
  * 패킷의 헤더뿐만아니라 내용까지 봄
  * 암호화된 트래픽은 볼 수 없음

## IDS 

침입자를 색출해내는게 목표

공격자의 행동은 정상적인 유저의 행동과 다를것이다라고 가정

오용탐지 (Misuse detection)

* rule or signature기반
* 알려져있는 공격을 규칙으로 사용
* 공격의 자취를 저장
* 패킷을 검사하다가 저장해둔 자취와 비슷하면 공격자라고 판단
* 정확도는 높지만 제로데이와 같이 알려지지않은 공격에 취약

이상탐지

* 평소에 정상적인 시스템의 상대를 학습시킴
* 머신러닝이나 통계기술이 많이 사용됨
* 학습시킨 시스템에서 벗어난 행동을 하면 공격자라고 판단



구현되는 위치로 구분 

Host-based IDS

* 컴퓨터안에 os하고 같이 설치

Network-based IDS

* 컴퓨터들은 따로 따로 있는데 그 네트워크에 별도로 설치
* 여러대의 컴퓨터를 동시에 확인 가능
* IP 주소가 없어서 공격하기 어려움



## IPS

IDS보다 발전

IDS : 공격 탐지, 그러나 어떻게 해결할 것인지는 애매함

IPS : 발전된 형태의 아이디에스, 네트워크를 이상한 행동을 하는지 지속적으로 모니터링을 하면서 막기위해 어떤 행동(reporting, blocking, dropping)을 취한다

최근에는 시스템이 발전하면서 IPS = IDS + Firewall이라고 부르기도 함 (경계가 애매)



PCRE : reqular expression : \s+, \n 0x22... 이런애들. 규칙을 정해놓고 규칙에 맞는지 확인



## 