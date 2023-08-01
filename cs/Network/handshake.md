# 3 way handshake & 4 way handshake

TCP 프로토콜의 데이터 전송 절차는 다음과 같다. 

1. Connection setup
2. Data transfer
3. Connection termination

3 way handshake는 Connection setup 단계에서 일어나며, 4 way handshake는 Connection termination 단계에서 일어난다. 



## 3 way handshake

![image-20230801181150681](/Users/github/TIL/cs/images/Network/3-way-handshake.png)

1. Client가 Server에 접속을 요청하는 SYN  패킷 전송 
2. Server에서는 요청을 수락하는 ACK 패킷과 SYN 패킷을 전송 
3. Client가 ACK 패킷을 발송하면 연결이 수립되고 데이터 전송 준비 완료 



## 4 way handshake

![image-20230801182348051](/Users/github/TIL/cs/images/Network/4-way-handshake.png)

1. Client process에서 active close 시에, Client TCP에서 FIN 패킷 전송 
2. Server 에서 요청을 받음에 대한 ACK 패킷 전송 (실제 서버 프로세스는 완전히 꺼지지 않았을 수도 있다.)
3. Server의 process가 끝나 passive close를 받으면 Server TCP에서 Client에게 FIN 패킷 전송 
4. Server tcp가 ACK를 받으면 연결 종료 