---
title: "13-1"
date: 2022-06-14
tags:
  - computer protection
series: "컴퓨터 보안"
---

## Other Security Solutions

* Virtual Private Network (가상사설망)
  *  사설망 : 외부로부터 분리된 특별한 목적을 가지고 생성된 망
  * 사설망을 모든곳에 깔긴 어려우니 가상으로 생성
  * IPsec을 가지고 많이 구현함
  * 암호화와 인증 제공 => 터널화

* 백신, 피씨방화벽
* Spam filter
* Data Loss Prevention(DLP)
  * 민감한 데이터가 분실되거나 악용되는 것을 방지하기위한 툴
  * 기업에서 중요한 데이터를 외부로 빠져나가지 못하게 함
  * 개인 직원이 주요 데이터를 빼돌리는 것 말고도 공격자가 들어와서 중요 데이터를 기업의 이미지를 훼손시키기 위해 빼가는 경우 - 개인 정보가 유출되었기에 패널티를 받음(벌금등)
  * 데이터가 어디에 어떻게 사용되고 어디로 이동했는지에 사용
* Endpoint(Threat) Detection and Respose (EDTR)
  * 단말내에서 이상한 일이 생기는지 탐지하고 대응
  * 랜섬웨어, 멜웨어 등을 막음
  * 원격에 있는 클라우드에서 개인의 pc를 감시
* Security information and event management(SIEM)
  * 로그같은걸 자동분석
  * 이벤트 자동분석
  * 사용자, entity(주체)의 동작을 분석
  * 머신러닝, ai의 도움
* Security orchestration, automation and response (SOAR)
  * 종합관리
  * 일일히 사람이 신경쓰기 어려우니 자동화
  * 공유, 자동화, 조직화, 표준화





## CNN

convolutional neural network

* 화면으로 찍은 사진을 가지고 어떤 물체인지 분류하는데 사용
* 특정 영역에 연산을 해서 결과를 얻어냄
* 일부 영역에 대한 특징을 압축해서 나타냄
  * filter라고 부르는 행렬과 곱셈을 수행
  * U = XW + b
  * X : input
  * W : weight vector (filter)
  * b : bias
  * W와 b를 잘만들어내는게 인공지능에서 traning, learning에 해당

* 활성화 함수
  * ReLU
  * 원소 하나하나에 대해서 독립적으로 적용
* 풀링
* feature vector (final output)
* softmax function
  * feature vector의 각각의 값을 극대화시킴
  * 차이를 극명하게 하기위해 사용



W와 b를 합쳐 세타(파라미터)라고 칭함

* 빨간 화살표가 에러
* 검은 점이 actual
* 그래프가 target

에러를 줄이는 방향으로 (Gradient Descent)

training data set을 가지고 모델을 트레이닝

내가 관찰한 값과 cnn으로 나온 값이 비슷하게 만들어주는 과정



### 적대적 예제

잘 훈련된 모델을 만듦

공격자는 입력을 조작함

그랬더니 함수의 오류가 증가

경계선을 넘어가 오류가 넘어갈만한 데이터를 입력으로 보냄

사람에겐 보이지않는 노이즈를 추가해서 기계가 잘못 판단하게



막자!

## countermeasures to adversarial examples : AI Robustness

* Adversarial training
  * 노이즈를 집어넣어서 경계선을 더 잘 긋도록 만들자
* Input transformation (Denoiser)
  * 인풋을 바꿔줌
  * 노이즈 섞인 이미지를 그대로 넣지 않고 노이즈가 제거되게 변환을 해줌
* Model Ensemble
  * 분류기를 하나만 쓰지말자
  * 한 분류기가 특정 노이즈한테 약할 수 있음
  * 여러 분류기가 똑같이 분류해야 결과를 내놓겠다!
  * 비용이 많이 들긴함

## Data Protection and Privacy

* Strava
  * 중동에 있는 미국기지의 위치를 노출시킴
  * 자기의 위치를 인식시키고 실행시킨 상태에서 운동하면 운동 경로 확인 가능
  * 막는 방법은 데이터를 공유못하게한다!

* GPT : 자연어 처리하는 모델 중 하나
  * 자연어 처리 모델을 학습하는데 사용된 데이터에서 개인정보 노출
  * GPT는 입력을 주면 그 다음 내용을 예상함

#### 막는 방법

* Differential Privacy(차분 privacy)
  * 차이가 있는 privacy
  * 프라이버시를 수학적으로 잘 정의
  * 내 데이터를 넣고 트레이닝 시켰을 때와 빼고 트레이닝 시켰을 때의 쿼리 결과가 구분이 안되면 dffirential privacy를 만족한다고 함
  * 어떤 개인의 데이터 포함 유무에 상관없이 모델의 결과가 바뀌지않을 때
  * 학습의 성능은 최대한 유지하면서 privacy도 보호
* Homomorphic Encryption (동형암호)
  * 어떤 암호문안에 있는 평문의 구조를 유지하는 암호문
  * 암호화를 한 다음에 결과에 대해서 계산을 한 것 = 계산 먼저하고 암호화 한 것
  * AES처럼 완전 랜덤한 것은 이 성질을 만족할 수 없음(랜덤bit가 완전 다른 결과를 만들어냄)
  * RSA textbook에서는 만족
  * 상자에 넣는 과정이 동형암호...
  * 검사 연산이 금세공...

데이터를 암호화해서 서버에 올림

서버는 암호화 한 상태에서 연산 수행

다시 데이터 주인에게 전달해서 암호를 풀어보면 유용한 정보 획득

서버는 모델을 사용자에게 공유하고싶지않고

사용자는 자신의 데이터를 서버에 노출시키고 싶지않음

두 조건을 만족하게 homomorphic encryption

* 근데 속도가 느림



## Anomaly Detection

이상탐지

ips(침입방지)  : 공격자의 행위를 분석해서 방지

snort : 행위를 시그너처로 만들고 탐지

알려진 공격에 대해서는 잘찾음

알려지지않은 공격을 탐지할 때는 정상적인 유저의 행동을 잘학습시키고 벗어나면 이상 탐지

지도학습, 반지도학습보다는 unsupervised learing이 적합

훈련할 때는 정상 데이터만? 그냥 데이터 주고 모델은 정상 행동으로 학습

  