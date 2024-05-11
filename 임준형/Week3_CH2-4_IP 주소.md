# 2.4 IP 주소

## 2.4.1 ARP

IP 주소에서 ARP를 통해 MAC 주소를 찾아 MAC 주소를 기반으로 통신함

ARP(Address Resolution Protocol)란 IP 주소로부터 MAC 주소를 구하는 IP와 MAC 주소의 다리역할을 하는 프로토콜

ARP를 통해 가상 주소인 IP 주소를 실제 주소인 MAC 주소로 변환함

이와 반대로 RARP를 통해 실제 주소인 MAC 주소를 가상 주소인 IP 주소로 변환하기도 함

![29.ARP.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/29.ARP.png)

![30.ARP과정.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/30.ARP과정.png)

A가 ARP Request 브로드캐스트를 보내 IP 주소인 120.70.80.3에 해당하는 MAC 주소를 찾음

해당 주소에 맞는 장치 B가 ARP Reply 유니캐스트를 통해 MAC 주소로 변환하는 과정을 거쳐 IP 주소에 맞는 MAC 주소를 찾음

## 2.4.2 홉바이홉 통신

IP 주소를 통해 통신하는 과정을 홉바이홉 통신이라고 함

여기서 홉이란 영어 뜻 자체로는 건너뛰는 모습을 의미함

통신망에서 각 패킷이 여러 개의 라우터를 건너가는 모습을 비유적으로 표현한 것

수많은 서브네트워크 안에 있는 라우터의 라우팅 테이블 IP를 기반으로 패킷을 전달하고 또 전달하여 라우팅을 수행, 최종 목적지까지 패킷을 전달함

![31.홉바이홉.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/31.홉바이홉.png)

통신 장치에 있는 `라우팅 테이블`의 IP를 통해 시작 주소부터 시작하여 다음 IP로 계속해서 이동하는 `라우팅`과정을 거쳐 패킷이 최종 목적지까지 도달하는 통신을 말함

~~~
라우팅
IP 주소를 찾아가는 과정
~~~

### 라우팅 테이블

송신지에서 수신지까지 도달하기 위해 사용

라우터에 들어가 있는 목적지 정보들과 그 목적지로 가기 위한 방법이 들어있는 리스트

라우팅 테이블에는 게이트웨이와 모든 목적지에 대해 해당 목적지에 도달하기 위해 거쳐야 할 다음 라우터의 정보를 가지고 있음

### 게이트웨이

서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는 관문 역할을 하는 컴퓨터나 SW를 두루 일컫는 용어

![32.게이트웨이.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/32.게이트웨이.png)

## 2.4.3 IP 주소 체계

IP 주소는 IPv4와 IPv6로 나뉨

IPv4는 32비트를 8비트 단위로 점을 찍어 표기. `123.45.67.89`와 같이 나타냄

IPv6는 64비트를 16비트 단위로 점을 찍어 표기 `2001:db8::ff00:42:8329`와 같이 나타냄

### 클래스 기반 할당 방식

앞에 있는 부분을 `네트워크 주소`, 뒤에 있는 부분을 컴퓨터에 부여하는 주소인 `호스트 주소`로 사용 

![33.클래스기반할당방식.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/33.클래스기반할당방식.png)

A, B, C 클래스는 일대일 방식으로 사용되고 D 클래스는 멀티캐스트 통신이다.

![34.상세내역.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/34.상세내역.png)

맨 왼쪽에 있는 비트를 `구분 비트`라고 함

네트워크의 첫 번째 주소는 네트워크 주소로 사용, 가장 마지막 주소는 브로드캐스트용 주소로 네트워크에 속해 있는 모든 컴퓨터에 데이터를 보낼 때 사용

만약 클래스 A로 12.0.0.0이란 네트워크를 부여받았을 때 `12.0.0.1` ~ `12.255.255.254`의 호스트 주소를 부여받은 것

이 방식은 사용하는 주소보다 버리는 주소가 많은 단점이 있어 `DHCP`, `IPv6`, `NAT` 등이 해결책

### DHCP(Dynamic Host Configuration Protocol)

IP 주소 및 기타 톷신 매개변수를 자동으로 할당하기 위한 네트워크 관리 프로토콜

네트워크 장치의 IP 주소를 수동으로 설정할 필요 없이 인터넷에 접속할 때마다 자동으로 IP 주소를 할당

많은 라우터와 게이트웨이 장비에 DHCP 기능이 있으며 이를 통해 대부분의 가정용 네트워크에서 IP 주소를 할당

### NAT

Network Address Translation

패킷이 라우팅 장치를 통해 전송되는 동안 패킷의 IP 정보 주소를 수정하여 IP 주소를 다른 주소로 매핑하는 방법

IP 수가 부족한 것을 NAT를 활용하여 공인 IP와 사설 IP로 나눠 해결

NAT를 가능하게 하는 SW는 ICS, RRAS, Netfilter 등
