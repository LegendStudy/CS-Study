# 2.2 TCP/IP 4계층 모델

## 2.2.1 계층 구조

![13.계층](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/13.계층.png)

![14.tcpip](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/14.tcpip.png)

### 애플리케이션 계층

FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜 계층, 웹 서비스, 이메일 등 서비스를 실질적으로 사람들에게 제공하는 계층

FTP: 장치와 장치 간의 파일을 전송하는 데 사용되는 표준 통신 프로토콜

SSH: 보안되지 않은 네트워크에서 네트워크 서비스를 안전하게 운영하기 위한 암호화 네트워크 프로토콜

HTTP: World Wide Web을 위한 데이터 통신의 기초이자 웹사이트를 이용하는 데 쓰는 프로토콜

SMTP: 전자 메일 전송을 위한 인터넷 표준 통신 프로토콜

DNS: 도메인 이름과 IP 주소를 매핑해주는 서버

### 전송 계층

전송 계층은 송신자와 수신자를 연결하는 통신 서비스를 제공

연결 지향 데이터 스트림 지원, 신뢰성 흐름 제어를 제공

대표적으로 TCP, UDP가 있음

TCP는 패킷 사이의 순서를 보장하고 연결지향 프로토콜을 사용하여 연결, 신뢰성을 구축해서 수신 여부를 확인.

`가상회선 패킷 교환 방식`을 사용

### 가상회선 패킷 교환 방식

각 패킷에는 가상회선 식별자가 포함되며 모든 패킷을 전송하면 가상회선이 해제되고 패킷들은 전송된 `순서대로` 도착하는 방식

![15.패킷교환](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/15.패킷교환.png)

데이터그램 패킷 교환 방식

패킷이 독립적으로 이동하며 최적의 경로를 선택

하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있으며 도착한 `순서가 다를 수` 있는 방식

![16.데이터그램](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/16.데이터그램.png)

### 3-WAY Hand Shake

TCP에만 존재. 신뢰성 있는 프로토콜

![17.3way](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/17.3way.png)

1. SYN 단계: 클라이언트는 서버에 클라이언트의 ISN을 담아 SYN을 보냄. ISN은 새로운 TCP 연결의 첫 번째 패킷에 해당된 임의의 시퀀스 번호
2. SYN + ACK 단계: 서버는 클라이언트의 SYN을 수신, 서버의 ISN을 보내며 승인 번호로 클라이언트의 ISN + 1을 보냄
3. ACK 단계: 클라이언트는 서버의 ISN + 1한 값인 승인 번호를 담아 ACK를 서버로 보냄

SYN: SYNchronization의 약자, 연결요청플래그

ACK: Acknowledgement 의 약자, 응답플래그

ISN: Initial Sequence Numbers의 약자, 초기 네트워크 연결을 할 때 할당된 32비트 고유 시퀀스 번호

### 4-WAY handshake

![18.4way](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/18.4way.png)

1번: 먼저 클라이언트가 연결을 닫으려고 할 때 FIN으로 설정된 세그먼트를 보냄. 클라이언트는 FIN_WAIT_1 상태로 들어가고 서버의 응답을 기다림.

2번: 서버는 클라이언트로 ACK라는 승인 세그먼트를 보냄. CLOSE_WAIT 상태에 들어감.클라이언트가 세그먼트를 받으면 FIN_WAIT_2 상태에 들어감

3번: 서버는 ACK를 보내고 일정시간 이후에 클라이언트에 FIN이라는 세그먼트를 보냄

4번: 클라이언트는 TIME_WAIT 상태가 되고 다시 서버로 ACK를 보내서 서버는 CLOSED 상태가 되고, 클라이언트는 대기한 후 연결이 닫고 연결을 종료함

### TIME_WAIT

소켓이 바로 소멸되지 않고 일정시간 유지되는 상태를 말하며 지연패킷등의 문제점을 해결하는 데 쓰임

CentOS6, 우분투에는 60초로 설정. 윈도우는 4분으로 설정

### 데이터 무결성(data integrity)

데이터의 정확성과 일관성을 유지하고 보증하는 것

## 인터넷 계층

장치로부터 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용되는 계층

상대방이 제대로 받았는지에 대해 보장하지 않는 비연결형

IP, ARP, ICMP 등

## 링크 계층 (네트워크 접근 계층)

전선, 광섬유, 무선 등으로 실질적으로 데이터를 전달, 장치 간 신호를 주고받는 '규칙'을 정하는 계층

물리, 데이터 링크 계층으오 나누기도 함.

물리 계층은 무선 LAN과 유선 LAN을 통해 0과 1로 이루어진 데이터를 보내는 계층.

데이터 링크 계층은 '이더넷 프레임'을 통해 에러 확인, 흐름 제어, 접근 제어를 담당하는 계층

### 유선 LAN(IEE802.3)

유선 LAN을 이루는 이더넷은 IEEE802.3이라는 프로토콜이며 전이중화 통신을 사용

~~~
전이중화 통신 VS 반이중화 통신

전이중화 통신: 양방향 동시 송신, 수신 가능

반이중화 통신: 양방향 통신, 동시 송신, 수신 불가
~~~

#### CSMA/CD

예전 유선 LAN에 '반이중화 통신'인 CSMA/CD를 사용했음. 보낸 이후 충돌이 발생하면 재전송하는 방식

수신로와 송신로를 구분한 것이 아니고 한 경로를 기반으로 데이터를 통신하므로 충돌에 대비해야했었음

### 무선 LAN(IEEE802.11)

수신과 송신에 같은 채널을 사용하기 때문에 반이중화 통신을 사용

동시에 전송하기 떄문에 메시지가 손실될 수 있어 충돌 방지 시스템이 필요

#### CSMA/CA

반이중화 통신

1. 사용 중인 채널이 있다면 다른 채널을 감지하다 미사용 상태인 채널을 발견
2. 프레임간 대기 순서가 있는데 이를 IFS라고 함. IFS가 낮으면 우선순위가 높음
3. 0 ~ 2^k - 1 사이에서 랜덤으로 뽑고, 제대로 받지 못했다면 k = k + 1을 하고 이 과정을 반복함
4. 반복하다 Kmax 보다 커지면 해당 프레임을 버림

#### BSS

기본 서비스 집합을 의미

동일 BSS 내에 있는 AP들과 장치들끼리 서로 통신 가능한 구조

하나의 AP만을 기반으로 구축 되어 있고 사용자가 한 곳에서 다른 곳으로 자유롭게 이동하며 네트워크 접속 하는 것은 불가능

#### ESS

하나 이상의 연결된 BSS 그룹

사용자가 움직이면서 네트워크를 연결할 수 있음

![19.essbss](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/19.essbss.png)

### 이더넷 프레임

데이터 링크 계층은 이더넷 프레임을 통해 전달받은 데이터의 에러를 검출, 캡슐화함.

![20.이더넷프레임](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/20.이더넷프레임.png)

Preamble: 이더넷 프레임 시작을 알림

SFD(Start Frame Delimiter): 다음 바이트로부터 MAC 주소 필드가 시작됨을 알림

DMAC, SMAC: 수신, 송신 MAC 주소

EtherType: 데이터 계층 위의 계층은 IP 프로토콜을 정의, IPv4 또는 IPv6

Payload: 전달받은 데이터

CRC: 에러 확인 비트

## 계층 간 데이터 송수신 과정

![21.계층간데이터송수신과정](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/21.계층간데이터송수신과정.png)

캡슐화와 비캡슐화

#### 캡슐화 과정

![22.캡슐화 과정.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/22.캡슐화과정.png)

애플리케이션 계층의 데이터가 전송 계층으로 전달되면서 '세그먼트'또는 '데이터그램'화되며 TCP(L4) 헤더가 부텽짐

그 후 인터넷 계층으로 가면서 IP(L3) 헤더가 붙어 '패킷'화됨.

그 후 링크 계층으로 전달되면서 프레임 헤더와 프레임 트레일러가 붙어 '프레임'화가 됨

#### 비캡슐화과정

![23.비캡슐화과정.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/23.비캡슐화과정.png)

비캡슐화 과정을 거쳐 사용자에게 애플리케이션의 PDU인 메시지로 전달됨

## 2.2.2 PDU

네트워크의 어떠한 계층에서 계층으로 데이터가 전달될 때 한 덩어리의 단위를 PDU(Protocol Data Unit)라고 함

PDU는 제어 관련 정보들이 포함된 '헤더', 데이터를 의미하는 '페이로드'로 구성되며 계층마다 부르는 명칭이 다름

- 애플리케이션 계층: 메시지
- 전송 계층: 세그먼트(TCP), 데이터그램(UDP)
- 인터넷 계층: 패킷
- 링크 계층: 프레임(데이터 링크 계층), 비트(물리 계층)