# TCP vs UDP 

#### TCP / IP 전송계층

TCP / IP는 총 5개의 Layer로 구성됨 

- Application Layer
- Transport Layer
- Network Layer
- Data Link Layer
- Physical Layer

Transport Layer는 두개의 응용 계층 사이에서의 `process-to-process`통신을 제공. 

Transport Layer의 주된 프로토콜은 TCP, UDP이다. 



Segment : TCP로 전송하는 패킷

Datagram : UDP로 전송하는 패킷



## TCP 

> 연결형, 신뢰성 전송 프로토콜

TCP 프로토콜은 연결 지향적 서비스를 제공하기 위해 데이터 전송 전 두 호스트 전송 계층 사이에 `논리적 연결`을 수립한다. 

그 후 데이터를 전송한 후 데이터 전송을 완료했으면 논리적 연결을 해제한다. 

##### 오류제어, 혼잡 제어, 흐름 제어

[흐름 제어]

=> 데이터를 보내는 속도와 데이터를 받는 속도의 균형을 맞춤

[오류 제어]

=> 훼손된 segment의 감지 및 재전송, 손실된 segment 재전송, 순서가 맞지 않게 도착한 segment 재 정렬, 중복 segment 감지 및 폐기 



이러한 제어들을 통해 TCP는 데이터들이 결점없이 전송 되는 것을 보장한다. 



## UDP 

> 비연결형, 비신뢰성 전송 프로토콜

UDP 프로토콜은 TCP 프로토콜과 달리 논리적 연결을 수립하지 않고, 각종 오류 제어들을 하지 않는다. 

단순 기능을 제공하기 때문에 TCP에 비해서 비교적 작은 header를 가지며, 전송 속도가 빠르다는 특징을 지닌다. 





