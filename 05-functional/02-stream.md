# 스트림 API (Stream API)

## 개념 요약

**Stream**은 컬렉션이나 배열의 요소를 **함수형 스타일로 처리**하는 API입니다.
데이터를 필터링, 변환, 집계하는 작업을 간결하게 표현할 수 있습니다.

---

## Stream의 특징

- **원본 데이터를 변경하지 않는다** - 새로운 Stream을 생성하여 처리
- **일회용** - 한 번 사용한 Stream은 재사용 불가
- **지연 연산** - 최종 연산이 호출되기 전까지 중간 연산은 실행되지 않음
- **내부 반복** - for문 없이 내부적으로 반복 처리

---

## Stream 생성

```java
import java.util.stream.*;

// 컬렉션에서 생성
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream1 = list.stream();

// 배열에서 생성
int[] arr = {1, 2, 3, 4, 5};
IntStream stream2 = Arrays.stream(arr);

// 직접 생성
Stream<String> stream3 = Stream.of("x", "y", "z");

// 범위로 생성
IntStream stream4 = IntStream.range(1, 6);    // 1~5
IntStream stream5 = IntStream.rangeClosed(1, 5);  // 1~5
```

---

## 중간 연산 (Intermediate Operations)

Stream을 변환하며, **여러 개를 연결**할 수 있습니다. 최종 연산 전까지 실행되지 않습니다.

**filter** - 조건에 맞는 요소만 걸러냄

```java
list.stream()
    .filter(s -> s.startsWith("a"))
```

**map** - 요소를 다른 값으로 변환

```java
list.stream()
    .map(s -> s.toUpperCase())
```

**sorted** - 정렬

```java
list.stream()
    .sorted()                          // 기본 정렬
    .sorted(Comparator.reverseOrder()) // 역순 정렬
```

**distinct** - 중복 제거

```java
list.stream().distinct()
```

**limit / skip** - 개수 제한 / 건너뜀

```java
list.stream().limit(3)   // 앞에서 3개만
list.stream().skip(2)    // 앞에서 2개 건너뜀
```

---

## 최종 연산 (Terminal Operations)

Stream을 소비하고 결과를 반환합니다. **한 번만 호출 가능**합니다.

**forEach** - 각 요소를 처리

```java
list.stream().forEach(System.out::println);
```

**collect** - 결과를 컬렉션으로 수집

```java
List<String> result = list.stream()
                          .filter(s -> s.length() > 3)
                          .collect(Collectors.toList());
```

**count / sum / average / max / min**

```java
long count = list.stream().count();
int sum    = intList.stream().mapToInt(Integer::intValue).sum();
double avg = intList.stream().mapToInt(Integer::intValue).average().getAsDouble();
```

**reduce** - 요소를 하나로 합침

```java
int total = IntStream.rangeClosed(1, 10)
                     .reduce(0, (a, b) -> a + b);  // 55
```

**anyMatch / allMatch / noneMatch** - 조건 판단

```java
boolean anyEven  = list.stream().anyMatch(n -> n % 2 == 0);   // 하나라도 짝수?
boolean allEven  = list.stream().allMatch(n -> n % 2 == 0);   // 모두 짝수?
boolean noneEven = list.stream().noneMatch(n -> n % 2 == 0);  // 짝수 없음?
```

---

## 코드 예제

```java
import java.util.*;
import java.util.stream.*;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // 짝수만 골라서 제곱한 뒤 합산
        int result = numbers.stream()
                            .filter(n -> n % 2 == 0)
                            .map(n -> n * n)
                            .reduce(0, Integer::sum);
        System.out.println(result);  // 4+16+36+64+100 = 220

        // 문자열 처리
        List<String> names = Arrays.asList("홍길동", "김철수", "이영희", "박민준");

        List<String> filtered = names.stream()
                                     .filter(name -> name.startsWith("김") || name.startsWith("이"))
                                     .sorted()
                                     .collect(Collectors.toList());
        System.out.println(filtered);  // [김철수, 이영희]

        // 통계
        IntSummaryStatistics stats = numbers.stream()
                                            .mapToInt(Integer::intValue)
                                            .summaryStatistics();
        System.out.println("합계: " + stats.getSum());
        System.out.println("평균: " + stats.getAverage());
        System.out.println("최댓값: " + stats.getMax());
    }
}
```

---

## 주의할 점

- Stream은 **일회용** - 한 번 최종 연산을 수행하면 재사용 불가
- 원본 컬렉션은 **변경되지 않는다**
- 무한 Stream 사용 시 반드시 `limit()`으로 크기를 제한해야 한다
- 성능이 중요한 경우 단순 반복은 for문이 Stream보다 빠를 수 있다

---

## 면접 단골 질문

**Q. Stream과 Collection의 차이는?**
Collection은 데이터를 저장하는 자료구조, Stream은 데이터를 처리하는 파이프라인. Stream은 데이터를 저장하지 않고 원본을 변경하지 않음

**Q. 중간 연산과 최종 연산의 차이는?**
중간 연산은 Stream을 반환하며 지연 실행, 최종 연산은 결과를 반환하며 Stream을 소비

**Q. 병렬 Stream이란?**
`parallelStream()`으로 생성하며 내부적으로 Fork/Join 프레임워크를 사용해 멀티스레드로 처리. 데이터가 많을 때 성능 향상 가능

---

## 참고 링크

- [Java Stream 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)
- [Java Stream 튜토리얼](https://docs.oracle.com/javase/tutorial/collections/streams/index.html)
