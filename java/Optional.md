# [JAVA] Optional 

 자바 프로그래밍에서 Null Pointer Exception을 종종 접한다. 

이럴 때 체크 할 수 있는 방법

- throw Exception 
  - 단점 : Expensive (스택트레이스를 찍어둔다.)
- return null 
  - 단점 : 해당 코드를 사용하는 클라이언트 코드가 주의해야 함. 
- Optional
  - 명시적으로 빈 값일 수도 있음을 알려준다.
  - 빈 값인 경우 처리를 강제한다. 



Optional은 `Java8`부터 지원하는 기능.

Optional은 오직 값 한개가 들어있을 수도 없을 수도 있는 컨테이너이다. 

![image-20230709223008500](/Users/github/TIL/java/images/optional1.png)

## Optional 사용 시 주의사항 

- return 값으로만 사용하자. 
  - 메서드 매개변수 타입, 맵의 키 타입, 인스턴스 필드 타입으로 사용하지 말자. 
    - 매개변수에 null이 넘어 갈 수 있으므로 결국에는 검증을 해 줘야 한다. 
    - 맵의 인터페이스 특징을 깨트림 -> (맵의 경우 key값은 null 일 수 없다.)
- Optional을 return 하는 메서드에서 null return 금지. 
- primitive 타입의 경우 primitive 타입 전용 Optional을 사용하자.(OptionalInt, Optional Long ... )
  - Optional.of를 사용할 경우 Boxing / UnBoxing이 일어난다. (성능 저하)
- Collection, Map, Stream Array, Optional은 Optional로 감싸지 말 것. 



## Optional API 

### Optional 생성 

#### Optional.of()

```java
Member member = new Member();
Optional<Member> optionalMember = Optional.of(member);

// Optional Class // 
public static <T> Optional<T> of(T value) {
	return new Optional<>(Objects.requireNonNull(value));
}
public static <T> T requireNonNull(T obj) {
    if (obj == null)
        throw new NullPointerException();
    return obj;
}
```

인스턴스를 Optional로 감싸주는 방식. 

인스턴스가 null이 아님을 확신할 때 Optional.of()을 사용한다. 

Optional.of에 null이 들어갈 경우 NullPointerException 발생 



#### Optional.ofNullable()

```java
Member member = new Member();
Optional<Member> optionalMember = Optional.ofNullable(member);
```

인스턴스가 null일 수도 있는 경우 Optional.ofNullable()사용. 



### Optional 값 여부 확인 

#### Optional.isPresent()

```java
Optional<Member> optionalMember = Optional.ofNullable(member);
boolean booleanPresent = optionalMember.isPresent();
```

boolean 값을 return.

 Optional 값이 들어 있으면 true를 return 함.



#### Optional.isEmpty()

isPresent()의 반대 역할. 



### Optional 값 가져오기 

#### Optional.get()

```java
Member member = new Member();
Optional<Member> optionalMember = Optional.ofNullable(member);
Member member1 = optionalMember.get();
```

Optional.get()을 사용해서 값을 가져올 수 있다.

다만 권장하는 방식은 아님. null 값이 존재 할 경우 문제가 발생한다. 



#### Optional.ifPresent()

```java
public void ifPresent(Consumer<? super T> action) {
  if (value != null) {
      action.accept(value);
  }
}
```

만약 값이 있는 경우 Consumer를 사용해 처리를 할 수 있다. 



#### Optional.orElse()

```java
public T orElse(T other) {
	return value != null ? value : other;
}
```

만약 값이 있으면 그대로 가져오고 아닐 경우 T를 반환 

***이때 Functional Interface를 반환하는 것이 아닌 Instance를 반환해야 한다.***



#### Optional.orElseGet()

```java
public T orElseGet(Supplier<? extends T> supplier) {
  return value != null ? value : supplier.get();
}
```

만약 값이 있으면 가져오고 없는 경우 새로 만들어서 return 



#### Optional.orElseThrow()

```java
public T orElseThrow() {
    if (value == null) {
        throw new NoSuchElementException("No value present");
    }
    return value;
}
```

만약 값이 있으면 가져오고, 값이 없다면 Exception을 던진다. 



#### Optional.filter()

```java
public Optional<T> filter(Predicate<? super T> predicate) {
    Objects.requireNonNull(predicate);
    if (!isPresent()) {
        return this;
    } else {
        return predicate.test(value) ? this : empty();
    }
}
```

Optional에 들어있는 값들을 특정 조건에 따라 걸러낸다.



### Optional에 있는 값 변환 

#### Optional.map(Function)

```java
public <U> Optional<U> map(Function<? super T, ? extends U> mapper) {
    Objects.requireNonNull(mapper);
    if (!isPresent()) {
        return empty();
    } else {
        return Optional.ofNullable(mapper.apply(value));
    }
}
```



#### Optional.flatMap(Function)

```java
public <U> Optional<U> flatMap(Function<? super T, ? extends Optional<? extends U>> mapper) {
    Objects.requireNonNull(mapper);
    if (!isPresent()) {
        return empty();
    } else {
        @SuppressWarnings("unchecked")
        Optional<U> r = (Optional<U>) mapper.apply(value);
        return Objects.requireNonNull(r);
    }
}

```

Optional로 두번 감싸진 경우 Optional값을 한번 벗겨준다. 