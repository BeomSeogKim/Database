# Cache

## Cache란 무엇인가? 

캐시는 값비싼 연산 결과 혹은 자주 참조되는 데이터를 메모리 안에 두고, 뒤이은 요청이 보다 빠르게 처리될 수 있도록 하는 저장소

## OpenSource

- Caffeine : 메모리 기반의 고성능 캐시로, 자바 8이상에서 사용 가능
- EhCache : 자바 프로세스 내부 혹은 분산 환경에서 사용할 수 있는 캐시.
- Hazelcast : 분산 컴퓨팅을 위한 인메모리 데이터 그리드.
- Redis : 네트워크를 통해 접근하는 key-value 저장소로, 데이터 구조 서버 역할
- Guava : 구글이 제공하는 캐시 라이브러리.

*Caffeine : Java8 이후에 나온 라이브러리로 Guava 캐시의 후속작이며, 빠른 성능과 유연한 API로 인해 인기가 많다.*

오픈 소스중 Caffeine이 읽기 및 쓰기 성능이 우세하다. [관련 링크](https://github.com/ben-manes/caffeine/wiki/Benchmarks)

---

## Caffeine

### Introduction

Caffeine은 Window TinyLfu의 eviction 정책을 사용한다. 

- Size Based 
  - 구성된 캐시 크기 제한을 초과하면 제거가 발생. 캐시는 최근에 또는 자주 사용되지 않은 항목을 제거하려고 시도한다.
- Time Based 
  - Expire After Access : 마지막 읽기 또는 쓰기가 발생한 이후 지정된 시간이 지나면 항목이 만료됨.
  - Expire After Write : 쓰기가 발생한 이후 지정된 기간이 지나면 항목이 만료됨.
  - Expire After : Expiry 구현을 기반으로 각 항목에 대한 사용자 정의 만료.
- Reference Based
  - 키나 값에 대한 약한 참조와 값에 대한 소프트 참조를 사용해 항목의 가비지 수집을 허용하도록 설정할 수 있음.

### Dependency (Gradle)

```gradle
// spring 에서 cache 사용
implementation 'org.springframework.boot:spring-boot-starter-cache'
// caffeine 관련
implementation 'com.github.ben-manes.caffeine:caffeine'
```

### CaffeinCache 관련 설정 

```java
@EnableCaching
@Configuration
public class CacheConfig {

    @Bean
    public Caffeine<Object, Object> caffeineConfig() {
        return Caffeine.newBuilder()
            .expireAfterWrite(300, TimeUnit.SECONDS)	// 만료 시간 설정
            .initialCapacity(10);	// 캐시의 초기 사이즈를 10으로 설정. 
    }

    @Bean
    public CacheManager cacheManager(Caffeine caffeine) {
        CaffeineCacheManager caffeineCacheManager = new CaffeineCacheManager();
        caffeineCacheManager.setCaffeine(caffeine);	// 캐시 구성 주입
        return caffeineCacheManager;
    }
}
```

- @EnableCaching => 해당 어노테이션이 있는 경우 Spring Boot는 Cache Infra 제공
- 해당 설정을 통해 애플리케이션 시작 시 스프링이 CacheManager를 생성하고, CacheManager는 Caffeine 인스턴스를 사용해 캐시를 관리한다.
- CacheManager는 @Cacheable, @CachePut, @CacheEvict 등의 캐시 관련 어노테이션이 사용된 메서드에 적용된다.

```properties
spring.cache.cache-names=MemberCache
spring.cache.caffeine.spec=initialCapacity=50,maximumSize=10,expireAfterAccess=300s
```

application.properties 에서 캐시 구성을 정의할 수 있다. 

cache-names의 경우 저장하고자 하는 캐시 value를 의미함.

***ExpireAfterWrite, ExpireAfterAccess가 공존하는 경우 ExpireAfterWrite가 우선권을 가진다.***

### @Cacheable

메서드에 대해 캐싱을 활성화 할 수 있는 어노테이션.

```java
@Cacheable(value = "MemberCache", unless = "#result == null", key = "{#id}")
public Member getMemberCache(Long id) {
    return memberRepository.findById(id)
        .orElseThrow(() -> new EntityNotFoundException("해당 회원을 찾을 수 없습니다."));
}
```

### 사용자 정의 캐시 키 이름 생성 

> 기본적으로 Spring Data는 params 메서드를 사용해 캐시 키를 계산한다. 
> 계산된 키에 대한 캐시에 값이 없으면 대상 메서드가 호출되고 반환된 값은 연결된 캐시에 저장된다. 

#### SpEL 사용

```java
@Cacheable(value = "StudentCache", key="{#surname}")
public int getId(String name, String surname) {
	return studentDAO.fetchId(name, surname);
}
```

key에 다음과 같이 지정을 하면 parameter 중 surname을 기반으로 캐시 키를 만든다. 

#### 사용자 정의 Key Generator 사용

```java
public class SurnameKeyGenerator implements KeyGenerator {

	public Object generate(Object target, Method method, Object... params) {
		return params[1];
	}
}
---
@Bean("surnameKeyGenerator")
public KeyGenerator keyGenerator() {
	return new SurnameKeyGenerator();
}
---
@Cacheable(value = "StudentCache", keyGenerator = "surnameKeyGenerator")
public int getId(String name, String surname) {
	return studentDAO.fetchId(name, surname);
}
```

Custom 한 Key Generator르 사용하고자 한다면 직접 Generator를 생성하고 빈으로 등록하면 된다, 

그 후 사용하고자 하는 곳에 keyGenerator로 명시해 주면된다. 

### 조건부 캐싱

> condition, unless 혹은 application.properties를 통해 조건부 캐시를 가질 수 있다.

#### condition

```java
@Cacheable(value = "StudentCache", condition = "#surname != 'abc'")
public int getId(String name, String surname) {
	return studentDAO.fetchId(name, surname);
}
```

해당 경우는 surname이 abc가 아닌 경우에만 캐시한다. 

#### unless 

```java
@Cacheable(value = "StudentCache", unless = "#surname == 'abc'")
public int getId(String name, String surname) {
	return studentDAO.fetchId(name, surname);
}
```

해당 경우는 surname이 abc가 아니면 캐시한다. 



### @Cacheable과 @CachePut의 차이점 

@Cacheable의 경우 지정된 캐시 키에 대해 한번만 실행되며, 후속 요청은 캐시가 만료되거나 플러시 될때 까지 해당 메서드를 실행하지 않는다. 반면 @CachePut의 경우 condition or unless 조건에 일치된다면 무조건적으로 실행한다.





### Cache 구축시 고려 사항 

- expriation 정책

- eviction 정책

- DB와 캐시 데이터와 일관성 유지

- 장애 대응

- 캐시 메모리 크기 설정

- 낮은 cache miss rate 유지하기



---

참고하면 좋을만한 자료 

- [HOWTODOINJAVA](https://howtodoinjava.com/spring-boot/spring-boot-caffeine-cache/)
- [dev.to - ](https://dev.to/noelopez/spring-cache-speed-up-your-app-1gf6)
- [dev.to - 2](https://dev.to/noelopez/spring-cache-with-caffeine-384l)



