# SecurityContext &  SecurityContextHolder



## Security Context

Authentication 객체가 저장되는 보관소로 필요시 언제든 Authentication을 꺼내 쓸 수 있다. 

ThreadLocal에 저장되어 아무 곳에서나 참조가 가능하도록 설계되어 있다. 

인증이 완료될 경우 HttpSession에 저장되어 어플리케이션 전반에 걸쳐 참조가 가능하다. 



## Security Context Holder

Security Context 저장방식을 지정

- MODE_THREADLOCAL : 쓰레드 당 Security Context 객체를 할당 (Default)
- MODE_INHERITABLETHREADLOCAL : 메인 쓰레드와 자식 쓰레드에 관해 동일한 Security Context 유지
- MODE_GLOBAL : 응용 프로그램에서 단 하나의 Security Context를 저장.





![image-20230809164521938](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/security/SecurityContext.png)
