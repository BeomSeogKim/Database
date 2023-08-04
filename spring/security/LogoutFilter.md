# LogoutFilter

> 로그아웃 시 작동하는 Filter

![image-20230803180653941](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/security/LogoutFilter.png)

### 작동 방식 

#### AntPathRequestMatcher

요청 url이 logout url 인지 검증 

###### true 

> Logout Filter 수행 

###### False

> Logout Filter 수행하지 않음



#### Authentication

Security Context에 존재하는 Authentication을 가져온다. 



#### SecurityContextLogoutHandler

크게 세가지 역할을 한다. 

- 세션 무효화
- 쿠키 삭제
- Security Context 초기화



#### SimpleUrlLogoutSuccessHandler

단순하게 redirect 기능을 가지고 있다. 

***만약 다른 기능을 추가하고 싶다면 직접 구현하면 된다***
