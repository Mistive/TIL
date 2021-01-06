# IoT 구조

## 목표
내 자취방을 IoT 기기로 꾸미고 싶다.
문제는 관련된 지식이 거의 없다는 것. 한 번 차근차근 그림을 그려보자.


## IoT 구조
![image](https://user-images.githubusercontent.com/39082893/103770919-2310ef80-506a-11eb-9f11-fc1346df39c4.png)
IoT는 기본적으로 Device라고 적혀있는 IoT 기기들(스마트 전구, 공기 청정기 등등등등등)이 있고, 이를 IoT 서버와 연결해줄 수 있는 인증 기능(oneM2M, OCF 등)과 인증된 기기들에게 Message를 주고 받을 수 있게 해주는 Message Broker가 있다.

그리고 IoT 디바이스로부터 받은 다양한 정보들을 처리할 수 있는 **Service Connection** 부분이 있고 이 부분은 여러가지 Module(데이터 분석, 머신러닝, 날씨 정보 등)들과의 상호작용을 통해 결과를 만들어 내어 User에게 제공한다.

또는 User로 붙어 입력받은 정보를 토대로 IoT 디바이스를 동작시키게 명령하기도 한다.

## 일의 순서
그렇다면 가장 먼저 무엇을 해야 할까?

나는 **라즈베리파이를 이용해 IoT 서버를 만들고** 다른 라즈베리파이 또는 IoT 기기들을 내가 만든 IoT 서버와 연동 시키려고 한다.

그 후 내가 만든 IoT 서버와 현재 시장에서 사용되고 있는 다양한 User Application들(구글 Home Mini, Samsung SmartThings, 빅스비)과 내 서버를 연결할 예정이다.

그럼 일단 제일 먼저 할 것은 IoT 서버의 큰 틀을 잡아야 할 것 같다.

## 01. IoT 서버 구조

