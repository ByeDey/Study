# Java 소개 & JVM / JDK / JRE

## 개념 요약

Java는 **플랫폼 독립적인 객체지향 프로그래밍 언어**로, 한 번 작성하면 어디서든 실행할 수 있다는 철학(Write Once, Run Anywhere)을 가지고 있습니다.

---

## Java란?

- 1995년 Sun Microsystems(현재 Oracle)에서 만든 언어
- **객체지향(OOP)** 언어
- **정적 타입** 언어 (변수 선언 시 타입을 명시해야 함)
- 가비지 컬렉션(Garbage Collection)을 통해 메모리를 자동 관리
- 웹, 안드로이드 앱, 서버 개발 등 다양한 분야에서 활용됨

---

## JVM / JDK / JRE

**JVM (Java Virtual Machine)**
- 자바 가상 머신
- `.class` 파일(바이트코드)을 읽어서 운영체제에 맞게 실행하는 역할
- Java가 플랫폼 독립적인 이유 → JVM이 OS와 Java 사이를 중간에서 통역해주기 때문

**JRE (Java Runtime Environment)**
- 자바 실행 환경
- JVM + 자바 표준 라이브러리
- Java 프로그램을 **실행만** 할 때 필요

**JDK (Java Development Kit)**
- 자바 개발 도구
- JRE + 컴파일러(`javac`) + 디버거 등 개발 도구 포함
- Java 프로그램을 **개발할 때** 필요 → **개발자는 JDK를 설치해야 한다**

**포함 관계**

```
JDK
 └── JRE
      └── JVM
```

---

## Java 실행 과정

```
소스 코드 (.java)
    ↓  javac (컴파일)
바이트코드 (.class)
    ↓  JVM (실행)
운영체제에서 실행
```

---

## 첫 번째 Java 프로그램

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

| 코드 | 설명 |
|------|------|
| `public class HelloWorld` | HelloWorld라는 이름의 클래스 선언 |
| `public static void main(String[] args)` | 프로그램의 시작점(진입점) |
| `System.out.println(...)` | 콘솔에 출력 |

---

## 주의할 점

- 파일명은 반드시 **클래스명과 동일**해야 한다 (대소문자 구분)
- Java는 **대소문자를 구분**한다 (`String` ≠ `string`)
- 모든 문장은 **세미콜론(`;`)** 으로 끝나야 한다

---

## 면접 단골 질문

**Q. JVM, JRE, JDK의 차이는?**
JDK는 개발 도구 전체, JRE는 실행 환경, JVM은 바이트코드를 실제로 실행하는 가상 머신

**Q. Java가 플랫폼 독립적인 이유는?**
소스 코드를 바이트코드로 컴파일하고, OS별 JVM이 바이트코드를 해석해서 실행하기 때문

---

## 참고 링크

- [Oracle 공식 Java 문서](https://docs.oracle.com/en/java/)
- [Java 다운로드 (JDK)](https://www.oracle.com/java/technologies/downloads/)
