# 정규화(Normalization)

> 정규화란 데이터베이스의 이상현상의 원인이 되는 데이터 중복성을 제거 해 무결성을 보존하는 것이다. 



## 1NF

> Relation R의 모든 속성 값이 도메인의 원자 값을 가지는 Relation

|  학생  | 수강중 강의 |
| :----: | :---------: |
|  KBS   |   CS, SQL   |
| ROBERT |     SQL     |
|  EUN   | SPRING, SQL |

위 릴레이션에서 KBS, EUN의 수강중 강의는 쪼갤 수 있다. 

따라서 다음과 같이 분리해야한다. 



|  학생  | 수강중 강의 |
| :----: | :---------: |
|  KBS   |     CS      |
|  KBS   |     SQL     |
| ROBERT |     SQL     |
|  EUN   |   SPRING    |
|  EUN   |     SQL     |



## 2NF

> 완전 함수 종속성을 충족 해야 한다. 
>
> => A, B가 Relation R의 속성이며, A -> B 종속성이 성립할때, B가 A의 속성 전체에 함수 종속하고, 
>
> 부분 집합 속성에 함수 종속하지 않을 경우를 의미함. 



|   학번   | 강의명 | 강의실 | 성적 |
| :------: | :----: | :----: | :--: |
| 20141234 |  SQL   | 백204  | 4.3  |
| 20151234 |  SQL   | 백204  | 4.0  |
| 20168872 |   CS   | 청201  | 3.5  |
| 20179900 |   CS   | 청201  | 3.7  |



이 경우 {학번, 강의명} 을 복합키를 기본키로 사용할 수 있다. 

성적의 경우 {학번, 강의명} 에 종속한다. 이에 반해 강의실의 경우 {강의명} 에 종속한다. 이러한 경우를 **부분 함수 종속성**이라 말한다. 

이러한 경우 제 2정규화를 진행한다. 

|   학번   | 강의명 | 성적 |
| :------: | :----: | :--: |
| 20141234 |  SQL   | 4.3  |
| 20151234 |  SQL   | 4.0  |
| 20168872 |   CS   | 3.5  |
| 20179900 |   CS   | 3.7  |

| 강의명 | 강의실 |
| :----: | :----: |
|  SQL   | 백204  |
|   CS   | 청 201 |



## 3NF

> 기본키가 아닌 속성이 기본키에 비이행적으로 (non-transitive) 종속해야 한다. 
>
> A => B, B => C가 성립되는 함수 종속성을  가진다. 

[문제가 있는 Relation]

|   학번   | 강의명 | 수강료 |
| :------: | :----: | :----: |
| 20141234 |  SQL   | 20,000 |
| 20151234 |   CS   | 30,000 |
| 20161234 | SPRING | 90,000 |

해당 Relation의 종속성을 보면 다음과 같다.  (이행적 함수 종속성)

- 학번 => 강의명
- 강의명 => 수강료
- 학번 => 수강료



이러한 Relation의 경우 다음과 같은 문제가 발생. 

1. 학생이 많아질 수록, 강의명, 수강료의 중복도 증가
2. 학번 없이 강의명 등록 불가 
3. 학생이 강의명을 지울 경우 해당 강의 정보 더이상 조회 불가



이러한 경우 이행적 종속성을 끊어주어야 한다. 

|   학번   | 강의명 |
| :------: | :----: |
| 20141234 |  SQL   |
| 20151234 |   CS   |
| 20161234 | SPRING |



| 강의명 | 수강료 |
| :----: | :----: |
|  SQL   | 20,000 |
|   CS   | 30,000 |
| SPRING | 90,000 |
