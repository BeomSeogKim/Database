# Spring Transaction

### 트랜잭션 우선순위 

트랜잭션은 **구체적**이고 **자세한** 것이 높은 우선 순위를 가진다. 

트랜잭션의 적용 범위는 총 네가지로 다음과 같다.

- 클래스의 메서드
- 클래스의 타입
- 인터페이스의 메서드 
- 인터페이스의 타입

클래스의 메서드가 가장 높은 우선 순위를 가지며, 인터페이스의 타입이 가장 낮은 우선 순위를 가진다. 

***스프링에서는 인터페이스에 @Transactional을 선언하는 것을 권장 하지는 않는다.***

> 스프링 5.0 이전의 경우 CGLIB 방식의 프록시 생성(구체 클래스를 기반으로 프록시를 생성)하는 경우  인터페이스에 있는 @Transactional을 인식하지 못했다. 스프링 5.0부터는 이부분을 개선해 해결이 되었지만 추후 다른 AOP방식에서는 또 적용이 되지 않을 위험이 있기 때문에 권장하는 방식은 아니다. 

### 프록시 내부 호출 

해당 부분의 경우 자칫 실수를 한다면 트랜잭션이 적용되지 않는 문제가 발생한다. 

[코드와 함께 설명]

[@Transactional 을 통한 내부 호출]

```java
@Slf4j
static class TestClass {

    @Transactional
    void callTx() {
      log.info("callTx");
      printTxInfo();
      callNonTx();
    }

    void callNonTx() {
        log.info("callNonTx");
        printTxInfo();
    }

    private void printTxInfo() {
        boolean txActive = TransactionSynchronizationManager.isActualTransactionActive();
        log.info("tx active = {}", txActive);
    }
}

@Test
void proxyTest() {
    testClass.callTx();
}
```

> callTx를 할 때 프록시를 호출한다. 
>
> 그렇기 때문에 callTx() 에서 트랜잭션이 적용되며 내부 호출인 callNonTx() 도 트랜잭션이 적용된다. 

```shell
callTx
tx active = true
callNonTx
tx active = true
```



[@Transactional이 적용되지 않은 메서드를 통한 내부 호출]

```java
@Slf4j
static class TestClass {

    @Transactional
    void callTx() {
      log.info("callTx");
      printTxInfo();
    }

    void callNonTx() {
        log.info("callNonTx");
        printTxInfo();
        callTx();
    }

    private void printTxInfo() {
        boolean txActive = TransactionSynchronizationManager.isActualTransactionActive();
        log.info("tx active = {}", txActive);
    }
}
```

> callNonTx의 경우 @Transactional이 적용되어 있지 않기 때문에 트랜잭션이 적용되지 않는다. 
>
> callNonTx에서 callTx()를 호출 할 경우 프록시를 통한 호출이 아닌 **내부 호출**(this.callTx())가 발생한다. 
>
> 그에 따라서 트랜잭션이 적용되지 않는다. <= 해당 부분을 조심해야 한다. 

```shel
callNonTx
tx active = false
callTx
tx active = false
```



### 스프링 AOP 적용시점 

> 보통은 어플리케이션이 실행되면서 어떠한 행동을 취하기를 기대할 때 @PostConstruct를 자주 사용한다. 
>
> 하지만 @Transactional 과 같은 AOP는 유의해서 사용해야 한다. 

Spring의 AOP는 초기화 코드가 먼저 호출되고, 그 다음에 AOP가 적용된다. 

하지만 @PostConstruct는 초기화 시점에 호출이 되기 때문에 AOP가 적용이 되지 않는다. 

=> @EventListener(value=ApplicationEvent.class) 를 사용하면 스프링 컨테이너가 완전히 생성되고 난 다음에 메서드 호출이 일어난다.  그렇기 대문에 트랜잭션이 적용된다. 



### Spring - Commit / Rollback

스프링은 예외에 대해서 기본적으로 다음과 같은 가정을 한다.

- Checked Exception : 비즈니스 의미가 있을 때
- Unchecked Exception : 복구 불가능한 예외

그렇기 때문에  Unchecked Exception은 Rollback을 진행하고, Checked Exception은 Commit을 진행한다. 

##### 비즈니스 의미가 있다.

> 시스템 예외가 아닌 유저의 문제로 인해 발생하는 일들을 의미한다. 
>
> 예를들어 결제를 하는 비즈니스 로직이 있다고 가정하자. 
>
> 클라이언트의 계좌에 잔고가 없어서 결제가 실패하는 경우는 비즈니스적인 문제이지, 시스템적 예외가 아니다. 
>
> 이런 경우를 **비즈니스 의미가 있다** 라고 한다.

이 외에 Checked Exception이 발생 할 때에도 rollback을 하도록 하려면 rollbackFor 옵션을 사용하면 된다. 









