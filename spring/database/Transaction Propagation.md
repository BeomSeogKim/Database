# 트랜잭션 전파 (propagation)

> 트랜잭션 전파란 트랜잭션을 어떻게 동작할 지 결정하는 메카니즘이다. 

트랜잭션은 물리 트랜잭션과 논리 트랜잭션으로 나뉜다. 

**[물리 트랜잭션]**

실제로 데이터 베이스에 적용되는 트랜잭션

**[논리 트랜잭션]**

트랜잭션 매니저를 통해 트랜잭션을 사용하는 단위. 두개 이상의 트랜잭션이 관리 될 때 개념적으로 사용된다. 

논리 트랜잭션은 데이터 베이스에 적용되는 트랜잭션은 아니다. 



트랜잭션 전파의 대원칙은 다음과 같다.

- 모든 논리 트랜잭션이 커밋 되어야 물리 트랜잭션이 커밋된다.
- 논리 트랜잭션 중 하나라도 롤백된다면 물리 트랜잭션은 롤백된다. 



## 상황 별 트랜잭션 살펴보기

트랜잭션의 기본 전파 옵션은 `REQUIRES`이다. 

> `REQUIRES` : 기존 트랜잭션이 있다면 트랜잭션에 참여하고, 트랜잭션이 없다면 트랜잭션을 생성한다. 

[외부 트랜잭션 커밋, 내부 트랜잭션 커밋]

```java
log.info("외부 트랜잭션 시작");
TransactionStatus outer = platformTransactionManager.getTransaction(new DefaultTransactionAttribute());
log.info("outer.isNewTransaction()={}", outer.isNewTransaction());

log.info("내부 트랜잭션 시작");
TransactionStatus inner = platformTransactionManager.getTransaction(new DefaultTransactionAttribute());
log.info("inner.isNewTransaction()={}", inner.isNewTransaction());
log.info("내부 트랜잭션 커밋");
platformTransactionManager.commit(inner);

log.info("외부 트랜잭션 커밋");
platformTransactionManager.commit(outer);
```

이 경우에는 두가지 논리 트랜잭션이 커밋었기 때문에 물리 트랜잭션이 커밋된다. 

[외부 트랜잭션 롤백, 내부 트랜잭션 커밋]

```java
log.info("외부 트랜잭션 시작");
TransactionStatus outer = platformTransactionManager.getTransaction(new DefaultTransactionAttribute());
log.info("outer.isNewTransaction()={}", outer.isNewTransaction());

log.info("내부 트랜잭션 시작");
TransactionStatus inner = platformTransactionManager.getTransaction(new DefaultTransactionAttribute());
log.info("inner.isNewTransaction()={}", inner.isNewTransaction());
log.info("내부 트랜잭션 커밋");
platformTransactionManager.commit(inner);

log.info("외부 트랜잭션 롤백");
platformTransactionManager.rollback(outer);
```

내부 트랜잭션은 커밋을 요청했지만 외부 요청은 롤백을 요청했다. 

물리 트랜잭션은 하나의 커밋이라도 롤백이 된다면 롤백을 시키므로, 두가지 요청 모두 롤백 처리가 된다. 

[외부 트랜잭션 커밋, 내부 트랜잭션 롤백]

```java
log.info("외부 트랜잭션 시작");
  TransactionStatus outer = platformTransactionManager.getTransaction(new DefaultTransactionAttribute());
  log.info("outer.isNewTransaction()={}", outer.isNewTransaction());

  log.info("내부 트랜잭션 시작");
  TransactionStatus inner = platformTransactionManager.getTransaction(new DefaultTransactionAttribute());
  log.info("inner.isNewTransaction()={}", inner.isNewTransaction());
  log.info("내부 트랜잭션 롤백");
  platformTransactionManager.rollback(inner);

  log.info("외부 트랜잭션 커밋");
  assertThatThrownBy(
          () -> platformTransactionManager.commit(outer)
  ).isInstanceOf(UnexpectedRollbackException.class);
```

우선적으로 내부 트랜잭션이 롤백 요청을 했기 때문에, 물리 트랜잭션은 롤백된다. 

외부 트랜잭션의 경우 커밋 요청을 했지만, 실제로는 롤백이 되는 상황이 발생한다. 

그렇기 때문에 `UnexpectedRollbackException`이 발생한다. 



## 트랜잭션의 다양한 옵션

트랜잭션 전파와 관련하여 다양한 옵션이 존재한다. 

###### REQUIRED 

- 기존 트랜잭션이 있는 경우 : 트랜잭션에 참여
- 기존 트랜잭션이 없는 경우 : 트랜잭션 생성

###### REQUIRES_NEW

- 모든 경우에 대해 새로운 트랜잭션 생성

###### SUPPORT

- 기존 트랜잭션이 없는 경우 : 트랜잭션 없이 진행 
- 기존 트랜잭션이 있는 경우 : 트랜잭션에 참여

###### NOT_SUPPORT

- 모든 경우에 대해 트랜잭션에 참여하지 않는다. 

###### MANDATORY

- 기존 트랜잭션이 없는 경우 : `IllegalTransactionStateException` 발생
- 기존 트랜잭션이 있는 경우 : 기존 트랜잭션에 참여 

###### NESTED

- 기존 트랜잭션이 없는 경우 : 새로운 트랜잭션을 생성 
- 기존 트랜잭션이 있는 경우 : 중첩 트랜잭션을 생성 
  - 중첩 트랜잭션은 외부 트랜잭션의 영향을 받지만 외부에 영향을 주지는 않는다. 











