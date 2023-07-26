# Query Method



Spring Data JPA은 3가지의 Query Method를 지원

- 메서드 이름으로 쿼리 생성 
- JPA Named Query
- @Query 어노테이션



---

Member Entity가 다음의 속성을 가진다고 가정

```java
@Entity
public class Member {
    @Id
    @GeneratedValue
    private Long id;

    private String username;
    private int age;
}
```



## 메서드 이름으로 쿼리 생성

```java
public interface MemberRepostiroy extends JpaRepository<Member, Long> {

    List<Member> findAllByUsernameAndAgeGreaterThan(String username, int age);
  
}
```

메서드 이름으로 쿼리를 생성하는 경우 Member Class가 가진 속성 이름과 동일하게 작성을 해주어야 한다. 

메서드 작성 법은 공식 문서를 참고 : [공식문서 바로가기](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)



## JPA Named Query

```java
@Entity
@NamedQuery(name = "Member.findByUsername",
        query = "select m from Member m where m.username = :username")
public class Member {
    @Id
    @GeneratedValue
    private Long id;

    private String username;

    private int age;
}
```

Named Query의 경우 Entity에 @NamedQuery를 사용해 적용

`name`에는 사용하고자 하는 이름을 명시해주며 `query`에는 실행할 query를 작성해준다.

```java
@Query(name = "Member.findByUsername")
List<Member> findByUsername(@Param("username") String username);
```

Spring Data JPA에서는  @Query에 name에 생성한 이름을 적어주면 된다. 



## @Query

```java
@Query("select m from Member m where m.age > :age")
List<Member> findAllAgeGreaterThan(@Param("age") int age);
```

Spring Data JPA에서 실행할 쿼리를 직접 명시할 수 있다. 