# 자바의 예외 

![image-20230822004059389](https://github.com/BeomSeogKim/TIL/blob/main/java/images/Exception.png)

##### [Error]

- 메모리 부족이나 심각한 시스템 오류와 같이 애플리케이션에서 복구 불가능한 시스템 예외
- 이 예외의 경우 잡으려고 하면 안된다. 

##### [Checked Exception]

- 컴파일러가 체크를 하는 예외
- Checked Exception의 경우 `try - catch` 문으로 처리해주거나, `throws 던지려는 예외`로 처리해주어야한다. 

[장점]

- 개발자가 실수를 누락하지 않도록 컴파일러에서 잡아준다. 

[단점]

- 하위 클래스에서 던지는 모든 예외에 대해서 처리를 해주어야한다. 

- 별로 신경쓰고 싶지 않은 예외에 대해서도 처리를 해야한다. 

- 하위클래스에서 올라오는 예외에 대한 의존관계 문제점이 존재한다. 

  

##### [UnChecked Exception]

- 컴파일러가 따로 체크를 하지 않는 예외
- UnChecked Exception의 경우 따로 처리해주지 않으면, 자동으로 상위 클래스로 예외를 던져준다. 

[장점]

- 개발자가 다루고 싶지 않은 예외에 대해서는 다루지 않아도 된다. 
- 하위클래스에서 올라오는 예외에 대한 의존관계 문제점을 해결 할 수 있다. 

[단점]

- 개발자가 처리를 해야하는 예외를 누락할 수 있다. 

## 예외의 처리 

> 예외의 경우 처리할 수 있다면 처리하고, 처리할 수 없다면 던져야 한다. 
>
> 던질 경우 상위 클래스까지 예외가 올라간다. 

Controller - Service - Repository 구조가 있다고 하자. 

만약 Client가 잘못된 요청을 하게 되어 Repository에서 예외가 발생했을 때의 프로세스는 다음과 같다. 

- **Repository**가 예외를 처리하는 경우 : 더이상 예외가 발생하지 않고, 정상 프로세스로 실행된다. 
- **Repository**가 예외를 처리하지 못하는 경우 : Service 클래스로 예외를 던진다. 
- **Service**가 예외를 처리하는 경우 : 더이상 예외가 발생하지 않고, 정상 프로세스로 실행된다. 
- **Service**가 예외를 처리하지 못하는 경우 : Controller 클래스로 예외를 던진다. 
- **Controller**가 예외를 처리하는 경우 : 더이상 예외가 발생하지 않고 정상 프로세스로 실행된다. 
- **Controller**가 예외를 처리하지 못하는 경우 
  - main() thread 인 경우 : 프로세스가 종료된다. 
  - Spring .. 인 경우 : 미리 설정해둔 오류 응답 페이지나, 오류 메세지를 응답한다. 

## 예외의 의존관계 

> UnChecked Exception의 경우 예외의 의존관계에 신경을 쓰지 않아도 되지만, 
>
> Checked Exception의 경우 예외의 의존관계로 부터 자유롭지 못하다. 

##### UnChecked Exception

```java
public class Example {

    static class Controller {
        Service service = new Service();
        public void call() {
            service.call();
        }
    }

    static class Service {
        Repository repository = new Repository();

        public void call() {
            repository.call();
        }

    }
    static class Repository {
        public void call() {
            throw new IllegalStateException("ex");
        }
    }
}
```

Unchecked Exception의 경우 올라오는 예외에 대해서 별다른 조치를 취하지 않으면 상위 클래스로 예외가 자동으로 넘어가게 된다. 

그렇기 때문에 하위클래스에서 사용하는 예외에 대해 알고 싶지 않은 경우에는 무시하면 된다. 

##### Checked Exception

```java
public class Example {

    static class Controller {
        Service service = new Service();
        public void call() {
            try {
                service.call();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    static class Service {
        Repository repository = new Repository();

        public void call() throws IOException {
            repository.call();
        }

    }
    static class Repository {
        public void call() throws IOException {
            throw new IOException("problem occurred");
        }
    }
}

```

Checked Exception의 경우 처리를 해주지 않을 경우 컴파일 에러가 발생하기 때문에 처리하거나 던져야 한다. 

그렇기 때문에 예외에 대한 의존성이 생기게 된다. 

해당 코드에서 만약 Repository가 Jdbc에 관련한 예외일 경우 Service, Controller 에서 모두 Jdbc와 관련한 예외를 import 하고 있어야한다. 

만약 Jdbc에서 JPA 혹은 다른 기술로 바뀌게 된다면 관련있는 모든 코드들의 예외를 고쳐야 한다는 문제점이 발생한다. 



























