# Spring Transaction

## Transaction Manager

> 데이터 접근 기술마다 Transaction 관련 구현해야하는 코드 방식들이 다르다. 스프링에서는 이를 공통 처리하기 위해 Transaction Manager Interface를 제공한다. 

![SpringTransaction-1](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/SpringTransaction-1.png)

데이터 접근 기술마다 트랜잭션을 사용하는 방법이 다르다.

- JDBC :  `connection.setAutoCommit(false)`
- JPA : `trasaction.begin()`

이러한 부분들을 공통화 하기 위해 `PlatformTransactionManager` 인터페이스가 존재한다. 

트랜잭션 매니저는 크게 두가지 역할을 한다. 

1. 트랜잭션 추상화
2. 리소스 동기화 

[트랜잭션 추상화]

앞서 살펴본 것 처럼 다른 데이터 접근 기술에 상관없이 트랜잭션 관련 기술을 사용하기 위해 추상화 한 것을 말한다. 



[리소스 동기화]

트랜잭션을 유지하기 위해서 트랜잭션이 시작한 순간부터 종료되는 순간까지 동일한 트랜잭션을 유지해야한다. 

트랜잭션 매니저는 `트랜잭션 동기화 매니저`를 통해 이를 보장해준다. 

### 트랜잭션 동기화 매니저

[Code]

```java
public abstract class TransactionSynchronizationManager {

	private static final ThreadLocal<Map<Object, Object>> resources =
			new NamedThreadLocal<>("Transactional resources");

	private static final ThreadLocal<Set<TransactionSynchronization>> synchronizations =
			new NamedThreadLocal<>("Transaction synchronizations");

	private static final ThreadLocal<String> currentTransactionName =
			new NamedThreadLocal<>("Current transaction name");
//   ... 
```

트랜잭션 동기화 매니저는 내부적으로 ThreadLocal을 사용해 커넥션을 저장하고 있다.

나중에 데이터 접근 관련 계층 (Repository)영역에서 커넥션을 사용해야할 경우 ThreadLocal에서 커넥션을 가져와 사용하면 된다. 



### 트랜잭션 매니저의 동작 과정 

![SpringTransaction-2](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/SpringTransaction-2.png)

1. 트랜잭션 호출
   - `transactionManager.getTransaction()`를 호출해 트랜잭션을 시작한다
2. 커넥션 생성 
   - DataSource를 통해 커넥션을 생성 / 획득한다. 
3. 수동커밋 변경
   - `setAutoCommit(false)`를 하여 수동 커밋모드로 변경
4. 커넥션 보관 
   - 트랜잭션 동기화 매니저에 보관한다. 
5. 데이터 접근 로직 호출
   - 비즈니스 로직에서 데이터 접근 로직을 호출
6. 동기화된 커넥션 조회 
   - `DataSourceUtils.getConnection()`을 사용해 동기화 매니저에 존재하는 커넥션을 획득
7. SQL 실행 
   - 획득한 커넥션을 사용해 SQL을 데이터베이스에 전달해 실행 
8. commit() / rollback()호출
   -  commit() : 정상적으로 비즈니스 로직이 완료된 경우
   - rollback() : 비즈니스 로직 도중 문제가 발생한 경우 
9. 커넥션 획득 
   - 동기화 매니저에 보관되어있는 커넥션을 획득한다. 
10. commit() / rollback()
    - commit() : TransactionManager가 commit()을 호출한 경우 
    - rollback() : TransactionManager가 rollback()을 호출한 경우  
11. 전체 리소스 정리 
    - setAutoCommit(true)
    - 커넥션 반환 
      - Connection Pool 을 사용하는 경우 : Connection Pool에 반환
      - 그 외 : Connection 종료



## AOP를 활용한 트랜잭션 사용

### 선언적 트랜잭션 관리 vs 프로그래밍 방식 트랜잭션 관리 

트랜잭션을 관리함에 있어 크게 두가지 방식의 트랜잭션 관리로 나뉜다. 

각각의 특징은 다음과 같다. 

##### 선언적 트랜잭션 관리 

- @Transactional (과거의 경우 XML)을 선언함으로써 편리하게 트랜잭션을 적용하는 방식
- 사용법이 간단하지만, 독립적인 테스트를 구현하기에는 조금 어렵다.

##### 프로그래밍 방식 트랜잭션 관리

- 트랜잭션 매니저 혹은 트랜잭션 템플릿을 사용해 트랜잭션을 관리하는 방식
- 스프링의 컨테이너 혹은 스프링의 AOP 기술 없이 간단하게 사용을 할 수 있다. 



### AOP를 활용한 트랜잭션 사용 방법

```java
@Transactional -- 1
public class MemberService {

    private final MemberRepository memberRepository;

    public MemberServiceV3_3( MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Transactional -- 2
    public void accountTransfer(String fromId, String toId, int money) throws SQLException {
        businessLogic(fromId, toId, money);
    }
```

@Transactional은 클래스 그리고 메서드에 붙일 수 있다. 

클래스에 @Transactional 을 적용한 경우 

> 클래스에 명시되어 있는 public 메서드에 AOP가 적용된다. 

메서드에 @Transactional 을 적용한 경우 

> 명시한 메서드에 AOP가 적용된다. 
