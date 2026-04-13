# JVM 구조 & 메모리 (JVM Internals & Memory)

## 개념 요약

**JVM(Java Virtual Machine)** 은 Java 바이트코드를 실행하는 가상 머신입니다.
JVM의 구조와 메모리 영역을 이해하면 성능 최적화와 메모리 관련 오류를 다루는 데 도움이 됩니다.

---

## JVM 구조

```
┌──────────────────────────────────────┐
│            Class Loader              │  .class 파일 로드
├──────────────────────────────────────┤
│           Runtime Data Area          │  메모리 영역
│  ┌────────┬──────┬────────────────┐  │
│  │ Method │ Heap │ Stack / PC 등  │  │
│  │  Area  │      │                │  │
│  └────────┴──────┴────────────────┘  │
├──────────────────────────────────────┤
│          Execution Engine            │  바이트코드 실행
│  ┌─────────────┬────────────────┐   │
│  │ Interpreter │   JIT Compiler │   │
│  └─────────────┴────────────────┘   │
└──────────────────────────────────────┘
```

---

## 메모리 영역 (Runtime Data Area)

**Method Area (메서드 영역)**
- 클래스 정보, static 변수, 상수, 메서드 코드 저장
- 모든 스레드가 공유
- JVM 시작 시 생성, 종료 시 해제

**Heap (힙)**
- `new`로 생성된 **객체와 배열** 저장
- 모든 스레드가 공유
- **Garbage Collection**의 대상 영역

**Stack (스택)**
- 메서드 호출 시 생성되는 **스택 프레임** 저장 (지역 변수, 매개변수, 반환 주소)
- 스레드마다 독립적으로 생성
- 메서드 종료 시 스택 프레임 자동 제거

**PC Register**
- 현재 실행 중인 명령어의 주소 저장
- 스레드마다 독립적으로 생성

**Native Method Stack**
- Java가 아닌 네이티브(C/C++) 메서드 실행을 위한 스택

---

## Heap 메모리 구조

GC 최적화를 위해 Heap은 세대별로 나뉩니다.

```
Heap
 ├── Young Generation  (새로 생성된 객체)
 │    ├── Eden         (객체 최초 생성)
 │    ├── Survivor 0
 │    └── Survivor 1
 └── Old Generation    (오래된 객체)
```

- **Young Generation** - 새로 생성된 객체가 위치, Minor GC 발생 영역
- **Old Generation** - Young에서 살아남은 객체가 이동, Major GC(Full GC) 발생 영역

---

## 가비지 컬렉션 (Garbage Collection)

더 이상 참조되지 않는 객체를 자동으로 메모리에서 제거하는 과정입니다.

```java
String s = new String("Hello");  // Heap에 객체 생성
s = null;  // 참조 해제 → GC 대상이 됨
```

**GC 동작 과정**

1. Eden 영역에 객체 생성
2. Eden이 가득 차면 **Minor GC** 발생
3. 살아남은 객체는 Survivor 영역으로 이동
4. 여러 번 살아남은 객체는 **Old Generation**으로 이동
5. Old Generation이 가득 차면 **Major GC(Full GC)** 발생

**주요 GC 종류**

| GC | 특징 |
|----|------|
| Serial GC | 단일 스레드, 소규모 애플리케이션 |
| Parallel GC | 멀티 스레드, Java 8 기본 |
| G1 GC | 균등 분할 방식, Java 9 이상 기본 |
| ZGC | 초저지연, Java 15 이상 |

---

## 클래스 로더 (Class Loader)

`.class` 파일을 읽어 JVM 메모리에 로드합니다.

| 클래스 로더 | 역할 |
|-----------|------|
| Bootstrap Class Loader | `java.lang` 등 핵심 Java 라이브러리 로드 |
| Extension Class Loader | `lib/ext` 디렉토리의 라이브러리 로드 |
| Application Class Loader | 개발자가 작성한 클래스 로드 |

---

## JIT 컴파일러 (Just-In-Time Compiler)

자주 실행되는 바이트코드를 **네이티브 코드로 미리 변환**하여 실행 속도를 높입니다.
인터프리터는 매번 바이트코드를 해석하지만, JIT는 변환된 네이티브 코드를 재사용합니다.

---

## 주의할 점

- `static` 변수는 Method Area에 저장되어 GC 대상이 아니므로 과도하게 사용하면 메모리 누수 가능
- `OutOfMemoryError`는 Heap이 꽉 찼을 때 발생
- `StackOverflowError`는 재귀 호출이 너무 깊어 Stack이 꽉 찼을 때 발생
- `System.gc()`로 GC를 직접 호출할 수 있지만 실제 실행은 JVM이 결정

---

## 면접 단골 질문

**Q. JVM 메모리 구조를 설명하세요.**
Method Area(클래스 정보, static), Heap(객체), Stack(지역변수, 스택 프레임), PC Register, Native Method Stack으로 구성

**Q. Heap과 Stack의 차이는?**
Heap은 객체가 저장되며 모든 스레드가 공유, Stack은 메서드 호출 정보와 지역 변수가 저장되며 스레드마다 독립적

**Q. 가비지 컬렉션이란?**
JVM이 더 이상 참조되지 않는 Heap 영역의 객체를 자동으로 메모리에서 제거하는 과정

**Q. `OutOfMemoryError`와 `StackOverflowError`의 차이는?**
OutOfMemoryError는 Heap 공간 부족, StackOverflowError는 Stack 공간 부족 (주로 무한 재귀)

---

## 참고 링크

- [Java JVM 공식 명세](https://docs.oracle.com/javase/specs/jvms/se8/html/index.html)
- [Java GC 튜닝 가이드](https://docs.oracle.com/en/java/javase/11/gctuning/index.html)
