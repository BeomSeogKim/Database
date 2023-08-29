# DataBase - Test

## Test의 특징 

Test가 지녀야 하는 특징은 다음 두가지다. 

- 테스트는 다른 테스트와 격리해야 한다. 
- 테스트는 반복 실행이 가능해야 한다. 

이러한 특징을 간편하게 사용을 하기 위해서는 @Transactional 을 사용하면된다. 

![Test-1](/Users/github/TIL/spring/images/database/Test-1.png)

테스트 코드에서 @Transactional 이 붙는 경우 서비스 코드에 붙이는 것과 달리 조금 상이하게 동작한다.

1. 테스트 메서드 혹은 테스트 클래스에 @Transactional이 있는 경우 트랜잭션을 시작한다. 
2. 테스트 로직을 시작하기에 앞서 `set autocommit(false)`를 수행한다. 
3. 테스트 로직들을 수행한다. 
   - `insert into...`
   - `select * from ...`
4. `Rollback`처리를 진행한다. 

@Transactional을 사용함으로 인해 Test가 지녀야 하는 특징을 간단하게 지킬 수 있다.

###### [테스트는 다른 테스트와 격리해야 한다]

> Transaction안에서 테스트가 수행 되기 때문에 테스트를 격리시킬 수 있다. 

###### [테스트는 반복 실행이 가능해야 한다]

> 테스트가 끝날 때 마다 @Rollback을 해주기 때문에, 반복 실행이 가능하다. 



## EmbeddedMode DataBase

> 테스트를 할 때 테스트 전용 Database를 설치하고 운영하는 방식은 비용적으로 매우 비효율적인 방식이다. 
>
> 이를 해결하기 위해서 Embedded DataBase를 사용할 수 있다. 

### H2

> H2 데이터베이스의 경우 자바로 개발되어 있다. 
>
> H2는 JVM 안에서 메모리 모드로 동작하는 특별한 기능을 제공한다. 

### schema.sql

> JPA를 사용하는 경우 ddl-auto 설정을 통해 스키마를 간단히 생성할 수 있지만 보통은 데이터 베이스에 관련 Table들을 설계해 주어야 한다. Spring Boot의 경우 test/reosurces 경로 하위에 `schema.sql`이 있으면 해당 정보를 통해 Table을 생성해준다. 

### DataSource Bean으로 등록 

> DataSource를 직접 빈으로 등록해서 사용할 수 있다. 
>
> ```java
> @Bean
> @Profile("test")
> public DataSource dataSource(){
>   DriverManagerDataSource dataSource = new DriverManagerDataSource();
>   dataSource.setDriverClassName("org.h2.Driver");
>   dataSource.setUrl("jdbc:h2:mem:db;DB_CLOSE_DELAY=-1");
>   dataSource.setUsername("sa");
>   dataSource.setPassword("");
>   return dataSource
> }
> ```

### Spring Boot 

> Spring Boot를 사용하는 경우 기본적으로 DataSource 관련한 설정정보가 아무것도 없다면 임베디드 모드로 접근하는 DataSource를 제공해 준다. 



