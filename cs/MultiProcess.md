# [CS] Multi Process

> Multi Process란 2개 이상의 process 가 동시에 실행되는 것을 말한다. 
>
> 여기서 동시는 동시성과 병렬성 두가지를 의미한다.

## 동시성(Concurrency) & 병렬성(Parallelism)

### [동시성]

Single Core일 때 여러 Process를 짧은 시간동안 번갈아 가면서 연산을 하게 되는 시분할 시스템(time sharing system)

으로 실행 되는 것.

*동시에 실행 되는 것 같아 보인다*

### [병렬성]

Multi Core 일 때 각각의 core가 각각의 process를 연산함으로써 process가 동시에 실행 되는 것.

*실제로 동시에 여러 작업이 처리된다*



***해당 글에서는 Single Core의 관점으로 Multi Process 내용을 다룹니다.***



### [메모리 관리]

여러 process 들이 동시에 memory에 적재되는 경우, 서로 다른 process 영역을 침범하지 않도록 

`각 process가 자신의 memory 영역에만 접근하도록 운영체제가 관리해준다.`



### [PC Register]

CPU는 PC register가 가르키고 있는 명령어를 읽어들여 연산을 진행한다. 여러 process 들이 동시에 실행 될 경우 

**실행중인 process의 code 영역을 PC register가 가르키게 된다. **

r예를 들어 process1 , process2 가 있다고 가정하면 

PC Register at process1 =>  PC Register at process2 => PC Register at process1 => PC Register at process2

와 같은 방식으로 PC Register가 움직여 다닌다. 



### [Context]

**process가 현재 어떠한 상태로 수행되고 있는지에 대한 총체적인 정보를 말한다.  **

Multi Process에서 하나의 process는 짧은 시간동안 CPU를 점유해 일정 부분의 명령을 수행하고 다른 process에게 제어권을  넘긴다. 후에 다시 차례가 되면 CPU를 점유해 명령을 수행한다. 따라서 이전에 어디까지 수행 했는지, register에는 어떠한 값이 저장되어 있는지에 대한 정보가 필요한데 이 정보를 context라고 한다. 



### [PCB(Process Control Block)]

PCB는 운영체제가 프로세스를 표현한 자료구조이다.  

PCB에는 프로세스의 중요한 정보가 담겨 있기 때문에 일반적으로 보호된 메모리 영역 안에 저장된다.

PCB에는 다음과 같은 정보들이 포함된다. 

| PCB             | 설명                                                     |
| --------------- | -------------------------------------------------------- |
| Process State   | new, running, waiting, halted 등의 state 존재            |
| Process Number  | 해당 process 의 number                                   |
| Program Counter | 해당 process가 다음에 실행할 명령어의 주소를 가르킴      |
| Registers       | 컴퓨터 구조에 따라 다양한 수와 유형을 가진 register 값들 |
| Memory Limits   | base register, limit register 등                         |
| ...             |                                                          |



### [Context Switch]

기존에 실행되던 프로세스에서 다른 프로세스로 CPU 제어권을 넘겨주는 것을 의미한다.

이 때, 기존에 실행되던 프로세스의 상태를 PCB에 저장하고 다른 프로세스의 PCB를 읽어 보관된 상태를 복구하는 작업이 이루어진다.



