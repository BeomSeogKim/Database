# ExceptionTranslationFilter

> FilterSecurityInterceptor에서 발생하는 예외를 처리해주는 Filter이다. 

ExceptionTranslationFilter는 크게 두가지 예외에 대해서 처리를 진행한다. 

###### AuthenticationException

> 인증을 하지 않은 Customer가 요청을 할 경우 발생하는 예외

- AuthenticationEntryPoint 호출
  - 로그인 페이지 이동 혹은 401 응답 코드 등을 처리한다. 
- 요청 정보 저장
  - HttpSessionrequestCache에 요청한 정보들을 저장한다. 

###### AccessDeniedException

> 인증된 Customer이지만 권한이 없는 경우 발생하는 예외

- AccessDeniedHandler 호출
  - 접근 제한 안내 페이지 이동 혹은 403 응답 코드 등을 처리한다. 



![image-20230807182401189](/Users/github/TIL/spring/images/security/ExceptionTranslationFilter.png)



## 동작 과정

#### FilterSecurityInterceptor

> 해당 필터에서는 요청사항에 대해서 검증을 한 후 비정상적인 요청일 경우 Exception을 발생시킨다. 

#### ExceptionTranslationFilter

> `AccessDeniedException`, `AuthenticationException`에 대해 예외 처리를 진행한다. 

##### AuthenticationException

> AuthenticationEntryPoint
>
> > 로그인 페이지 이동, Http StatusCode 401 등의 설정들을 진행한다. 

> HttpSessionRequestCache
>
> > 이전 요청에서 Server에 전송한 값들을 DefaultSavedRequest 객체로 생성해 HttpSessionRequestCache에 저장한다. 



#### AccessDeniedException

> AccessDeniedHandler를 호출 해 페이지 이동, Http StatusCode 403 등의 설정들을 진행한다. 