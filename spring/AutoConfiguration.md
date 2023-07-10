# [Spring] Auto Configuration

Spring Boot는 자동 구성이라는 기능을 제공함. 

**자동 구성**  : 일반적으로 자주 사용하는 수많은 빈들을 자동으로 등록해주는 기능 



## 동작 원리 



### @SpringBootApplication

```java
@SpringBootApplication
public class VoucherApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(VoucherApplication.class, args);
    }

}
```

AutoConfiguration은 `@SpringBootApplication`으로 부터 일어난다. 

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

`@SpringBootApplication`에는 여러가지 어노테이션이 포함되어 있다.   
그 중 자동 구성과 관련된 어노테이션은 `@EnableAutoConfiguration`. 

#### @EnableAutoConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
```

EnableAutoConfiguration은 AutoConfiguration을 활성화 하는 기능을 제공해 준다. 

EnableAutoConfiguration은 AutoConfigurationImportSelector을 사용하는데, Selector는 동적으로 설정정보를 선택할 수 있도록 구현되어있다. 

![image-20230710135046346](/Users/github/TIL/spring/images/AutoConfiguration/AutoConfiguration1.png)

![image-20230710135146784](/Users/github/TIL/spring/images/AutoConfiguration/AutoConfiguration2.png)

Spring Boot는 spring-boot-autoconfigure에 있는 META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports 설정 파일들을 읽어 들여 자동 구성을 해준다. 



## 사용자가 만든 jar 파일 자동 구성 등록

#### jar 파일을 만들고자 하는 프로젝트에서 Config 관련된 클래스에 @AutoConfiguration 등록 

```java
@AutoConfiguration
@ConditionalOnProperty(name = "version", havingValue = "v2")
public class MyAppAutoConfig {
```

***@ConditionalOnProperty는 환경정보에 version=v2 옵션이 있을 때 동작하게끔 하는 장치이다.***

#### resources/META-INF/spring 하위 폴더에 파일 생성

![image-20230710140121449](/Users/github/TIL/spring/images/AutoConfiguration/AutoConfiguration3.png)

파일 이름은  사진과 같이 동일한 이름이어야 한다. 

![AutoConfiguration4](/Users/github/TIL/spring/images/AutoConfiguration/AutoConfiguration4.png)

`java` directory를 기준으로 해 경로 및 Class 이름 지정 . 

#### build를 하여 jar 파일 생성 

#### 사용하고자하는 프로젝트에서 경로에 jar 파일 주입 

libs라는 경로에 jar 파일을 주입 했다고 가정.

#### gradle 파일에 다음과 같이 추가 

```java
dependencies {
    implementation files("libs/memory-v2.jar")
		// 추가 의존성 ... 
}
```

