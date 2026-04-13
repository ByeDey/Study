# 제네릭 (Generics)

## 개념 요약

**제네릭**은 클래스, 인터페이스, 메서드를 정의할 때 **타입을 매개변수**로 사용하는 기법입니다.
컴파일 시점에 타입을 체크하여 타입 안전성을 높이고 형 변환을 줄여줍니다.

---

## 제네릭 없이 사용할 때의 문제

```java
// Object 타입으로 모두 받으면 형 변환이 필요하고 타입 오류가 런타임에 발생
List list = new ArrayList();
list.add("문자열");
list.add(123);  // 의도치 않은 타입 추가 가능

String s = (String) list.get(1);  // 런타임 오류! ClassCastException
```

---

## 제네릭 클래스

```java
public class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}

Box<String> strBox = new Box<>();
strBox.set("Hello");
String s = strBox.get();  // 형 변환 불필요

Box<Integer> intBox = new Box<>();
intBox.set(100);
int n = intBox.get();
```

`T`는 타입 매개변수이며, 관례적으로 아래와 같이 사용합니다.

| 타입 매개변수 | 의미 |
|-------------|------|
| `T` | Type |
| `E` | Element |
| `K` | Key |
| `V` | Value |
| `N` | Number |

---

## 제네릭 메서드

```java
public class Utils {
    // 제네릭 메서드
    public static <T> void print(T value) {
        System.out.println(value);
    }

    public static <T extends Comparable<T>> T max(T a, T b) {
        return a.compareTo(b) >= 0 ? a : b;
    }
}

Utils.print("Hello");    // Hello
Utils.print(123);        // 123
Utils.max(10, 20);       // 20
Utils.max("apple", "banana");  // banana
```

---

## 와일드카드 (Wildcard)

타입 매개변수를 유연하게 지정할 때 사용합니다.

```java
// 비한정 와일드카드 - 모든 타입 허용
public void print(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}

// 상한 경계 와일드카드 - T와 T의 자식 타입만 허용
public double sum(List<? extends Number> list) {
    double total = 0;
    for (Number n : list) total += n.doubleValue();
    return total;
}

// 하한 경계 와일드카드 - T와 T의 부모 타입만 허용
public void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}
```

---

## 코드 예제

```java
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() { return key; }
    public V getValue() { return value; }

    @Override
    public String toString() {
        return "(" + key + ", " + value + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        Pair<String, Integer> pair1 = new Pair<>("나이", 25);
        Pair<String, String> pair2 = new Pair<>("이름", "홍길동");

        System.out.println(pair1);  // (나이, 25)
        System.out.println(pair2);  // (이름, 홍길동)
    }
}
```

---

## 주의할 점

- 제네릭 타입에는 **기본 자료형을 사용할 수 없다** (`int` → `Integer` 사용)
- 제네릭 타입 정보는 컴파일 후 **타입 소거(Type Erasure)** 로 제거됨 → 런타임에는 타입 정보 없음
- `new T()`, `T[]` 와 같이 제네릭 타입으로 직접 인스턴스 생성 불가

---

## 면접 단골 질문

**Q. 제네릭을 사용하는 이유는?**
컴파일 시점에 타입을 체크하여 타입 안전성을 높이고, 불필요한 형 변환을 줄이기 위해

**Q. 타입 소거(Type Erasure)란?**
컴파일 후 제네릭 타입 정보가 제거되는 것. `List<String>`과 `List<Integer>`는 런타임에 모두 `List`로 처리됨

**Q. `<? extends T>`와 `<? super T>`의 차이는?**
`extends`는 T와 T의 하위 타입만 허용(읽기 적합), `super`는 T와 T의 상위 타입만 허용(쓰기 적합)

---

## 참고 링크

- [Java 제네릭 공식 문서](https://docs.oracle.com/javase/tutorial/java/generics/index.html)
