# Connection Pool & DataSource



## Connection Pool

> Connection Pool 이란 애플리케이션이 시작될 때 여러개의 커넥션을 연결 한 후 저장하는 저장소의 역할을 한다. 

### 커넥션 조회 및 생성 과정 

![ConnectionPool-1](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/ConnectionPool-1.png)

Connection Pool을 사용하지 않을 때, 일반적인 커넥션 조회 및 생성 과정은 위의 그림과 같다. 

1. DB Driver에게 Connection 조회 요청
2. DB Driver와 DataBase간에 TCP / IP 연결 수립
3. DataBase에 Username, Password, 기타 정보를 전달
4. DataBase 내부에서 주어진 인자들로 인증 후 DB Session 생성
5. DB Session 생성 후 DB Driver에게 생성 완료 응답 메세지를 보냄
6. DB Driver가 Connection 객체 반환 

위의 작업은 DataBase마다 다르지만 시간이 많이 소요되는 작업이다. 

> MySQL의 경우 수 ms 정도 걸리지만 다른 DB의 경우 수십 ms까지도 걸린다고 한다. 



클라이언트가 서버에 요청을 했을 때, Connection 생성 및 조회의 시간까지 기다리게 되어 **좋지 않은 사용자 경험**을 준다. 

이러한 단점을 보완하기 위해 Connection Pool 이라는 개념이 생겨난다. 



### Connection Pool 초기화 과정 

![ConnectionPool-2](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/ConnectionPool-2.png)

애플리케이션이 시작 될 때 Connection Pool은 미리 설정해 둔 Connection의 개수만큼 Connection을 생성해 Connection Pool에 저장을 해둔다. 

커넥션 풀에 존재하는 커넥션들을 데이터베이스와 연결되어 있는 상태이기에 언제든지 **즉시 사용**할 수 있다. 



### Connection Pool 사용 과정 

![image-20230817155932826](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/ConnectionPool-3.png)

Connection Pool을 사용할 경우 Connection을 조회할 때 Connection Pool에 미리 생성해둔 Connection을 반환한다. 

![ConnectionPool-4](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/ConnectionPool-4.png)

Connection을 다 사용한 경우 Connection 객체를 다시 Connection Pool에 반환한다



## DataSource

> DB Driver에서 Connection을 유연하게 반환하기 위해 도입된 인터페이스 

Application Logic에서 Connection을 얻을 수 있는 방식이 다양하다. 

![DataSource-1](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/DataSource-1.png)

만약 DriverManager를 사용하고 있다가 Hikari CP를 사용하려고 하는 경우 애플리케이션 코드를 수정해야 한다는 문제점이 생긴다. 

이를 해결 하기 위해 DataSource라는 인터페이스가 생겨났다. 

![DataSource-2](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/database/DataSource-2.png)

DataSouce의 주된 추상메서드는 `getConnection()` 이다. 

> DriverManager의 경우 DataSource를 구현하지 않아 DataSource를 사용할 수 없다. 
>
> 하지만 Spring에서 DataSource를 구현한 `DriverManagerDataSource`를 제공해주기 때문에 이를 사용하면 된다.  
