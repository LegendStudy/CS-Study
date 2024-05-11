# 2.5 HTTP

## 2.5.1 HTTP/1.0

한 연결당 하나의 요청을 처리하도록 설계. RTT 증가를 일으킴

### RTT 증가

![35.RTT증가.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/35.RTT증가.png)

서버로부터 파일을 가져올 때 마다 TCP의 3-Way Hanshake 때문에 RTT가 증가하는 단점이 있음

~~~
RTT
패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간. 패킷 왕복 시간
~~~

## 2.5.2 HTTP/1.1

매번 TCP 연결을 하지 않고 한 번 TCP 초기화를 한 이후에 keep-alive 옵션으로 여러 개의 파일을 송수신할 수 있게 바뀜

![36.HTTP1.1비교.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/36.HTTP1.1비교.png)

한 번 3-Way Hanshake가 발생하면 그 다음부터 발생하지 않음

### HOL Blocking

네트워크에서 같은 큐에 있는 패킷이 그 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저아 현상

![37.HOL.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/37.HOL.png)

그림처럼 image가 느리게 받아지면 그 뒤에 있는 것들이 대기하게 되는 현상

### 무거운 헤더 구조

HTTP/1.1 헤더에는 쿠키 등 많은 메타데이터가 들어 있고 압축이 되지 않아 무거움

## 2.5.3 HTTP/2

### 멀티플렉싱

여러 개의 스트림을 사용하여 송수신하는 것

특정 스트림의 패킷이 손실되어도 해당 스트림에만 영향을 미쳐 나머지 스트림은 멀쩡하게 동작 가능

~~~
스트림(stream)
시간이 지남에 따라 사용할 수 잇게 되는 일련의 데이터 요소를 가리키는 데이터 흐름
~~~

하나의 연결 내 여러 스트림을 캡처한 모습

![38.멀티플렉싱.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/38.멀티플렉싱.png)

병렬적인 스트림들을 통해 데이터를 서빙

스트림 내의 데이터들을 쪼갬

애플리케이션에서 받아온 메시지를 독립된 프레임으로 조각내어 서로 송수신한 이후 다시 조립하여 데이터를 통신함

![39.HTTP2.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/39.HTTP2.png)

단일 연결을 사용하여 병렬로 여러 요청을 받을 수 있고 응답을 줄 수 있음. 이를 통해 HOL Blocking을 해결할 수 있음

### 헤더 압축

HTTP/1.x에는 크기가 큰 헤더이기 때문에 압축하여 해결

![40.헤더압축.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/40.헤더압축.png)

HTTP/2에서는 헤더 압축을 써서 해결함

허프만 코딩 압축 알고리즘을 사용하는 HPACK 압축 형식을 가짐

### 허프만 코딩

문자열을 문자 단위로 쪼개 빈도수를 세어 빈도가 높은 정보는 적은 비트 수를 사용하여 표현, 빈도가 낮은 정보는 비트 수를 많이 사용하여 표현해 전체 데이터 표현에 필요한 비트양을 줄이는 원리

### 서버 푸시

HTTP/1.1에서는 클라이언트가 서버에 요청을 해야 파일을 다운로드 받을 수 있었지만 HTTP/2에서는 클라이언트 요청 없이 서버가 바로 리소스를 푸시할 수 있음

![41.서버푸시.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/41.서버푸시.png)

## 2.5.4 HTTPS

HTTP/2는 HTTPS 위에서 동작. `HTTPS`: HTTP + SSL/TLS 계층


SSL 1.0부터 시작해 SSL 2.0, SSL 3.0, TLS 1.0, TLS 1.3 까지 버전을 올려 마지막으로 TLS로 명칭이 바뀌었으나 보통 SSL/TLS로 부름

SSL/TLS은 전송 계층에서 보안을 제공하는 프로톸콜

Clinet와 Server가 통신할 떄 SSL/TLS를 통해 제 3자가 메시지를 도청하거나 변조하지 못하게 함

![42.인터셉터방지.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/42.인터셉터방지.png)

SSL/TLS를 통해 공격자가 서버인 척하며 사용자 정보를 가로채는 네트워크 상의 `인터셉터`를 방지할 수 있음

### 보안 세션

보안이 시작되고 끝나는 동안 유지되는 세션을 말함

SSL/TLS는 핸드셰이크를 통해 보안 세션을 생성하고 이를 기반으로 상태 정보 등을 공유함

클라이언트와 서버와 키를 공유하고 이를 기반으로 인증, 인증 확인 등의 작업이 일어나는 단 한 번의 1-RTT가 생긴 후 데이터를 송수신 함

클라이언트에서 사이퍼 슈트를 서버에 전달하면 서버는 받은 사이퍼 슈트의 암호화 알고리즘 리스트를 제공할 수 있는지 확인함

제공할 수 있다면 서버에서 클라이언트로 인증서를 보내는 인증 메커니즘이 시작되고 해싱 알고리즘 등으로 암호화된 데이터의 송수신이 시작됨

### 인증 메커니즘

CA에서 발급한 인증서를 기반으로 이루어짐

CA에서 발급한 인증서는 안전한 연결을 시작하는 데 있어 필요한 `공개키`를 클라이어트에 사용자가 접속한 `서버가 신뢰`할 수 있는 서버임을 보장함

### 암호화 알고리즘

키 교환 암호화 알고리즘으로는 대수곡선 기반의 DCDHE 또는 모듈식 기반의 DHE를 사용함. 디피-헬만 방식이 근간

### 해싱 알고리즘

데이터를 추정하기 힘든 더 작고, 섞여 있는 조각으로 만드는 알고리즘

SSL/TLS는 해싱 알고리즘으로 SHA-256 알고리즘으과 SHA-384 알고리즘을 사용

### SHA_256

함수 결과값이 256비트인 알고리즘이며 비트 코인을 비롯한 많은 블록체인 시스템에서도 사용함

SHA_256알고리즘은 해시 함수의 결괏값이 256 비트인 알고리즘이며 비트 코인을 비롯한 많은 블록체인 시스템에서도 사용함

SHA-256 알고리즘은 해싱을 해야 할 메시지에 1을 추가하는 등 전처리를 하고 전처리된 메시지를 기반으로 해시를 반환함

~~~
해시
다양한 길이를 가진 데이터를 고정된 길이를 가진 데이터로 매핑한 값

해싱
임의의 데이터를 해시로 바꿔주는 일이며 해시 함수가 이를 담당

해시 함수
임의의 데이터를 입력으로 받아 일정한 길이의 데이터로 바꿔주는 함수
~~~
TLS 1.3은 사용자가 이전에 방문한 사이트로 다시 방문하면 SSL/TLS에서 보안 세션을 만들 때 걸리는 통신을 찾지 않아도 됨 0-RTT라고 함

### HTTPS 구축 방법

HTTPS 구축 방법은 크게 3가지

직접 CA에서 구매한 인증키를 기반으로 HTTPS 서비스를 구축

서버 앞단의 HTTPS를 제공하는 로드밸런서를 둠

서버 앞단에 HTTPW를 제공하는 CDN을 둬서 구축

## 2.5.5 HTTP/3

QUIC 계층 위에서 돌아가며 TCP 기반이 아닌 UDP 기반으로 동작

![43.HTTP3.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/43.HTTP3.png)

HTTP/2 에서의 장점인 멀티플렉싱을 가지고 있으며 초기 연결 설정 시 지연 시간 감소라는 장점을 가지고 있음

### 초기 연결 설정 시 지연 시간 감소

QUIC는 TCP를 사용하지 않기 떄문에 3-Way Hanshake를 하지 않아도 됨

QUIC는 첫 연결 설정에만 1-RTT만 소요됨

오류 수정 메커니즘(FEC, Forword Error Correction)이 적용되어있음

이는 전송한 패킷이 손실되면 수신 측에서 에러를 검출하고 수정하는 방식. 열악한 네트워크 환경에서도 낮은 패킷 손실률을 갖고 있음