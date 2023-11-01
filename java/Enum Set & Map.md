# EnumSet, EnumMap

## EnumSet

EnumSet은 Enum을 Set에 담으려고 할 경우 사용하는 특수한 형태의 Set이다. 

EnumSet의 장점은 다음과 같다. 

1. 성능
   - EnumSet은 내부적으로 bit vector를 사용해 구현되기 때문에, 처리속도가 빠르며 메모리 사용량도 적다. 
2. 타입 안정성
   - EnumSet의 경우 Enum의 서브셋만 저장하므로 타입 안정성이 높다, 
3. Null 항목 불가능
   - EnumSet은 null을 허용하지 않기 때문에, 실수로 null을 추가하거나 조회하려고 하는 경우를 방지해준다. 

### Bit Vector 

EnumSet은 Enum 상수의 순서에 따라 비트를 할당한다. 

예를 들어 

```java
public enum Weekday {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

위와 같은 Enum이 있을 때 EnumSet을 다음과 같이 만든다고 가정하자. 

```java
EnumSet<Weekday> weekdays = EnumSet.of(Weekday.MONDAY, Weekday.WEDNESDAY);
```

weekdays에는 MONDAY, WEDNESDAY만 존재한다. 

내부적으로 EnumSet은 비트벡터로 표현된다. `Weekday`에 7개의 상수가 존재하므로 7비트의 비트 벡터가 필요하다. 

각 비트는 하나의 Enum 상수에 대응된다. 

- `MONDAY` 는 첫번째 비트에 매핑
- `WEDNESDAY` 는 세번째 비트에 매핑 

```java
0000101  (2진수)
```

위와 같이 내부적으로 표현 할 수 있다. 

## EnumMap

EnumMap은 Enum을 Map에 담으려고 할 경우 사용하는 특수한 형태의 Set이다. 

EnumMap의 장점은 다음과 같다. 

1. 성능
   - EnumMap은 내부적으로 배열을 사용해 구현되므로, 처리 속도가 빠르고 메모리 사용량도 적다.
2. 타입안정성
   - EnumMap은 key 값으로 Enum만 받으므로, 타입 안정성이 높다. 
3. 순서 보장
   - EnumMap은 키를 Enum의 정의 순서대로 저장한다.
4. Null키 불가능
   - EnumMap은 null key를 허용하지 않는다. 

### 

## 사용 예시 

[enum]

```java
public enum TrafficSignal {
    RED,
    YELLOW,
    GREEN
}
```

[EnumSet]

```java
import java.util.EnumSet;

public class EnumSetExample {

    public static void main(String[] args) {
        // EnumSet 생성
        EnumSet<TrafficSignal> signalSet = EnumSet.of(TrafficSignal.RED, TrafficSignal.GREEN);

        // 원소 추가
        signalSet.add(TrafficSignal.YELLOW);

        // 원소 삭제
        signalSet.remove(TrafficSignal.RED);

        // 원소 포함 여부 확인
        boolean hasRed = signalSet.contains(TrafficSignal.RED);  // false

        // 모든 원소 순회 및 출력
        for (TrafficSignal signal : signalSet) {
            System.out.println(signal);
        }
    }
}
```

[EnumMap]

```java
import java.util.EnumMap;

public class EnumMapExample {

    public static void main(String[] args) {
        // EnumMap 생성
        EnumMap<TrafficSignal, String> signalMap = new EnumMap<>(TrafficSignal.class);

        // 원소 추가
        signalMap.put(TrafficSignal.RED, "Stop");
        signalMap.put(TrafficSignal.YELLOW, "Get Ready");
        signalMap.put(TrafficSignal.GREEN, "Go");

        // 원소 삭제
        signalMap.remove(TrafficSignal.YELLOW);

        // 원소 검색
        String redMeaning = signalMap.get(TrafficSignal.RED);  // "Stop"

        // 모든 키와 값 순회 및 출력
        for (TrafficSignal signal : signalMap.keySet()) {
            System.out.println(signal + ": " + signalMap.get(signal));
        }
    }
}
```

