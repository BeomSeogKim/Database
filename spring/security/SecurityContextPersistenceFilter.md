# SecurityContextPersistenceFilter

> Security Context의 생성, 저장, 조회를 담당하는 Filter



크게 세가지로 나뉜다. 

- 익명 사용자
- 인증 시
- 인증 후 



### 익명 사용자 

- 새로운 Security Context를 생성 후 Security Context Holder에 저장한다. 
- AnonymousAuthenticationFilter에서 AnonymousAuthenticationToken을 발급받아 Security Context에 저장한다. 



### 인증 시 

- 새로운 Security Context를 생성 후 Security Context Holder에 저장한다. 
- UsernamePasswordAuthenticationFilter에서 UsernamePasswordAuthenticationToken을 발급받아 Security Context에 저장한다. 
- *인증이 완료되면 Http Session에 저장한다.*



### 인증 후 

- Session에서 Security Context를 가져와 Security Context Holder에  저장한다. 
- Security Context안에 Authentication이 존재한다면 인증을 유지한다. 



***공통사항 : 최종 응답 후 Security Context Holder에 존재하는 값들을 지워준다.***



![image-20230811174349907](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/security/SecurityContextPersistenceFilter.png)
