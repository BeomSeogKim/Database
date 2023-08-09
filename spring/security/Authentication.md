# Authentication

> Authentication이란 사용자가 누구인지를 증명하는 것이다. 

Authentication은 사용자의 인증 정보를 저장하는 토큰 개념이다. 

**인증 시** => 아이디와 비밀번호를 담고 인증 검증을 위해 전달되어 사용된다. 

**인증 후** => 최종 인증 결과(user 객체, 권한정보)를 담고, Security Context에 저장되어 전역적으로 참조가 가능하다. 



Authentication Parameter

- principal : 사용자 아이디 혹은 사용자 객체를 저장
- credential : 비밀번호
- authorities : 인증된 사용자의 권한 목록
- details : 인증 부가 정보
- Authenticated : 인증 여부

![image-20230809162728239](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/security/Authentication.png)

[인증 전]

- Principal : 로그인 하고자 하는 유저의 아이디
- Credential : 로그인 하고자 하는 유저의 비밀번호
- Authorities : 인증 되기 전이므로 값이 없음
- Authenticated : 인증이 되지 않았으므로 False



[인증 후]

- Principal : 유저 객체
- Credential : 애플리케이션 마다 다르지만, 보통 보안을 위해 null
- Authorities : 유저가 가지고 있는 권한 목록들 
- Authenticated : 인증 된 후 이므로 True

