# CSRF (Cross Site Request Forgery)

> CSRF는 해킹 방식의 일종으로 보통 스팸 메일 혹은 스팸 문자와 같은 것을 사용해 해킹을 시도하는 방식이다. 



## CSRF 공격 방식

![image-20230807184405466](/Users/github/TIL/spring/images/security/CSRF.png)

1. USER는 SERVER에 로그인을 한 상태 
   - USER에게 SERVER가 발급해준 Token이 존재한다. 
2. HACKER가 USER에게 악성 링크를 전송
3. USER가 악성 링크를 눌러 사이트 접속
4. 이미지 혹은 추가적인 링크를 누름 
   - Link에는 쇼핑몰의 주문과 관련된 주소가 심어져있다.
5. 링크를 누르는 순간 USER의 정보를 통해 SERVER에 주문 및 특정 동작을 실행 



이러한 방식을 방지하기 위해서 Spring Security는 다음과 같은 방식을 도입했다. 

- 모든 HTTP요청에 대해서 파라미터로 랜덤하게 생성된 토큰을 요구
- 요청시 전송한 토큰이 없거나, 서버에 있는 토큰과 비교했을 때 일치하지 않는다면 요청은 실패. 