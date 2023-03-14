# [CS] multi process 환경에서 process간 데이터통신

> 프로세스간 통식 방법으로 파이프, 소켓,공유메모리 등을 이용한 방법이 있다.

원칙적으로는 process는 독립적인 주소 공간을 갖기 때문에, 다른 process의 주소공간을 참조할 수 없다.

하지만 경우에 따라 운영체제는 process 간 자원 접근을 위한 메커니즘인 IPC(Inter Process Communication)를 제공한다.

프로세스 간 통신(IPC) 방법으로는 파이프, 파일, 소켓, 공유메모리 등을 이용한 방법이 있다. 

이 중 공유메모리(shared memory)와 메시지 전달(message passing)의 두가지 모델에 대해 알아보려고 한다.



## 공유메모리(shared memory)

- 공유 메모리 방식의 경우 process 들이 주소 공간의 일부를 공유한다. 

- 공유한 메모리 영역에 읽기 / 쓰기를 통해 통신을 수행한다. 

- process가 공유 메모리 할당을 kernel에 요청하면 kernel은 해당 process에 메모리 공간을 할당해 준다. 

- 공유메모리 영역이 구축 된 이후에는 모든 접근이 일반적인 메모리 접근으로 취급된다.
  - 더이상 kernel 도움 없이 process 들이 해당 메모리 영역에 접근할 수 있다. 
  - **kernel의 관여 없이 데이터를 통신 할 수 있기 때문에 IPC 속도가 빠르다.**
- 동시에 같은 메모리 위치에 접근하게 되면 일관성 문제가 발생할 수 있다. 
  - 이에 대해서 process들 끼리 직접 공유 메모리 접근에 대한 동기화 문제를 책임져야한다.
- 예시로는 공유메모리, POSIX가 있다.



## 메세지 전달(message passsing)

* 메세지 전달 방식의 경우 system call을 사용하여 구현된다.
* kernel을 통해 send(message)와 recevive(message) 라른 두가지 연산을 제공 받는다. 
* 공유 메모리보다는 속도가 느리다
* 충돌을 회피할 필요가 없기 때문에 적은양의 데이터를 굥환하는데 유용하다
* 구현이 쉽다.
* 예시로는 Pipe, socket, message queue 등이 있다.

