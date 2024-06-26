### 3-Way HandSwake
​
Tcp 에서는 신뢰성을 확보하기 위해 3-Way HandSwake라는 작업을 진행한다.
​
![image](https://github.com/LegendStudy/CS-Study/assets/96263955/c1f07ef1-506c-4f73-9a22-908ec08b922c)

​
그림과 같이 3단계를 거친다고 해서 3-Way HandSwake라고 하는데 SYN , SYN + ACK, ACK 단계로 나뉜다.
​
SYN 단계에서는 클라이언트의 ISN을 서버에 보낸다. 이때 ISN은 첫번째 패킷에 담긴 임의의 시퀀스 번호이다.
​
SYN + ACK 단계에서는 서버가 클라이언트의 SYN을 수신하고 서버의 ISN을 보내며 승인번호로 클라이언트의 ISN+1 값을 보낸다.
​
ACK단계는 클라이언트가 서버의 승인번호를 받고 승인번호+1 값을 승인번호로 서버에 보내고 서버가 성공적으로 수신하면 완료된다.
​
이처럼 3-wWay HandSwake 과정으로 신뢰성이 구축되고 데이터 전송을 시작한다. TCP는 이 과정이 있기 때문에 신뢰성 있는 계층이라 하며 UDP는 이 과정이 없기 때문에 신뢰성이 없는 계층이라고 한다.
​
### 4-Way HandSwake
​
3-Way HandSwake로 수립한 연결을 해제할 때 4-Way HandSwake 과정을 거친다.
​
![image](https://github.com/LegendStudy/CS-Study/assets/96263955/591d6279-7731-41b4-aa51-28c9b5a887da)
​
위 그림이 4-Way HandSwake 과정인데 1번으로 클라이언트가 연결을 닫으려고 할 때 FIN 으로 설정된 세그먼트를 서버에게 보낸다.
​
그리고 클라이언트는 FIN\_WAIT\_1 상태에 들어가고 서버의 응답을 기다린다. 서버는 FIN을 수신하고 클라이언트에게 ACK라는 승인 세그먼트를 보내고 COLSE\_WAIT 상태에 들어가고 클라이언트가 ACK를 수신하면 FIN\_WAIT\_2 상태에 들어간다. 그리고 서버는 일정시간 후에 클라이언트에게 FIN이라는 세그먼트를 보내고 클라이언트가 수신하면 TIME\_WAIT 상태에 들어가며 서버에게 ACK를 보내고 서버는 CLOSED 상태가 된다. 이후 클라이언트는 일정 시간이 지난후 CLOSED 되며 서버의 모든 자원의 연결이 해제되는 방식이다.
​
여기서 주목해야할 것은 왜 굳이 연결을 바로 CLOSED 하지않고 TIME\_WAIT을 하는지다. TIME\_WAIT 상태에서는 클라이언트가 해당 포트를 열고 기다린다. 그 이유는 네트워크 지연이나 패킷 순서 문제등으로 서버가 마지막 ACK를 제대로 받지 못했을 경우를 대비한것으로 서버가 받지 못했다면 서버는 다시 FIN을보내고 클라이언트는 다시 ACK를 보내 연결을 확실하게 종료할 수 있기 때문이다.
