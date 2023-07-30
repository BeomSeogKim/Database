# OSI 7계층 TCP / IP 4계층

> OSI 7계층, TCP / IP 는 통신 시스템을 나누어 설명하는 개념이다. 
>
> OSI 7계층은 네트워크 통신을 표준화한 모델이다. 
>
> 하지만 실무적으로 이용하기에 복잡하기 때문에 보통 TCP / IP 4계층을 사용한다. 



![image-20230730232918606](/Users/github/TIL/cs/images/Network/TCP:IP.png)

**OSI 7계층, TCP/IP 4계층모델은 하위 계층의 기능을 이용하며, 상위 계층에게 기능을 제공한다.**



## Encapsulation / Decapsulation

#### Encapsulation

> 통신 프로토콜 특성을 포함한 정보를 Header에 포함시켜 하위 계층에 전송하는 방식 

#### Decapsulation 

> Encapsulation의 반대 과정을 통해 원래의 Data를 얻는 방식



![image-20230730234439566](/Users/github/TIL/cs/images/Network/Encapsulation : Decapsulation.png)

[Data를 전송하는 과정 - Encapsulation]

각 계층을 내려가며 사용자가 전하고자 하는 데이터에 Header 정보를 포함시켜 하위 계층에 전달을 한다. 

Encapsulation이 끝나면 최종적으로 물리계층에서 binary 데이터로 변환시켜 전송



[Data를 읽는 과정 - Decapsulation]

각 계층을 올라가며 Header를 제거하고 원본의 Data를 얻는 과정 



