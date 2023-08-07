# AccessDecisionManager

> 여러가지 정보를 통해 사용자의 리소스 접근 요청을 허락할 것인지 거절할 것인지 결정하는 주체

- AccessDecisionManager는 인증정보, 요청정보, 권한정보를 이용해 사용자의 접근 허용 여부를 결정한다. 
- 여러개의 Voter를 가질 수 있으며, Voter들의 접근 허용, 거부, 보류에 해당되는 값을 리턴받고, 허용 여부를 결정한다.
- 최종 접근 거부 시 예외가 발생한다. 



#### 접근결정의 세가지 유형 

##### AffirmativeBased

- Voter들의 결정 여부 중 하나라도 허용이 된다면 접근을 허용해주는 방식

##### ConsensusBased

- Voter들의 접근 허용 / 거부 에 대해서 다수결로 결정하는 방식
- 동점일 경우
  - Default : 접근 허용
  - 접근 거부시키고 싶다면 alloIfEqualGrantedDeniedDecision을 false로 변경

##### UnanimousBased

- Voter 들의 결정 여부 중 하나라도 거부가 된다면 접근을 거부하는 방식





### AccessDecisionVoter

> 판단을 심사하는 위원

Voter가 판단 시 사용하는 자료 

- Authentication : 인증 정보
- FilterInvocation : 요청 정보 (antMatcher("/member"))
- ConfigAttributes : 권한 정보 (hasRole("USER"))



Voter의 결정 방식 

- ACCESS_GRANTED : 접근 허용( 1 )
- ACCESS_DENIED : 접근 거부 ( -1 )
- ACCESS_ABSTAIN : 접근 보류 ( 0 )



## 동작 방식

![image-20230807100854962](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/security/AccessDecisionManager.png)

FilterSecurityInterceptor 에서는 AccessDecisionManager에게 인가 처리를 맡긴다. 

AccessDecisionManager는 각각의 AccessDecisionVoter에게 투표를 맡긴다. 

투표결과를 취합한 후 접근 결정의 유형에 따라 결정한 다음 만약 접근 거부가 된다면 ExceptionTranslatorFilter를 통해 예외를 발생시킨다.
