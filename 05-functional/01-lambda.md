# 람다식 (Lambda Expression)

## 개념 요약

**람다식**은 메서드를 하나의 식으로 간결하게 표현하는 방법입니다.
함수형 인터페이스(추상 메서드가 하나인 인터페이스)를 구현할 때 사용합니다.

---

## 기본 문법

```java
// 기존 방식 (익명 클래스)
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("실행");
    }
};

// 람다식
Runnable r = () -> System.out.println("실행");
```

```
(매개변수) -> { 실행 코드 }
```

매개변수가 하나면 괄호 생략 가능, 실행 코드가 한 줄이면 중괄호와 `return` 생략 가능합니다.

```java
// 매개변수 없음
() -> System.out.println("Hello")

// 매개변수 하나 (괄호 생략 가능)
x -> x * x

// 매개변수 여러 개
(x, y) -> x + y

// 여러 줄
(x, y) -> {
    int result = x + y;
    return result;
}
```

---

## 함수형 인터페이스

추상 메서드가 **정확히 하나**인 인터페이스를 함수형 인터페이스라고 합니다.
`@FunctionalInterface` 어노테이션으로 명시합니다.

```java
@FunctionalInterface
public interface Calculator {
    int calculate(int a, int b);
}

Calculator add = (a, b) -> a + b;
Calculator multiply = (a, b) -> a * b;

System.out.println(add.calculate(3, 5));       // 8
System.out.println(multiply.calculate(3, 5));  // 15
```

---

## Java 기본 제공 함수형 인터페이스

자주 사용하는 함수형 인터페이스는 `java.util.function` 패키지에 이미 정의되어 있습니다.

| 인터페이스 | 메서드 | 설명 |
|-----------|--------|------|
| `Runnable` | `run()` | 매개변수 없음, 반환 없음 |
| `Supplier<T>` | `get()` | 매개변수 없음, T 반환 |
| `Consumer<T>` | `accept(T)` | T 받음, 반환 없음 |
| `Function<T, R>` | `apply(T)` | T 받아 R 반환 |
| `Predicate<T>` | `test(T)` | T 받아 boolean 반환 |
| `BiFunction<T, U, R>` | `apply(T, U)` | T, U 받아 R 반환 |

```java
import java.util.function.*;

// Supplier - 값을 공급
Supplier<String> supplier = () -> "Hello";
System.out.println(supplier.get());  // Hello

// Consumer - 값을 소비
Consumer<String> consumer = s -> System.out.println(s.toUpperCase());
consumer.accept("hello");  // HELLO

// Function - 값을 변환
Function<String, Integer> function = s -> s.length();
System.out.println(function.apply("hello"));  // 5

// Predicate - 조건 판단
Predicate<Integer> isPositive = n -> n > 0;
System.out.println(isPositive.test(5));   // true
System.out.println(isPositive.test(-1));  // false
```

---

## 메서드 참조 (Method Reference)

람다식을 더 간결하게 표현하는 방법입니다.

```java
// 람다식
Function<String, Integer> f1 = s -> Integer.parseInt(s);

// 메서드 참조
Function<String, Integer> f2 = Integer::parseInt;

// 종류
클래스::정적메서드      // Integer::parseInt
객체::인스턴스메서드   // System.out::println
클래스::인스턴스메서드 // String::length
클래스::new            // ArrayList::new (생성자 참조)
```

```java
List<String> names = Arrays.asList("홍길동", "김철수", "이영희");

// 람다식
names.forEach(name -> System.out.println(name));

// 메서드 참조
names.forEach(System.out::println);
```

---

## 코드 예제

```java
import java.util.*;
import java.util.function.*;

public class LambdaExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Predicate - 짝수 필터
        Predicate<Integer> isEven = n -> n % 2 == 0;

        // Consumer - 출력
        Consumer<Integer> print = n -> System.out.print(n + " ");

        // Function - 제곱
        Function<Integer, Integer> square = n -> n * n;

        numbers.stream()
               .filter(isEven)
               .map(square)
               .forEach(print);
        // 4 16 36 64 100
    }
}
```

---

## 주의할 점

- 람다식에서 외부 변수를 사용할 때 해당 변수는 **effectively final** 이어야 한다 (변경 불가)
- 함수형 인터페이스는 추상 메서드가 **정확히 하나**여야 한다
- 람다식은 익명 클래스와 달리 자체적인 `this`를 가지지 않는다

---

## 면접 단골 질문

**Q. 람다식이란 무엇인가?**
함수형 인터페이스를 간결하게 구현하는 방법으로, 메서드를 하나의 식으로 표현한 것

**Q. 함수형 인터페이스란?**
추상 메서드가 정확히 하나인 인터페이스. `@FunctionalInterface` 어노테이션으로 명시

**Q. 람다식과 익명 클래스의 차이는?**
익명 클래스는 자체적인 스코프와 `this`를 가지지만, 람다식은 감싸는 클래스의 `this`를 사용

---

## 참고 링크

- [Java 람다식 공식 문서](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
- [java.util.function 패키지](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
