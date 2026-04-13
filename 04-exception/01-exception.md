# 예외처리 (Exception Handling)

## 개념 요약

**예외(Exception)** 는 프로그램 실행 중에 발생하는 오류입니다.
예외처리를 통해 프로그램이 비정상적으로 종료되는 것을 방지할 수 있습니다.

---

## 오류의 종류

```
Throwable
 ├── Error          → JVM 수준의 심각한 오류 (처리 불가, ex. OutOfMemoryError)
 └── Exception
      ├── Checked Exception    → 반드시 처리해야 하는 예외 (컴파일 오류)
      └── Unchecked Exception  → 처리하지 않아도 되는 예외 (RuntimeException 하위)
```

**자주 발생하는 예외**

| 예외 | 발생 상황 |
|------|----------|
| `NullPointerException` | null 객체의 메서드/필드 접근 |
| `ArrayIndexOutOfBoundsException` | 배열 범위 초과 접근 |
| `ClassCastException` | 잘못된 타입 캐스팅 |
| `NumberFormatException` | 숫자로 변환 불가능한 문자열 변환 시 |
| `ArithmeticException` | 0으로 나누기 등 산술 오류 |
| `IOException` | 입출력 오류 (Checked) |
| `FileNotFoundException` | 파일을 찾을 수 없음 (Checked) |

---

## try-catch-finally

```java
try {
    // 예외가 발생할 수 있는 코드
} catch (예외타입 변수명) {
    // 예외 발생 시 처리할 코드
} finally {
    // 예외 발생 여부와 관계없이 항상 실행 (생략 가능)
}
```

```java
int[] arr = {1, 2, 3};

try {
    System.out.println(arr[5]);  // 예외 발생
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("인덱스 범위 초과: " + e.getMessage());
} finally {
    System.out.println("항상 실행됩니다.");
}
// 인덱스 범위 초과: Index 5 out of bounds for length 3
// 항상 실행됩니다.
```

---

## 다중 catch

여러 종류의 예외를 각각 처리할 수 있습니다.

```java
try {
    String str = null;
    System.out.println(str.length());  // NullPointerException
    int result = 10 / 0;               // ArithmeticException
} catch (NullPointerException e) {
    System.out.println("null 참조 오류: " + e.getMessage());
} catch (ArithmeticException e) {
    System.out.println("산술 오류: " + e.getMessage());
} catch (Exception e) {
    // 위에서 처리되지 않은 모든 예외 (가장 마지막에 작성)
    System.out.println("알 수 없는 오류: " + e.getMessage());
}
```

Java 7 이상에서는 `|`로 여러 예외를 하나의 catch에서 처리할 수 있습니다.

```java
} catch (NullPointerException | ArithmeticException e) {
    System.out.println("오류 발생: " + e.getMessage());
}
```

---

## throws - 예외 떠넘기기

메서드에서 발생한 예외를 직접 처리하지 않고, 호출한 쪽으로 넘길 때 사용합니다.

```java
public void readFile(String path) throws IOException {
    // 예외를 직접 처리하지 않고 호출자에게 넘김
    FileReader reader = new FileReader(path);
}

public static void main(String[] args) {
    try {
        readFile("test.txt");
    } catch (IOException e) {
        System.out.println("파일 오류: " + e.getMessage());
    }
}
```

---

## throw - 예외 직접 발생시키기

조건에 따라 개발자가 직접 예외를 던질 수 있습니다.

```java
public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("나이는 0 이상이어야 합니다: " + age);
    }
    this.age = age;
}

try {
    setAge(-1);
} catch (IllegalArgumentException e) {
    System.out.println(e.getMessage());  // 나이는 0 이상이어야 합니다: -1
}
```

---

## 사용자 정의 예외

`Exception` 또는 `RuntimeException`을 상속받아 직접 예외 클래스를 만들 수 있습니다.

```java
// 사용자 정의 예외
public class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

// 사용
public class BankAccount {
    private int balance;

    public void withdraw(int amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException("잔액 부족. 현재 잔액: " + balance);
        }
        balance -= amount;
    }
}
```

---

## 코드 예제

```java
public class ExceptionExample {
    public static int divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("0으로 나눌 수 없습니다.");
        }
        return a / b;
    }

    public static void main(String[] args) {
        // 정상 케이스
        try {
            System.out.println(divide(10, 2));  // 5
            System.out.println(divide(10, 0));  // 예외 발생
        } catch (ArithmeticException e) {
            System.out.println("오류: " + e.getMessage());
        } finally {
            System.out.println("연산 종료");
        }

        // 문자열 → 숫자 변환 예외
        String input = "abc";
        try {
            int number = Integer.parseInt(input);
        } catch (NumberFormatException e) {
            System.out.println("숫자 변환 실패: " + input);
        }
    }
}
```

---

## 주의할 점

- `catch` 블록은 **구체적인 예외부터** 작성해야 한다 (부모 예외를 먼저 쓰면 자식 예외는 도달 불가)
- `finally`는 `return`이나 예외 발생 여부와 관계없이 **항상 실행**된다
- 예외를 무조건 catch하고 아무것도 하지 않는 **빈 catch 블록**은 지양해야 한다
- Checked Exception은 반드시 `try-catch` 또는 `throws`로 처리해야 한다

---

## 면접 단골 질문

**Q. Checked Exception과 Unchecked Exception의 차이는?**
Checked Exception은 컴파일 시점에 처리를 강제하는 예외 (`IOException`, `SQLException` 등).
Unchecked Exception은 런타임에 발생하며 처리를 강제하지 않는 예외 (`NullPointerException`, `RuntimeException` 하위)

**Q. `throw`와 `throws`의 차이는?**
`throw`는 예외를 직접 발생시키는 키워드, `throws`는 메서드 선언부에서 예외를 호출자에게 넘기는 키워드

**Q. finally 블록은 언제 사용하는가?**
예외 발생 여부와 관계없이 반드시 실행해야 하는 코드(파일 닫기, DB 연결 해제 등)를 작성할 때 사용

---

## 참고 링크

- [Java 예외처리 공식 문서](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)
