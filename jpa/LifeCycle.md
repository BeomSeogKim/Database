# Life Cycle of Entity 

> 엔티티의 생명주기는 크게 4개로 나뉘어 진다. 
>
> 비영속(new / transient), 영속(managed), 준영속(detached), 삭제(removed)

![image-20230719114039135](https://github.com/BeomSeogKim/TIL/blob/main/jpa/images/lifecycle/image-20230719114039135.png)



## 비영속 상태 

영속성 컨텍스트와 전혀 관계가 없는 새로운 상태 



## 영속 상태 

영속성 컨텍스트에 관리가 되는 상태 

```java
Member member = new Member();	// 비영속 상태
// member setting logic 

EntityManger em = // ... Entity Manager 가져오기 
  
em.persist(member);	// 영속 상태 -> 영속성 컨텍스트에 등록해 관리됨
```



## 준영속 상태

영속성 컨텍스트에 관리가 되다 분리된 상태 

준영속 상태를 만드는 방법에는 `detach` `clear` `close` 방식이 있다. 

###### detach

특정 엔티티만 준 영속 상태로 전환

###### clear

영속성 컨텍스트 초기화 

###### close

영속성 컨텍스트 종료



## 삭제 상태 

삭제된 상태 
