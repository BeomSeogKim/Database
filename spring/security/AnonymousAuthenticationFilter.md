# AnonymousAuthenticationFilter

> 로그인 하지 않은 사용자의 경우 `null`이 아닌 Anonymous객체를 만들어서 처리하는 필터



![image-20230804173005366](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/security/AnonymousAuthenticationFilter.png)



### 동작 과정 설명 

#### Authentication 유무 검사

Security Context에 Authentication이 있는지 체크를한다. 

###### Authentication 존재 

> chain.doFilter() 실행 

###### Authentication 존재 X

> Anonymous User객체를 생성
>
> AnonymousAuthenticationToken을 발급한다. 
>
> ***Anonymous User의 경우 세션에는 따로 저장되지 않는다.***

