# ☕ Java Study

> Java 이론을 체계적으로 정리한 학습 저장소입니다.

---

## 📚 커리큘럼 목차

### 1단계 - 기초 (Basics)

| # | 주제 | 파일 |
|---|------|------|
| 01 | Java 소개 & JVM / JDK / JRE | [01-java-intro.md](./01-basics/01-java-intro.md) |
| 02 | 변수와 자료형 | [02-variables.md](./01-basics/02-variables.md) |
| 03 | 연산자 | [03-operators.md](./01-basics/03-operators.md) |
| 04 | 제어문 (if, switch, for, while) | [04-control-flow.md](./01-basics/04-control-flow.md) |
| 05 | 배열 | [05-arrays.md](./01-basics/05-arrays.md) |

---

### 2단계 - 객체지향 프로그래밍 (OOP)

| # | 주제 | 파일 |
|---|------|------|
| 01 | 클래스와 객체 | [01-class-object.md](./02-oop/01-class-object.md) |
| 02 | 생성자 | [02-constructor.md](./02-oop/02-constructor.md) |
| 03 | 상속 | [03-inheritance.md](./02-oop/03-inheritance.md) |
| 04 | 다형성 | [04-polymorphism.md](./02-oop/04-polymorphism.md) |
| 05 | 캡슐화 & 접근제어자 | [05-encapsulation.md](./02-oop/05-encapsulation.md) |
| 06 | 인터페이스 & 추상클래스 | [06-interface.md](./02-oop/06-interface.md) |

---

### 3단계 - 컬렉션 & 예외처리 (Collections & Exception)

| # | 주제 | 파일 |
|---|------|------|
| 01 | List (ArrayList, LinkedList) | [01-list.md](./03-collections/01-list.md) |
| 02 | Map (HashMap, TreeMap) | [02-map.md](./03-collections/02-map.md) |
| 03 | Set (HashSet) | [03-set.md](./03-collections/03-set.md) |
| 04 | 예외처리 (try-catch) | [01-exception.md](./04-exception/01-exception.md) |

---

### 4단계 - 함수형 프로그래밍 (Functional)

| # | 주제 | 파일 |
|---|------|------|
| 01 | 람다식 (Lambda) | [01-lambda.md](./05-functional/01-lambda.md) |
| 02 | 스트림 API (Stream) | [02-stream.md](./05-functional/02-stream.md) |
| 03 | Optional | [03-optional.md](./05-functional/03-optional.md) |

---

### 5단계 - 고급 (Advanced)

| # | 주제 | 파일 |
|---|------|------|
| 01 | 제네릭 (Generics) | [01-generics.md](./06-advanced/01-generics.md) |
| 02 | 멀티스레드 (Multithreading) | [02-multithreading.md](./06-advanced/02-multithreading.md) |
| 03 | JVM 구조 & 메모리 | [03-jvm-memory.md](./06-advanced/03-jvm-memory.md) |
| 04 | 디자인 패턴 (Design Patterns) | [04-design-patterns.md](./06-advanced/04-design-patterns.md) |

---

## 📁 폴더 구조

```
Study/
├── README.md
├── 01-basics/
│   ├── 01-java-intro.md
│   ├── 02-variables.md
│   ├── 03-operators.md
│   ├── 04-control-flow.md
│   └── 05-arrays.md
├── 02-oop/
│   ├── 01-class-object.md
│   ├── 02-constructor.md
│   ├── 03-inheritance.md
│   ├── 04-polymorphism.md
│   ├── 05-encapsulation.md
│   └── 06-interface.md
├── 03-collections/
│   ├── 01-list.md
│   ├── 02-map.md
│   └── 03-set.md
├── 04-exception/
│   └── 01-exception.md
├── 05-functional/
│   ├── 01-lambda.md
│   ├── 02-stream.md
│   └── 03-optional.md
└── 06-advanced/
    ├── 01-generics.md
    ├── 02-multithreading.md
    ├── 03-jvm-memory.md
    └── 04-design-patterns.md
```

---

## 📝 각 문서 구성

모든 문서는 아래의 통일된 형식으로 작성되어 있습니다.

- 📌 **개념 요약** - 핵심 내용 한 줄 정리
- 🔍 **상세 설명** - 개념 및 동작 원리
- 💻 **코드 예제** - 직접 실행 가능한 예제 코드
- ⚠️ **주의할 점** - 실수하기 쉬운 포인트
- ❓ **면접 단골 질문** - 취업/이직 대비 질문 정리
- 📎 **참고 링크** - 공식 문서 및 참고 자료
