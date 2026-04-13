# 디자인 패턴 (Design Patterns)

## 개념 요약

**디자인 패턴**은 소프트웨어 개발에서 자주 발생하는 문제들에 대한 **검증된 해결책**입니다.
크게 생성, 구조, 행위 패턴으로 분류됩니다.

---

## 패턴 분류

| 분류 | 설명 | 주요 패턴 |
|------|------|----------|
| 생성 (Creational) | 객체 생성 방식 | Singleton, Factory, Builder |
| 구조 (Structural) | 클래스/객체 조합 구조 | Adapter, Decorator, Proxy |
| 행위 (Behavioral) | 객체 간 상호작용 | Observer, Strategy, Template Method |

---

## Singleton 패턴

**인스턴스를 오직 하나만** 생성하고, 어디서든 같은 인스턴스에 접근하도록 합니다.

```java
public class DatabaseConnection {
    private static DatabaseConnection instance;

    // 외부에서 생성자 직접 호출 불가
    private DatabaseConnection() {}

    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }

    public void connect() {
        System.out.println("DB 연결");
    }
}

// 사용
DatabaseConnection db1 = DatabaseConnection.getInstance();
DatabaseConnection db2 = DatabaseConnection.getInstance();
System.out.println(db1 == db2);  // true (같은 인스턴스)
```

멀티스레드 환경에서 안전한 Singleton:

```java
public static synchronized DatabaseConnection getInstance() {
    if (instance == null) {
        instance = new DatabaseConnection();
    }
    return instance;
}
```

---

## Factory 패턴

객체 생성 로직을 별도의 클래스(Factory)에 위임하여, **어떤 객체를 생성할지 결정**합니다.

```java
public abstract class Animal {
    abstract void sound();
}

public class Dog extends Animal {
    @Override
    public void sound() { System.out.println("왈왈!"); }
}

public class Cat extends Animal {
    @Override
    public void sound() { System.out.println("야옹~"); }
}

// Factory 클래스
public class AnimalFactory {
    public static Animal create(String type) {
        switch (type) {
            case "dog": return new Dog();
            case "cat": return new Cat();
            default: throw new IllegalArgumentException("알 수 없는 타입: " + type);
        }
    }
}

// 사용 - 구체적인 클래스를 몰라도 됨
Animal dog = AnimalFactory.create("dog");
Animal cat = AnimalFactory.create("cat");
dog.sound();  // 왈왈!
cat.sound();  // 야옹~
```

---

## Builder 패턴

**복잡한 객체 생성 과정**을 단계적으로 분리하여 다양한 설정으로 객체를 만들 수 있게 합니다.

```java
public class Person {
    private String name;
    private int age;
    private String email;
    private String phone;

    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }

    public static class Builder {
        private String name;  // 필수
        private int age;      // 필수
        private String email = "";  // 선택
        private String phone = "";  // 선택

        public Builder(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public Person build() {
            return new Person(this);
        }
    }
}

// 사용
Person p = new Person.Builder("홍길동", 25)
                     .email("hong@email.com")
                     .phone("010-1234-5678")
                     .build();
```

---

## Observer 패턴

한 객체의 상태 변화가 발생하면 **의존하는 모든 객체에 자동으로 알림**을 보내는 패턴입니다.

```java
import java.util.*;

// 관찰 대상 (Subject)
public class EventSource {
    private List<Observer> observers = new ArrayList<>();
    private String event;

    public void addObserver(Observer o) { observers.add(o); }
    public void removeObserver(Observer o) { observers.remove(o); }

    public void setEvent(String event) {
        this.event = event;
        notifyObservers();
    }

    private void notifyObservers() {
        for (Observer o : observers) o.update(event);
    }
}

// 관찰자 (Observer)
public interface Observer {
    void update(String event);
}

public class Logger implements Observer {
    @Override
    public void update(String event) {
        System.out.println("[LOG] 이벤트 발생: " + event);
    }
}

// 사용
EventSource source = new EventSource();
source.addObserver(new Logger());
source.setEvent("클릭");  // [LOG] 이벤트 발생: 클릭
```

---

## Strategy 패턴

알고리즘을 **인터페이스로 분리**하여 런타임에 교체할 수 있도록 합니다.

```java
// 전략 인터페이스
public interface SortStrategy {
    void sort(int[] arr);
}

public class BubbleSort implements SortStrategy {
    @Override
    public void sort(int[] arr) {
        System.out.println("버블 정렬 실행");
    }
}

public class QuickSort implements SortStrategy {
    @Override
    public void sort(int[] arr) {
        System.out.println("퀵 정렬 실행");
    }
}

// Context
public class Sorter {
    private SortStrategy strategy;

    public Sorter(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void sort(int[] arr) {
        strategy.sort(arr);
    }
}

// 사용
int[] arr = {5, 3, 1, 4, 2};
Sorter sorter = new Sorter(new BubbleSort());
sorter.sort(arr);  // 버블 정렬 실행

sorter.setStrategy(new QuickSort());  // 전략 교체
sorter.sort(arr);  // 퀵 정렬 실행
```

---

## 주의할 점

- Singleton은 멀티스레드 환경에서 동기화 처리가 필요하다
- 패턴을 억지로 적용하면 오히려 코드가 복잡해질 수 있다
- 패턴의 이름보다 **어떤 문제를 해결하는지** 이해하는 것이 중요하다

---

## 면접 단골 질문

**Q. Singleton 패턴이란?**
인스턴스를 하나만 생성하고 전역에서 접근할 수 있도록 하는 패턴. 생성자를 private으로 막고 static 메서드로 인스턴스를 반환

**Q. Factory 패턴을 사용하는 이유는?**
객체 생성 로직을 분리하여 클라이언트가 구체적인 클래스를 몰라도 되게 하고, 새로운 타입 추가 시 클라이언트 코드 변경을 최소화하기 위해

**Q. Strategy 패턴과 Template Method 패턴의 차이는?**
Strategy는 알고리즘을 인터페이스로 분리해 객체 주입으로 교체, Template Method는 상위 클래스에 알고리즘 골격을 정의하고 자식 클래스가 일부를 오버라이딩

---

## 참고 링크

- [Refactoring Guru - Design Patterns](https://refactoring.guru/design-patterns)
- [GoF 디자인 패턴 위키](https://en.wikipedia.org/wiki/Design_Patterns)
