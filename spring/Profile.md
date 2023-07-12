# [Spring] 외부 설정

개발을 진행하다보면 실제 배포 서버와, 테스트 서버, 로컬 환경에서 사용해야하는 값들이 서로 다른 일들이 많다. 

> 로그 설정, DB연결, 등등 .. 

## 배포 방식 

### 환경에 따른 Build 파일 생성

배포 환경마다 다르게 값을 설정하는 가장 기본적인 방식은 개발자가 `직접` 배포환경에 맞게 Application을 build하는 것이다. 

![image-20230712165651031](/Users/github/TIL/spring/images/Profiles/Profiles1.png)

이러한 방식의 경우 다음과 같은 단점을 지닌다. 

1. 환경에 따라 여러번 Build 과정을 거침
2. 환경에 따라 Build의 결과물이 다르다. 
   - 한가지 환경에서 빌드에 성공을 했다고 다른 환경에서 빌드가 성공할 것이라는 보장을 하지 못한다. 

### 실행 시 외부 설정값 주입

이러한 단점을 해결하기 위해 설정값들을 실행 시점에 주입을 할 수 있다. 

![image-20230712170117281](/Users/github/TIL/spring/images/Profiles/Profiles2.png)

이러한 방식의 경우 다음과 같은 장점을 가진다.

1. Build의 결과물이 같으므로 환경에 따른 Build 결과물이 동일하다. 
2. 외부설정 주입을 통한 유연한 환경 구축 가능 



## 외부설정 주입 방법

외부 설정은 4가지 방식으로 주입을 할 수 있다. 

###### OS 환경 변수 

###### 자바 시스템 속성

###### 자바 커맨드 라인 인수 

###### 외부 파일



### OS 환경 변수 

OS 환경 변수란 해당 OS를 사용하는 모든 프로그램에서 읽을 수 있는 설정값이다. 

다르 외부설정과 비교해 `사용범위가 가장 넓다`는 특징을 지님

### 자바 시스템 속성 

자바 시스템 속성은 실행한 JVM 안에서 접근 가능한 외부설정이다. 

자바 시스템 속성을 주입해주기 위해서 jar 파일을 실행할 때 다음과 같이 값을 주입할 수 있다. 

> username=hello, password=sa 값을 주입하고자 하며, app.jar 파일이 있다고 가정 

```shell
java -Dusername=hello -Dpassword=sa -jar app.jar 
```

자바 시스템 속성을 사용할 때에는 key값 앞에 -D를 붙여서 사용하면 된다. 

IntelliJ에서는 다음과 같은 설정을 통해 보다 간편하게 실행시킬 수 있다. 

![image-20230712171617580](/Users/github/TIL/spring/images/Profiles/Profiles3.png)

### 커맨드 라인 인수 

```java
public static void main(String[] args) {
    SpringApplication.run(ExternalApplication.class, args);
}
```

커맨드라인 인수는 애플리케이션 실행시점에서 `args`에 전달되는 값이다. 

스프링은 커맨드라인 인수를 key-value 형식으로 받을 수 있도록 지원을 한다. 

key-value형식으로 값을 전달해 주기 위해서는 다음과 같이 사용하면 된다. 

```shell
java -jar app.jar --username=hello --password=sa
```



## 설정 데이터 

> 설정 데이터의 경우 코드 외부에 존재하는 파일을 읽어오는 방식인 외부 설정파일 조회 방식과, 
>
> 코드 내부에 존재하는 파일을 읽는 방식인 내부 설정파일 조회 방식이 존재한다. 
>
> 보안과 관련된 내용 외적으로는 내부 설정파일을 많이 쓰므로 내부 설정 파일 조회 방식만 알아볼 예정.

내부 설정 파일 조회의 경우 두가지 방식이 존재한다. 

##### profile에 따른 별도의 properties 파일 생성 

##### 하나의 properties파일에서 별도의 설정 구분 



### 별도의 properties 파일 관리

스프링은 `spring.profiles.active` 값을 읽어 프로필을 할당한다. 

만약 자바 시스템 속성으로 `-Dspring.profiles.active=dev` 설정을 주었다고 한다면 스프링은 다음의 규칙을 만족하여 설정값을 조회한다. 

`application-dev.properties` 

이렇게 properties를 관리하는 경우 파일이 늘어난다는 단점이 존재한다. 

보통은 하나의 properties에 서로 다른 profile에 대한 값들을 같이 관리하는 방식을 사용한다. 



### 하나의 properties 파일 관리 

```properties
url=local.db.com
username= local_user
password=local_pw
#---
spring.config.activate.on-profile=dev
url=dev.db.com
username= dev_user
password=dev_pw
#---
spring.config.activate.on-profile=prod
url=prod.db.com
username= prod_user
password=prod_pw
#---
```

기본적으로 하나의 properties 파일에서 여러가지 설정을 관리할 때에는 `#---`를 사용.

최상단 설정의 경우 default 설정으로, profile이 아무것도 명시 되지 않을 경우 기본적으로 사용되는 설정값. 

이후 특정 profile에 따른 값을 설정하기 위해 `spring.config.activate.on-profile`를 사용.

