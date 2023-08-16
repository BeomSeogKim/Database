# JDBC (Java DataBase Connectivity)



### Backend Application의 동작 과정 

![JDBC-1](/Users/github/TIL/spring/images/database/JDBC-1.png)

백엔드 어플리케이션은 웹, 핸드폰, 패드 등등으로부터 요청을 받으면, 데이터베이스로 부터 값들을 조회한 후 특정 로직을 수행하거나 응답을 내려줍니다. 

이 때 어플리케이션과 데이터베이스의 상호작용을 조금 더 자세히 살펴보면 다음과 같습니다. 

![image-JDBC2](/Users/github/TIL/spring/images/database/JDBC-2.png)

1. TCP / IP 연결
2. 필요한 로직을 위한 SQL 전달 (e.g. "select * from member where member_id = 1L")
3. SQL에 해당하는 결과를 반환



### JDBC의 등장 배경

만약 백엔드 팀이 기존에는 MySQL 데이터베이스를 사용하다가, Oracle 데이터 베이스를 사용하려고 합니다.

이럴 때 JDBC가 없는 시절에는 다음과 같은 로직들을 일일히 고쳐주어야 했습니다. 

- TCP / IP 연결과 관련된 코드
- SQL을 전달하는 코드 
- SQL에 대한 응답을 받는 코드 



이러한 방식의 개발을 하게 된다면, **데이터베이스가 바뀔 때 마다 관련된 코드를 변경**해야하는 단점과, **데이터베이스별로 사용되는 코드를 익혀야한다**는 단점이 존재합니다. 

이를 해결하기 위해 **JDBC**가 등장합니다. 

JDBC는 Connection, Statement, ResultSet에 대한 코드를 인터페이스화 시켜놓은 추상체입니다. 

그렇기 때문에 더이상 개발자는 데이터베이스마다 사용하는 코드를 일일히 외울 필요가 없어졌습니다. 

![image-JDBC-3](/Users/github/TIL/spring/images/database/JDBC-3.png)



### SQLMapper와 ORM의 등장

JDBC가 나타난 이후로 불편했던 많은 부분들이 해결 되었습니다. 

그렇지만 JDBC는 1997년에 나온 기술로 지원이 안되는 부분들이 여럿 존재합니다. 

예를 들어 Paging과 관련된 쿼리가 그 예시입니다. 

보다 편리하게 사용하기 위해 `SQL Mapper`와 `ORM`이 등장합니다. 



#### SQL Mapper

SQL Mapper는 작성한 query를 적절하게 변경해주는 역할을 수행합니다.

SQL Mapper의 기술로는 JdbcTemplate, MyBatis가 있습니다. 

![image-JDBC-4](/Users/github/TIL/spring/images/database/JDBC-4.png)



#### ORM

ORM의 경우 객체에 관련된 매핑정보를 읽어들여 SQL을 작성해주는 역할을 수행합니다. 

JPA는 인터페이스며, JPA의 구현체로 Hibernate, EclipseLink가 있습니다. 

![image-20230816190016687](/Users/github/TIL/spring/images/database/JDBC-5.png)