# [CS] MultiProcess vs Multi Thread

> Multi thread는 multi process보다 적은 메모리 공간을 차지하고 Context Switching 이 빠르다.
>
> MultiProcess는 하나의 process가 죽더라도 다른 process에 영향을 주지않아 안정성이 높다.



두 방식은 동시에 여러 작업을 한다는 측면에서 유사하다. 

|               | 메모리사용 / CPU 시간       | Context Switching | 안정성 |
| ------------- | --------------------------- | ----------------- | ------ |
| multi process | 많은 메모리 공간 / CPU 시간 | 느리다            | 높다   |
| multi thread  | 적은 메모리 공간 / CPU 시간 | 빠르다            | 낮다   |

multi thread를 구현할 경우 메모리 공간과 시스템 자원 소모가 줄어든다. 

하지만 thread 간에 자원을 공유하기 때문에 동기화 문제가 발생할 수 있기 때문에 이를 고려한 프로그램 설계가 필요하다. 



[multi process가 유리한 경우]

* 메모리 구분이 필요할 때 
* 안정성이 요구 될 때

[multi thread가 유리한 경우]

* Context Switching이 자주 일어날 때 
* 자원을 효율적으로 사용해야 할 때 