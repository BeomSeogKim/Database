# [Java] record

> 자바 14부터 도입된 기능으로 데이터를 저장하고 접근할 수 있는 immutable 클래스를 생성하느데 사용되는 특별한 종류의 클래스로, getter, equals(),hashCode() 및 toString()메서드를 제공한다. 



## record의 목적 

데이터베이스의 값, query를 통한 값 자체를 갖고 있는 클래스를 종종 사용한다. 

많은 경우 이러한 데이터들은 immutable(불변)이기 때문에 동기화 없이 데이터의 유효성을 보장한다. 

이러한 조건을 만족하기 위해서는 다음의 조건을 따라서 class 를 생성해야한다. 

1. private, final field 사용
2. getter 사용 
3. 각 필드에 해당하는 argument가 존재하는 public 생성자
4. 두가지 인스턴스에 대해 모든 filed 값이 같을 경우 `equals()` 결과가 `true`
5. 두가지 인스턴스에 대해 모든 filed 값이 같을 경우 `hashCode()` 결과가 `true`
6. class의 이름과 각각의 field 이름을 포함한 `toString()` 제공 

```java
public class Address {
    private final String street;
    private final String zipcode;

    public Address(String street, String zipcode) {
        this.street = street;
        this.zipcode = zipcode;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Address address = (Address) o;
        return Objects.equals(street, address.street) && Objects.equals(zipcode, address.zipcode);
    }

    @Override
    public int hashCode() {
        return Objects.hash(street, zipcode);
    }

    @Override
    public String toString() {
        return "Address{" +
                "street='" + street + '\'' +
                ", zipcode='" + zipcode + '\'' +
                '}';
    }

    public String getStreet() {
        return street;
    }

    public String getZipcode() {
        return zipcode;
    }
}

```

위의 조건을 만족하며 클래스를 생성한 결과 다음과 같은 단점 존재. 

1. 다수의 상용구 코드 (boilerplate code) 존재
   - 단순한 작업의 반복 
   - 새로운 필드가 생성될 경우 equals, hashCode, toString 메서드를 재 정의 해주어야 함 
2. 클래스의 목적의 모호함
   - 필요한 필드는 street, zipcode
   - 그 외적인 코드들로 인해 클래스의 목적이 모호해짐

이러한 단점을 보완하고자 record 사용 

## record

앞서 구현한 Address 클래스를 record로 구현을 한다면 다음과 같이 생성. 

```java
public record RAddress(String street, String zipcode) {
}
```

### Constructor

record에서 자동적으로 생성자를 만들어준다. 

자체적으로 생성된 생성자의 코드는 다음과 같다. 

```java
public RAddress(String street, String zipcode) {
    this.street = street;
    this.zipcode = zipcode;
}
```

Client에서 record를 사용할 경우 다음과 같이 사용 

```java
RAddress rAddress = new RAddress("11st", "180335");
```

### Getter

자체적으로 생성된 getter는 다음과 같다. 

```java
public String street() {
    return this.street;
}

public String zipcode() {
    return this.zipcode;
}
```

***일반적인 클래스의 경우 자바빈 프로퍼티 규약에 의해 getXxx 를 사용하지만 record의 경우 필드명을 바로 사용한다.***

Client가 record를 사용할 경우 다음과 같이 사용. 

```java
String street = rAddress.street();
String zipcode = rAddress.zipcode();
```

### equals

record에 `equals` 메서드가 이미 정의되어 있다. 

모든 field에 대해서 타입과 값만 같으면 true를 반환한다. 

### hashCode

equals와 마찬가지로 모든 field에 대해서 타입과 값만 같으면 true를 반환한다. 

### toString

record는 toString 메서드 또한 제공한다. 

앞서 생성한 RAddress를 출력해보면 다음과 같은 결과를 확인할 수 있다. 

```shell
RAddress[street=11st, zipcode=180335]
```

### Static Variables & methods

record는 static 변수와 메서드를 가질 수 있다. 

```java
public record RAddress(String street, String zipcode) {
    public static String DEFAULT_STREET = "Default Street";
    public static String DEFAULT_ZIPCODE = "123456";

    public static RAddress defaultAddress() {
        return new RAddress(DEFAULT_STREET, DEFAULT_ZIPCODE);
    }
}
```

