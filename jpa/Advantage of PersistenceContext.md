# 영속성 컨텍스트의 이점

> Application과 데이터베이스 사이에 속한 ORM 기술의 경우 중간에 위치함으로써 많은 이점을 취할 수 있다. 
>
> 대표적인 이점은 다음과 같다.

###### 1차 캐시 

###### 동일성 보장

###### 트랜잭션을 지원하는 쓰기 지연

###### 변경 감지



## 1차 캐시 

영속성 컨텍스트안에 내부적으로 1차 캐시라는 공간을 가진다. 

데이터를 조회할 때 만약 1차 캐시에 데이터가 있으면 바로 가져오고, 데이터가 없다면 DataBase를 통해서 데이터를 가져온다. 

#### [1차 캐시에 데이터가 없는 경우]

![image-20230719121056877](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/Advantage/image-20230719121056877.png)

![image-20230719121150726](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/Advantage/image-20230719121150726.png)

![image-20230719121222986](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/Advantage/image-20230719121222986.png)

![image-20230719121244175](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/Advantage/image-20230719121244175.png)

#### [1차 캐시에 데이터가 있는 경우]

![image-20230719121959849](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/Advantage/image-20230719121959849.png)

![image-20230719122033652](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/Advantage/image-20230719122033652.png)

## 동일성 보장

값을 조회할 때 1차 캐시에 있는 값을 조회해오기 때문에 같은 인스턴스임을 보장한다. 

```java
Member member1 = em.find(Member.class, 1L);
Member member2 = em.find(Member.class, 1L);

var bool = (member1 == member2);	// bool = true
```



## 트랜잭션을 지원하는 쓰기 지연

데이터에 변경사항이 있을 때 Query를 모아두었다 Database에 전송하는 방식을 지원한다. 

```java
EntityManager em =  (Some Logic to get EntityManger) ;

transaction.begin();	// 트랜잭션 시작

Member tommy = new Member("tommy");
Member elsa = new Member("elsa");

em.persist(tommy);	// SQL 저장소에 쿼리 로직 저장. Database에 쿼리 날리지 않음
em.persist(elsa);		// SQL 저장소에 쿼리 로직 저장. Database에 쿼리 날리지 않음

transaction.commit();		// SQL 저장소에 있던 쿼리 DB에 실행
```



## 변경 감지

영속성 컨텍스트에 관리되는 Entity의 경우 (**영속 상태**) 기존의 상태와 다를 경우 자동으로 업데이트가 가능하다.

```java
EntityManager em =  (Some Logic to get EntityManger) ;

transaction.begin();	// 트랜잭션 시작

Member member = em.find(Member.class, 1L);

member.setName("new-nickname");

transaction.commit();		// 변경 감지 => Update Query 실행
```
