# [Spring] Component Scan

> 그 동안 컴포넌트 스캔에 대해서 생각을 해 본 적도 사용한 적도 없어 Component Scan이 무엇인지 잘 몰랐다.
>
> 이 부분에 대해서 다뤄 보려고 한다. 



MemberServiceImpl, MemberRepositoryImpl 에 대해서만 고려를 한다. 



## Component등록

[Code]

```java
@Component
public class MemberServiceImpl implements MemberService{
 ... 
}
```

```java
@Component
public class MemberRepositoryImpl implements MemberRepository{
  ...
}
```

MemberServiceImpl과 MemberRepositoryImpl에 @Component 가 붙어있으면 스프링 컨테이너에 다음과 같이 등록된다.

![image-20230308222736425](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/image-20230308222736425.png)

`@ComponentScan`은 `@Component`가 붙은 모든 클래스들을 스프링 빈으로 등록한다. 

등록시에 다음과 같은 규칙을 가진다.

* 기본 전략
  * 클래스명을 그대로 사용하되 앞글자만 소문자를 사용
    * MemberServiceImpl -> memberServiceImpl
    * MemberRepositoryImpl -> memberRepositoryImpl
* 직접 지정
  * 빈의 이름을 직접 지정하는 방식
    * @Component("넣고자하는 이름")

## @Autowired 기능

> 생성자에 @Autowired가 있다면 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아 주입해준다. 

[Code]

```java
@Component
public class MemberServiceImpl implements MemberService{
	private final MemberRepository memberRepository;
  
  @Autowired
  public MemberServiceImpl(MemberRepository memberRepository){
    this.memberRepository = memberRepository;
  }
 ... 
}
```

위 코드에서 MemberRepository는 Interface 이다. 

` @Autowired` 를 사용한다면 스프링 컨테이너가 등록된 MemberRepoository 빈을 찾아 주입시켜준다. 



## 탐색 위치 및 기본 스캔 대상

> basePackages 옵션을 사용해 탐색할 패키지의 시작 위치를 지정할 수 있다. 

[Code]

```java
@ComponentScan(
  			basePackages = "{탐색을 시작할 패키지}"
)
```

위와 같이 basePackages 옵션을 사용하면 탐색할 패키지의 시작위치를 지정할 수있다.

basePackages의 경우 여러가지 패키지를 지정할 수도 있다. 이러한 경우 다음과 같이 구성해주면된다. 

[Code]

```java
@ComponentScan(
  			basePackages = {"{탐색을 시작할 패키지}","{탐색을 시작할 패키지}"}
)
```

basePackages 옵션을 사용하지 않을 경우에는 `@ComponentScan` 이 붙은 클래스의 패키지가 시작 위치가 된다. 

> 저는 @ComponentScan을 사용한 적이 없는데 그거 없어도 Spring 실행 잘만 되던데요?

**우리가 그냥 사용해서 몰랐을 뿐 이미 적용이 되어 있다.**



![@SpringBootApplication](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/%40SpringBootApplication.png)

위 사진이 익숙하지 않은가? 

맞다 바로 스프링 부트를 사용한다면 기본으로 생성이 되어있는 클래스이다. 

@SpringBootApplication을 파고 들어가보면

![@SpringBootApplication2](https://github.com/BeomSeogKim/TIL/blob/main/spring/images/%40SpringBootApplication2.png)

@SpringBootApplication에 @ComponentScan 이 붙어있는 것을 확인할 수 있다. 



## 컴포넌트 스캔 대상

> @Component 가 붙은 곳에는 모두 컴포넌트 스캔 대상이다.

대표적인 컴포넌트 스캔 대상은 다음과 같다.

* @Component
* @Controller
  * MVC Controller에서 사용
* @Service
  * 비즈니스 로직에 사용 
* @Repository
  * 데이터 접근 계층에서 사용
* @Configuration
  * 설정 정보에서 사용 

별다른 마법이 있는 것은 아니고 @Component 가 붙은 곳은 다 컴포넌트 스캔 대상이 된다.

@Controller, @Service, @Repository, @Configuration 모두 코드를 들여다 보면 @Component 가 붙어있다.



## 컴포넌트 대상 제외 

> @ComponentScan 에서 옵션을 활용해 컴포넌트 대상에서 제외 시킬 수 있다. 

[Code]

```java
@ComponentScan(
	excludeFilters = @Filter(type = FilterType.Annotation, classes = 제외할 클래스)
)
```



FilterType에는 총 5개의  옵션이 있다. 

* ANNOTATION 
  * 기본값, 어노테이션을 인식해서 동작
* ASSIGNABLE_TYPE
  * 지정한 타입과 자식 타입을 인식해 동작
* ASPECTJ
  * AspectJ 패턴 사용
* REGEX
  * 정규표현식 사용
* CUSTOM
  * TypeFilter 인터페이스를 구현해서 처리
