# [CS] Multi Thread

> 하나의 process가 동시에 여러개의 일을 수행할 수 있도록 해주는 것.



thread는 **한 process 내에서 실행되는 동작의 단위** 이다. 

각 thread는 속해있는 process의 Stack 메모리를 제외한 나머지 메모리 영역을 공유할 수있다. 

Multi Thread는 하나의 process가 동시에 여러개의 일을 할 수 있도록 해주는 것이다.

즉 하나의 process에서 여러 작업을 병렬로 처리하기 위해 multi thread를 사용한다. 



## Stack Memory

thread가 함수를 호출하기 위해서는 

인자 전달, Return Address 저장, 함수 내 지역변수 저장 등을 위한 독립적인 stack memory 공간을 필요로 한다.

결과적으로 thread는 process로부터 Stack memory 영역은 독립적으로 할당받고

 Code, Data, Heap 영역은 공유하는 형태를 가진다. 



## PC Register

Multi Thread에서는 각각의 Thread마다 PC register를 가지고 있어야 한다. 

한 process 내에서도 Context Switch가 일어나게 되는데, PC register에 code address 가 저장되어야 실행 할 수 있기 때문



## Process vs Thread

[Process]

* 운영체제로부터 자원을 할당 받는 단위
* 실행파일(program)이 메모리에 적재되어 CPU를 할당받아 실행되는 것.

[Thread]

* process가 할당받은 자원을 이용하는 실행의 단위 
