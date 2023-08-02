# HTTP

> HyperText Transfer Protocol 의 약자.
>
> Server - Client 모델을 따르면서 request - response 구조로 웹 상에서 정보를 주고받을 수 있는 프로토콜. 



클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response를 응답하는 구조로 되어있다. 



### HTTP request message의 구성 요소 

- start line (method, path, HTTP Version)
- headers 
- body 

![image-20230802170458491](/Users/github/TIL/cs/images/HTTP/request.png)

###  HTTP response message의 구성 요소 

- status line (HTTP version, status code, status message)
- headers
- body

![image-20230802170549371](/Users/github/TIL/cs/images/HTTP/response.png)



### ConnectionLess & StateLess

HTTP는 서버에 연결 후 요청에 대한 응답을 받으면 연결을 끊어버리는 특성을 지닌다. (ConnectionLess)

이러한 특성으로 인해 많은 사람이 웹을 이용하더라도 실제 동시 접속자 수를 줄여 더 많은 유저의 요청 사항을 처리할 수 있다. 

다만, 연결을 끊어버리기 때문에 클라이언트의 상태를 유지할 수 없다는 단점이 생긴다. (StateLess)

HTTP의 단점을 보안하기 위해 Cookie, Session, JWT 등이 도입되었다. 