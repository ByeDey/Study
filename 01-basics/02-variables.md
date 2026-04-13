# 변수와 자료형

## 개념 요약

**변수**는 데이터를 저장하는 메모리 공간에 붙인 이름입니다.
Java는 **정적 타입 언어**이므로 변수를 선언할 때 반드시 **자료형(타입)** 을 명시해야 합니다.

---

## 변수 선언과 초기화

```java
// 선언
int age;

// 초기화
age = 25;

// 선언과 동시에 초기화
int score = 100;
String name = "홍길동";
```

---

## 기본 자료형 (Primitive Type)

Java에는 8가지 기본 자료형이 있습니다.

**정수형**

| 자료형 | 크기 | 범위 | 예시 |
|--------|------|------|------|
| `byte` | 1byte | -128 ~ 127 | `byte b = 10;` |
| `short` | 2byte | -32,768 ~ 32,767 | `short s = 1000;` |
| `int` | 4byte | 약 -21억 ~ 21억 | `int i = 100000;` |
| `long` | 8byte | 매우 큰 수 | `long l = 100L;` |

정수는 기본적으로 `int`를 사용하고, 더 큰 수가 필요하면 `long`을 사용합니다.
`long` 타입은 숫자 뒤에 **`L`** 을 붙여야 합니다.

**실수형**

| 자료형 | 크기 | 정밀도 | 예시 |
|--------|------|--------|------|
| `float` | 4byte | 소수점 약 7자리 | `float f = 3.14f;` |
| `double` | 8byte | 소수점 약 15자리 | `double d = 3.14;` |

실수는 기본적으로 `double`을 사용합니다.
`float` 타입은 숫자 뒤에 **`f`** 를 붙여야 합니다.

**문자형 / 논리형**

| 자료형 | 크기 | 설명 | 예시 |
|--------|------|------|------|
| `char` | 2byte | 문자 하나 | `char c = 'A';` |
| `boolean` | 1byte | true / false | `boolean flag = true;` |

`char`는 **작은따옴표(`'`)** 를 사용합니다.

---

## 참조 자료형 (Reference Type)

기본 자료형을 제외한 모든 타입은 참조 자료형입니다.
값 자체가 아닌 **메모리 주소(참조)** 를 저장합니다.

**String (문자열)**

```java
String str = "Hello";

// 문자열 연결
String result = str + " World";  // "Hello World"

// 문자열 길이
int len = str.length();  // 5

// 문자열 비교 (== 가 아닌 equals() 사용!)
boolean isEqual = str.equals("Hello");  // true
```

문자열 비교는 반드시 `equals()`를 사용해야 합니다.
`==`는 주소값을 비교하기 때문에 의도한 결과가 나오지 않을 수 있습니다.

---

## 형 변환 (Type Casting)

**자동 형 변환** - 작은 타입 → 큰 타입으로 자동 변환

```java
int i = 100;
long l = i;     // 자동 변환 (int → long)
double d = i;   // 자동 변환 (int → double)
```

```
byte → short → int → long → float → double
```

**강제 형 변환** - 큰 타입 → 작은 타입으로 명시적 변환 (데이터 손실 가능)

```java
double d = 3.99;
int i = (int) d;  // 강제 변환 → i = 3 (소수점 버림)
```

---

## 변수 명명 규칙

| 규칙 | 예시 |
|------|------|
| 영문자, 숫자, `_`, `$` 사용 가능 | `myName`, `score_1` |
| 숫자로 시작 불가 | ~~`1score`~~ → `score1` |
| 대소문자 구분 | `age` ≠ `Age` |
| 카멜케이스 사용 권장 | `studentName`, `totalScore` |
| 예약어 사용 불가 | ~~`int`, `class`, `public`~~ |

---

## 코드 예제

```java
public class VariableExample {
    public static void main(String[] args) {
        int age = 20;
        double height = 175.5;
        char grade = 'A';
        boolean isStudent = true;
        String name = "홍길동";

        System.out.println("이름: " + name);
        System.out.println("나이: " + age);
        System.out.println("키: " + height);
        System.out.println("학점: " + grade);
        System.out.println("학생 여부: " + isStudent);

        // 형 변환
        double scoreDouble = age;       // 자동 형 변환 → 20.0
        int truncated = (int) 3.99;     // 강제 형 변환 → 3

        System.out.println(scoreDouble);  // 20.0
        System.out.println(truncated);    // 3
    }
}
```

---

## 주의할 점

- `long` 리터럴에는 `L` 접미사를 붙여야 한다 (`100L`)
- `float` 리터럴에는 `f` 접미사를 붙여야 한다 (`3.14f`)
- `char`는 작은따옴표(`'`), `String`은 큰따옴표(`"`) 사용
- 문자열 비교는 `equals()` 사용 (`==` 사용 금지)
- 강제 형 변환 시 데이터 손실이 발생할 수 있음

---

## 면접 단골 질문

**Q. 기본 자료형과 참조 자료형의 차이는?**
기본 자료형은 값 자체를 저장, 참조 자료형은 메모리 주소(참조)를 저장

**Q. String을 비교할 때 `==`와 `equals()`의 차이는?**
`==`는 참조(주소)를 비교, `equals()`는 실제 값(내용)을 비교

**Q. `int`와 `Integer`의 차이는?**
`int`는 기본 자료형, `Integer`는 `int`를 객체로 감싼 래퍼 클래스(Wrapper Class)

---

## 참고 링크

- [Java 기본 자료형 공식 문서](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
