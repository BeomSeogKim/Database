# HTTP method



## GET 

리소스를 조회할 때 사용하는 method

보통은 Query String 을 사용해 요청을 넘긴다. 

> Query String 이란?
>
> url 주소 끝에 붙여서 서버에게 {key} - {value} 형식으로 파라미터를 넘기는 방식을 의미한다. 
>
> ? 로 시작하며 여러가지가 있을 경우  & 를 사용해서 이어붙인다. 
>
> ex) http://localhost:8080/users?name=hello&age=20



## POST

클라이언트에서 요청한 데이터를 처리할 때 사용하는 method 

요청한 데이터를 처리하는 방식은 크게 두가지로 분류할 수 있다. 

1. 요청한 데이터를 토대로 리소스 생성 
2. 특정 프로세스 처리 



## PUT 

리소스를 `대체`하려고 하는 경우 사용하는 method. 

PUT의 경우 리소스가 없다면 리소스를 생성한다. 

> 대체란 모든 값들을 변경한다는 뜻이다. 
>
> 예를들어 user {name : tommy, age : 10}이 있었는데 PUT {name : Ralph} 요청이 들어온다면 
>
> user{name : Ralph} 로 변경된다. 



## PATCH

리소스의 `일부분을 수정`하려고 할 때 사용하는 method

> PATCH의 경우 리소스의 일부분을 수정하기 때문에 PUT 과는 다소 다르다. 
>
> 예를들어 user {name : tommy, age : 10}이 있었는데 PATCH {name : Ralph} 요청이 들어온다면 
>
> user{name : Ralph, age : 10} 로 변경된다. 



## DELETE

리소스를 삭제하려고 할 때 사용하는 method