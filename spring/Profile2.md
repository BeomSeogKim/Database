# [Spring] 외부 설정 2

> 스플잉에서는 외부 설정 값 들을 크게 세가지 방식으로 조회할 수 있다. 

###### Envirionment

###### @Value

###### @ConfigurationProperties



yml에 다음과 같은 값들이 기재되어 있다고 가정

```yaml
my:
  datasource:
    url: local.db.com
    username: local_user
    password: local_pw
    etc:
      max-connection: 1
      timeout: 3500ms
      options: CACHE, ADMIN
```



---

## Environment

```java
public class EnvTest{
  private final Environment env;
  
  public Envtest(Environment env){
    this.env = env;
  }
  
  void envTest(){
  	String url = env.getProperty("my.datasource.url");
    String username = env.getProperty("my.datasource.username");
    // ... 
    int maxConnection = env.getProperty("my.datasource.etc.max-connection", Integer.class);
    Duration timeout = env.getProperty("my.datasource.etc.timeout", Duration.class);
    // ... 
  }
}
```

Environment의 경우 `getProperty`를 사용해 값을 가져올 수 있다. 

String이 아닌 다른 타입으로 값을 가져오고 싶을 때 `env.getProperty(key, Type)`에서 Type에 원하는 타입 정보를 기입하면 된다. 

##### 단점 

값을 사용할 때마다,`env.getProperty`로 값을 꺼내와야 한다. 



## @Value

```java 
public class ValueTest{
   	@Value("${my.datasource.url}")
    private String url;
    @Value("${my.datasource.username}")
    private String username;
    @Value("${my.datasource.password}")
    private String password;
    @Value("${my.datasource.etc.max-connection}")
    private int maxConnection;
    @Value("${my.datasource.etc.timeout}")
    private Duration timeout;
    @Value("${my.datasource.etc.options}")
    private List<String> options;
}
```

@Value의 경우 외부 설정의 키 값을 주면 원하는 값을 주입 받을 수 있다. 

@Value의 경우 **필드** 에서 사용할 수 도 있고, **파라미터** 에도 사용할 수 있다. 

### 기본값 사용하기 

>  만약 외부설정 값이 없을 때 기본 값을 설정하고자 하면 :뒤에 원하는 값을 적어주면 된다. 

```java
@Value("${my.datasource.etc.max-connection:10}")
private int maxConnection;
```

이 예시의 경우 max-connection 값이 존재 하지 않을경우 10으로 값을 설정한다. 

##### 단점

설정 정보들은 보통 특정 정보의 묶음으로 이루어진다. 

@Value의 경우 외부 설정 정보의 키 값을 입력받고, 주입 받아와야 하는 것이 불편하다.

이러한 정보를 객체로 변환해서 사용할 수 있는 설정이 존재한다. 



## @ConfigurationProperties

> 외부 설정의 묶음 정보를 객체로 변환하는 기능 

***타입 안전한 속성***

객체를 사용할 경우 타입을 사용할 수 있다.

그렇기에 실수로 잘못된 타입이 들어오는 문제를 방지할 수 있음. 

@ConfigurationProperties의 기본 주입 방식은 **자바 빈 프로퍼티 방식**. 

하지만 `setter`가 가지는 단점이 존재하기 때문에 생성자를 통해 주입받아서 사용할 수 있다. 

```java
@Getter
@ConfigurationProperties("my.datasource")
@Validated
public class MyDataSourcePropertiesV3 {
    @NotEmpty
    private String url;
    @NotEmpty
    private String username;
    @NotEmpty
    private String password;
    private Etc etc;

    public MyDataSourcePropertiesV3(String url, String username, String password, @DefaultValue Etc etc) {
        this.url = url;
        this.username = username;
        this.password = password;
        this.etc = etc;
    }

    @Getter
    public static class Etc {
        @Min(1) @Max(999)
        private int maxConnection;
        @DurationMin(seconds = 1) @DurationMax(seconds = 60)
        private Duration timeout;
        private List<String> options = new ArrayList<>();

        public Etc(int maxConnection, Duration timeout, @DefaultValue("DEFAULT") List<String> options) {
            this.maxConnection = maxConnection;
            this.timeout = timeout;
            this.options = options;
        }
    }
}

```

###### ConfigurationProperties("my.datasource")

외부 설정을 주입 받는 객체임을 의미하며, 주입을 시작하고자 하는 key의 묶음 시작점을 명시 해 주면 된다. 

###### @DefaultValue

앞서 살펴봤던 @Value에서 기본값 사용하기와 동일한 기능을 수행한다. 

###### @Validated

만약 외부 설정정보에 대해서 Validation을 적용하고 싶은 경우 `org.springframework.validation.annotation`패키지의 Validated 사용.

잘못된 외부 설정정보를 입력했을 경우 애플리케이션 로딩 시점에 예외를 발생시킨다. 



#### :warning: 주의사항

ConfigurationProperties의 경우 외부 설정을 주입받아 객체를 생성한다. 

이것을 사용하기 위해서는 두가지 방식이 존재한다. 

###### @EnableConfigurationProperties(등록하고자하는 클래스.class)

이 어노테이션을 사용하면 등록하고자 하는 객체를 스프링 빈으로 등록해준다. 

###### @ConfigurationPropertiesScan

이 어노테이션을 사용하면 ComponentScan과 유사하게 동작한다. 



