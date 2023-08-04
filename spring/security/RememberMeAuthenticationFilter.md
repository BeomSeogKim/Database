# RememberMeAuthenticationFilter

> Remember me 기능을 사용할 때 동작하는 Filter
>
> 기본적으로 Authentication이 존재하지 않고, remember-me 토큰이 존재할 때 Filter가 동작한다. 



![image-20230804164139201](/Users/github/TIL/spring/images/security/RememberMeAuthenticationFilter.png)



RememberMeServices에는 두가지 종류가 존재한다. 

###### TokenBasedRememberMeServices

> In-memory에 존재하는 토큰과 HTTP request로 요청한 토큰을 비교한다. 

###### PersistentTokenBasedRememberMeServices

> Database에 존재하는 토큰과 HTTP request로 요청한 토큰을 비교한다. 



## 동작 과정 설명

#### Token Cookie 추출

Http request로 요청한 Cookie를 추출

 

#### 토큰 존재 유무 판단

Cookie를 추출 했을 때 존재 유무에 따라 로직이 다르게 동작한다. 

###### [토큰이 존재하지 않는 경우]

해당 필터 로직을 수행하지 않으며, chain.doFilter()를 호출한다. 

###### [토큰이 존재하는 경우]

토큰의 유효성 검사를 진행한다. 



#### Token Validation Logic

###### [Decode Token]

> 토큰의 유효성을 검사한다. 
>
> 검사 결과 문제가 없을 경우 추가 검증 로직을 수행하며 문제가 있는 경우 Exception을 발생시킨다.



###### [토큰 일치 여부 검사]

> HTTP request로 요청한 토큰과 Security Context에 존재하는 토큰이 일치하는 지 검증한다. 
>
> 일치하는 경우 추가 검증 로직을 수행하며 문제가 있는 경우 Exception을 발생시킨다. 



###### [User 계정 여부 검사]

> 토큰에 User정보가 있는지 검증한다. 
>
> User 정보가 있는 경우 Authentication을 생성해서 Authentication Manager에 넘겨 인증 처리를 진행한다. 
>
> User 정보가 없는 경우 Exception을 발생시킨다. 

###### 