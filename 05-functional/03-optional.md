# Optional

## 개념 요약

**Optional**은 값이 있을 수도, 없을 수도 있는 상황을 명시적으로 표현하는 래퍼 클래스입니다.
`NullPointerException`을 방지하고 null 처리를 안전하게 하기 위해 사용합니다.

---

## Optional 생성

```java
import java.util.Optional;

// 값이 있는 Optional
Optional<String> opt1 = Optional.of("Hello");

// null을 허용하는 Optional
Optional<String> opt2 = Optional.ofNullable(null);  // 빈 Optional 생성
Optional<String> opt3 = Optional.ofNullable("Hi");  // 값 있는 Optional 생성

// 빈 Optional
Optional<String> opt4 = Optional.empty();
```

`Optional.of()`에 null을 전달하면 `NullPointerException`이 발생하므로,
null 가능성이 있으면 `Optional.ofNullable()`을 사용합니다.

---

## 값 확인 및 꺼내기

```java
Optional<String> opt = Optional.ofNullable("Hello");

// 값 존재 여부 확인
opt.isPresent();  // true (값 있음)
opt.isEmpty();    // false (Java 11 이상)

// 값 꺼내기
opt.get();  // "Hello" (값이 없으면 NoSuchElementException 발생)

// 안전하게 꺼내기
opt.orElse("기본값");               // 값이 없으면 기본값 반환
opt.orElseGet(() -> "기본값");      // 값이 없으면 람다 실행 후 반환
opt.orElseThrow(() -> new RuntimeException("값 없음"));  // 값이 없으면 예외 발생
```

---

## 값 변환 및 필터링

```java
Optional<String> opt = Optional.of("hello");

// map - 값 변환
Optional<Integer> length = opt.map(s -> s.length());
System.out.println(length.orElse(0));  // 5

// filter - 조건 필터링
Optional<String> filtered = opt.filter(s -> s.startsWith("h"));
System.out.println(filtered.orElse("없음"));  // hello

// ifPresent - 값이 있을 때만 실행
opt.ifPresent(s -> System.out.println(s.toUpperCase()));  // HELLO
```

---

## 기존 null 처리 방식 vs Optional

**기존 방식 - null 체크**

```java
public String getUserEmail(User user) {
    if (user != null) {
        Address address = user.getAddress();
        if (address != null) {
            return address.getEmail();
        }
    }
    return "이메일 없음";
}
```

**Optional 사용**

```java
public String getUserEmail(User user) {
    return Optional.ofNullable(user)
                   .map(User::getAddress)
                   .map(Address::getEmail)
                   .orElse("이메일 없음");
}
```

---

## 코드 예제

```java
import java.util.*;

public class OptionalExample {
    static Optional<String> findUserById(int id) {
        Map<Integer, String> users = new HashMap<>();
        users.put(1, "홍길동");
        users.put(2, "김철수");

        return Optional.ofNullable(users.get(id));
    }

    public static void main(String[] args) {
        // 존재하는 유저
        String user1 = findUserById(1).orElse("없는 유저");
        System.out.println(user1);  // 홍길동

        // 존재하지 않는 유저
        String user2 = findUserById(99).orElse("없는 유저");
        System.out.println(user2);  // 없는 유저

        // 값이 있을 때만 처리
        findUserById(2).ifPresent(name ->
            System.out.println(name + "님 환영합니다.")
        );  // 김철수님 환영합니다.

        // map으로 변환
        int nameLength = findUserById(1)
                            .map(String::length)
                            .orElse(0);
        System.out.println(nameLength);  // 3
    }
}
```

---

## 주의할 점

- `Optional.get()`은 값이 없으면 예외가 발생하므로 단독 사용은 지양
- Optional은 **반환 타입**으로 사용하는 것이 목적 - 필드 타입이나 매개변수 타입으로는 권장하지 않음
- `isPresent()` + `get()` 조합보다 `orElse()`, `ifPresent()` 사용이 더 권장됨
- Optional 자체도 객체이므로 과도하게 사용하면 성능에 영향을 줄 수 있음

---

## 면접 단골 질문

**Q. Optional을 사용하는 이유는?**
null 반환으로 인한 NullPointerException을 방지하고, 값의 존재 여부를 명시적으로 표현하기 위해

**Q. `orElse()`와 `orElseGet()`의 차이는?**
`orElse()`는 값의 존재 여부와 관계없이 항상 인자를 평가, `orElseGet()`은 값이 없을 때만 람다를 실행. 인자 생성 비용이 크면 `orElseGet()` 사용 권장

**Q. Optional을 필드 타입으로 사용하면 안 되는 이유는?**
Optional은 직렬화를 지원하지 않고, 메서드 반환 타입으로 null 처리를 명시하는 목적으로 설계되었기 때문

---

## 참고 링크

- [Java Optional 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)
