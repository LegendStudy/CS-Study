# 1.2 프로그래밍 패러다임

프로그래머에게 프로그래밍의 고나점을 갖게 해주는 역할을 하는 개발 방법론

객체지향 프로그래밍: 플고르개럼들이 프로그램을 상호 작용하는 객체들의 집합으로 볼 수 있음

함수형 프로그래밍: 상태 값을 지니지 않는 함수 값들의 연속으로 생각할 수 있음

그로그래밍 패러다임은 크게 선언형, 명령형으로 나눔

선언형: `함수형` 하위 집합을 가짐

명령형: `객체지향`, `절치지향`으로 나뉨

![1.패러다임분류](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/1.패러다임분류.png)

## 1.2.1 선언형과 함수형 프로그래밍

선언형 프로그래밍은 `무엇을 풀어내는가`, `프로그램은 함수로 이루어짐`

함수형 프로그래밍은 선언형 패러다임의 일종

함수형 프로그래밍은 커링, 불변성 등 많은 특징이 있음

### 순수 함수

출력이 입력에만 의존하는 것

```javascript
const pure = (a, b) => {
    return a + b
}
```
pure 함수는 매개변수 a, b에만 영향을 받음. 만약 a,b 외의 다른 전역 변수가 출력에 영향을 주면 순수 함수가 아님


### 고차 함수

함수가 함수를 값으로 매개 변수로 받아 로직을 생성할 수 있는 것

### 일급 객체

이때 고차 함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야함

~~~ 
일급 객체 특징
- 변수나 메서드에 함수를 할당할 수 있음
- 함수 안에 함수를 매개변수로 담을 수 있음
- 함수가 함수를 반환할 수 있음
~~~

## 1.2 객체지향 프로그래밍

객체들의 집합으로 프로그램의 상호 작용을 표현하며 데이터를 객체로 취급, 객체 내부에 선언된 메서드를 활용하는 방식

설계에 많은 시간이 소요되며 처리 속도가 다른 패러다임에 비해 **상대적으로 느림**

추상화, 캡슐화, 상속성, 다형성

### 추상화

복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려 내는 것

### 캡슐화

객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것

### 상속성

상위 클래스의 특성을 하위 클래스가 이어받아서 재사용 및 추가, 확장하는 것

재사용, 계층적인 관계 생성, 유지 보수성 측면에서 중요

### 다형성

하나의 메서드나 클래스가 다양한 방법으로 동작하는 것

ex) overloading, overriding

### 오버로딩
같은 이름을 가진 메서드를 여러 개 두는 것, 

`정적 다형성`

```java
class Person {
    public void eat(String a) {
        System.out.println("I eat "
                + a);
    }

    public void eat(String a, String b) {
        System.out.println("I eat "
                + a + " and " + b);
    }
}
public class CalculateArea {
    public static void main(String[] args) {
        Person a = new Person();
        a.eat("apple");
        a.eat("tomato", "phodo");
    }
}

/* 
    I eat apple
    I eat tomato and phodo 
*/
```

### 오버라이

메서드 오버라이딩을 말함

상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것을 의미

`동적 다형성`

```java
class Animal {
    public void bark() {
        System.out.println("mumu! mumu!");
    }
}

class Dog extends Animal {
    @Override
    public void bark() {
        System.out.println("wal!!! wal!!!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.bark();
    }
}
/*
    wal!!! wal!!! 
*/
```

### 설계 원칙

SOLID 원칙
~~~
S: 단일 책임 원칙
O: 개방-폐쇄 원칙
L: 리스코프 치환 원칙
I: 인터페이스 분리 원칙
D: 의존 역전 원칙
~~~

### 단일 책임 원칙 (SRP, Responsibility Principle)

모든 클래스는 각각 하나의 책임만 가져야 하는 원칙

A라는 로직이 존재시 어떠한 클래스는 A에 관한 클래스여야하고 수정할 때도 A와 관련된 수정이여야함

### 개방 폐쇄 원칙 (OCP, Open Closed Principle)

유지 보수 사항이 생길시 코드를 쉽게 확장할 수 있어야하며 수정할 때는 닫혀있어야하는 원칙

기존 코드는 잘 변경하지 않으면서 확장은 쉽게 가능하도록 설계해야함

### 리스코프 치환 원칙 (LSP, Liskov Substitution Principle)

프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미

클래스는 상속이 이루어지며 부모, 자식이라는 계층 관계가 생성됨

이때 부모 객체에 자식 객체를 넣어도 시스템이 문제 없이 돌아가게 만드는 것을 말함

### 인터페이스 분리 원칙 (ISP, Interface Segregation Principle)

구체적인 여러 개의 인터페이스를 만들어야하는 원칙

### 의존 역전 원칙 (DIP, Dependency Inversion Principle)

자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않는 원칙

타이어를 갈아끼울 수 있는 틀을 만들어 놓은 후 다양한 타이어를 교체할 수 있어야함 -> interface 사용하여 개발

상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야하는 것

## 1.2.3 절차형 프로그래밍

로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있음

코드의 가독성이 좋으며 실행 속도가 빠름

계산이 많은 작업에 쓰임. 포트란을 이용한 대기 과학 관련 연산 작업, 머신 러닝의 배치작업 등이 있음

모듈화가 어렵고 유지 보수성이 떨어짐

# Chapter2 네트워크

# 2.1 네트워크의 기초

네트워크란 노드와 링크가 서로 연결되어 있거나 연결되어 있으며 리소스를 공유하는 집합을 의미

![2.노드와링크](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/2.노드와링크.png)

노드란 서버, 라우터, 스위치 등 네트워크 장치를 의미 링크는 유선 또는 무선을 의미

## 2.1.1 처리량과 지연 시간

좋은 네트워크란 많은 처리량을 처리할 수 있으며 지연 시간이 짧고 장애 빈도가 적으며 좋은 보안을 갖춘 네트워크

## 처리량

![3.처리량](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/3.처리량.png)

링크 내에서 성공적으로 전달된 데이터의 양을 말함. 얼만큼을 트래픽을 처리했는지.

`많은 트래픽을 처리한다` = `많은 처리량을 가진다`

단위: `bps(bits per second)`, 초당 전송, 수신되는 비트 수

트래픽: 특정 시점에 링크 내에 `흐르는` 데이터 양을 말함

서버에 저장된 파일을 클라이언트가 다운로드할 때 발생되는 데이터의 누적량을 뜻함

- 트래픽이 많아졌다. = 흐르는 데이터가 많아졌다.
- 처리량이 많아졌다. = 처리되는 트래픽이 많아졌다.

## 대역폭

주어진 시간 동안 네트워크 연결을 통해 흐를 수 있는 최대 비트 수

## 지연 시간

요청이 처리되는 시간, 어떤 메시지가 두 장치 사이를 왕복하는데 걸린 시간

![4.지연시간](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/4.지연시간.png)

## 2.1.2 네트워크 토플로지와 병목 현상

### 네트워크 토폴로지

노드와 링크가 어떻게 배치되어있는지에 대한 방식이자 연결 형태를 의미

트리 토플로지는 계층형 토플로지이며 트리 형태로 배치한 네트워크 구성을 말함

![5.네트워크토폴로지](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/5.네트워크토폴로지.png)

노드의 추가, 삭제가 쉽고 특정 노드에 트래픽이 집중될 때 하위 노드에 영향을 끼칠 수 있음

### 버스 토폴로지

중앙 통신 회선 하나에 여러 개의 노드가 연결되어 공유하는 네트워크 구성. 근거리 통신망(LAN)에서 사용함

![6.버스토폴로지](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/6.버스토폴로지.png)

설치 비용이 적고 신뢰성이 우숳며 중앙 통신 회선에 노드를 추가하거나 삭제하기 쉬움. 스푸핑이 가능한 문제점이 있음

스푸핑은 LAN 상에서 송신부의 패킷을 송신과 관련 없는 다른 호스트에 가지 않도록 스위칭 기능을 마비시키거나 속여서 특정 노드에 해당 패킷이 오도록 처리하는 것

![7.스푸핑](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/7.스푸핑.png)

스푸핑을 적용하면 올바르게 수신부로 가야 할 패킷이 악의적인 노드에 전달됨

### 스타 토폴로지

중앙에 있는 노드에 모두 연결된 네트워크 구성

![8.스타토폴로지](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/8.스타토폴로지.png)

노드를 추가하거나 에러를 탐지하기 쉽고 패킷의 충돌 발생 가능성이 적음

노드에 장애가 발생해도 쉽게 에러를 발생할 수 있음. 

장애 노드가 중앙 노드가 아닐 경우 다른 영향에 끼치는 것이 적음

중앙 노드에 장애가 발생하면 전체 네트워크를 사용할 수 없고, 설치비용이 비쌈

### 링형 토폴로지

각각의 노드가 양 옆의 두 노드와 연결하여 전체적으로 고리처럼 하나의 연속된 길을 통해 통신을 하는 망 구성 방식

![9.링형토폴로지.png](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/9.링형토폴로지.png.png)

데이터는 노드에서 노드로 이동, 각각의 노드는 고리 모양의 길을 통해 패킷을 처리

노드 수가 적어도 네트워크상의 손실이 거의 없고 충돌이 발생할 가능성이 적으며 노드의 고장 발견을 쉽게 찾을 수 있음

네트워크 구성 변경이 어렵고 회선에 장애가 발생하면 전체 네트워크 영향을 크게 끼침

### 메시 토폴로지

망형 토플로지라고도 하며 그물망처럼 연결되어 있는 구조

![10.메시토폴로지](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/10.메시토폴로지.png)

한 단말 장치에 발생해도 여러 개의 경로가 존재해 네트워크를 계속 사용할 수 있으며 트래픽도 분산 처리가 가능함

노드의 추가가 어렵고 구축 비용과 운용 비용이 고가인 단점이 존재

### 병목 현상

토폴로지가 중요한 이유는 병목 현상을 찾을 때 중요한 기준이 되기 때문

전체 시스템의 성능이나 용량이 하나의 구성 요소로 인해 제한을 받는 현상

## 2.1.3 네트워크 분류

전 세계 규모의 WAN, 서울시 등 시 정도의 규모인 MAN, 사무실과 개인적으로 소유 가능한 규모의 LAN으로 나뉨

![12.네트워크분류](https://raw.githubusercontent.com/LegendStudy/CS-Study/master/임준형/image/week2/12.네트워크분류.png)

LAN: 사근거리 통신망. 전송 속도가 빠르고 혼잡하지 않음

MAN: 대도시 지역 네트워크를 나타냄. 전송 속도는 평균이며 LAN보다 더 혼잡

WAN: 광역 네트워크를 의미. 전송 속도는 낮으며 MAN보다 더 혼잡

## 2.1.4 네트워크 성능 분석 명령어

네트워크 병목 현상의 주된 원인

~~~
- 네트워크 대역폭
- 네트워크 토폴로지
- 서버 CPU, 메모리 사용량
- 비효율적인 네트워크 구성
~~~

### ping(Packet INternet Groper)

네트워크 상태를 확인하려는 대상 노드를 향해 일정 크기의 패킷을 전송하는 명령어

ping은 TCP/IP 프로토콜 중에 ICMP 프로토콜을 통해 동작

이때문에 ICMP 프로토콜을 지원하지 않는 기기를 대상으로는 실행할 수 없거나 네트워크 정책상 ICMP나 traceroute를 차단하는 대상의 경우 ping 테스팅은 불가능

### netstat

접속되어 있는 서비스들의 네트워크 상태를 표시하는데 사용됨

네트워크 접속, 라우팅 테이블, 네트워크 프로토콜 등 리스트를 보여줌. 서비스의 포트가 열려 있는지 확인할 때 사용

### nslookup

DNS에 관련된 내용을 확인하기 위해 쓰는 명령어

특정 도메인에 매핑된 IP를 확인하기 위해 사용함

### tracert

윈도우에서는 tracert, 리눅스에서는 traceroute

목적지 노드까지 네트워크 경로를 확인할 때 사용함

목적지 노드까지 구간들 중 어느 구간에서 응답 시간이 느려지는지 확인할 수 있음

ftp를 통해 대형 파일을 전송하여 테스팅하거나 tcpdump를 통해 노드로 오고 가는 패킷을 캡처하는 등의 명령어가 있으며 네트웤 ㅡ분석 프로그램에는 wireshark, netmon이 있음

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

IP, ARP, ICMP 등