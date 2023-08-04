# UsernamePasswordAuthenticationFilter

> 로그인 아이디 / 비밀번호를 기반한 Authentication Filter

![image-20230803172340082](/Users/github/TIL/spring/images/security/UsernameAuthenticationFilter.png)

## 동작 과정 상세 설명 

### 1

Request가 form login page url에 해당하는지 검증한다. 

###### [true]

> UsernamePasswordAuthentciation 검증 과정 진행 

###### [false]

> chan.doFIlter()로 다른 필터 작동 



#### 2

form login에서 client가 기입한 값을 토대로 username, password를 포함한 Authentication 을 생성 



### 3

생성된 Authentication을 Authentication에게 넘기고 검증 위임



### 4

AuthencticationProvider를 호출해 Authentication 검증을 위임

###### 

### 5 & 6

Authentication 유효성 검사 실시 

###### [잘못된 Authentication]

> Authentication Exception을 발생시키고 UsernamePasswordAuthenticationFIlter에게 전달

###### [정상 Authentication]

> 검증된 Authentication을 발급한 후 에 Authentciation Manager에게 전달



### 7

생성된 Authentication을 Security Context에 저장



### 8

성공한 이후의 작업을 수행할 Class 호출