# 인터페이스 & 추상클래스 (Interface & Abstract Class)

## 개념 요약

**추상클래스**와 **인터페이스**는 모두 직접 객체를 생성할 수 없고,
다른 클래스가 이를 구현하도록 강제하는 역할을 합니다.

---

## 추상클래스 (Abstract Class)

`abstract` 키워드를 사용합니다.
구현부가 없는 **추상 메서드**와 구현된 **일반 메서드**를 함께 가질 수 있습니다.

```java
public abstract class Animal {
    String name;

    // 추상 메서드 - 구현부 없음, 자식 클래스에서 반드시 구현해야 함
    abstract void sound();

    // 일반 메서드 - 구현부 있음
    void breathe() {
        System.out.println(name + "이(가) 숨을 쉰다.");
    }
}

public class Dog extends Animal {
    Dog(String name) {
        this.name = name;
    }

    @Override
    void sound() {  // 추상 메서드 반드시 구현
        System.out.println(name + ": 왈왈!");
    }
}

// Animal a = new Animal();  // 오류 - 추상클래스는 직접 생성 불가
Dog dog = new Dog("멍멍이");
dog.sound();    // 멍멍이: 왈왈!
dog.breathe();  // 멍멍이이(가) 숨을 쉰다.
```

---

## 인터페이스 (Interface)

`interface` 키워드를 사용합니다.
모든 메서드가 기본적으로 `public abstract`이며, 모든 필드는 `public static final`입니다.
**다중 구현**이 가능합니다.

```java
public interface Flyable {
    void fly();  // public abstract 생략 가능
}

public interface Swimmable {
    void swim();
}

// 인터페이스 다중 구현
public class Duck extends Animal implements Flyable, Swimmable {
    Duck(String name) {
        this.name = name;
    }

    @Override
    void sound() {
        System.out.println(name + ": 꽥꽥!");
    }

    @Override
    public void fly() {
        System.out.println(name + "이(가) 난다.");
    }

    @Override
    public void swim() {
        System.out.println(name + "이(가) 수영한다.");
    }
}

Duck duck = new Duck("오리");
duck.sound();  // 오리: 꽥꽥!
duck.fly();    // 오리이(가) 난다.
duck.swim();   // 오리이(가) 수영한다.
```

---

## default 메서드 (Java 8 이상)

인터페이스에서도 구현부가 있는 메서드를 정의할 수 있습니다.
인터페이스를 구현한 클래스에서 오버라이딩할 수 있습니다.

```java
public interface Flyable {
    void fly();

    default void land() {
        System.out.println("착륙합니다.");
    }
}
```

---

## 추상클래스 vs 인터페이스

| 구분 | 추상클래스 | 인터페이스 |
|------|-----------|-----------|
| 키워드 | `abstract class` | `interface` |
| 상속/구현 | `extends` (단일) | `implements` (다중) |
| 메서드 | 추상 + 일반 메서드 가능 | 추상 메서드 (+ default) |
| 필드 | 일반 필드 가능 | `public static final` 상수만 |
| 생성자 | 있음 (직접 생성은 불가) | 없음 |
| 사용 목적 | **공통 기능**을 공유할 때 | **기능 명세(계약)**를 정의할 때 |

---

## 코드 예제

```java
// 추상클래스: 공통 속성과 기능 정의
public abstract class Shape {
    String color;

    Shape(String color) {
        this.color = color;
    }

    abstract double area();  // 자식마다 다르게 구현

    void printInfo() {
        System.out.println("색상: " + color + ", 넓이: " + area());
    }
}

// 인터페이스: 기능 명세
public interface Drawable {
    void draw();
}

public class Circle extends Shape implements Drawable {
    double radius;

    Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }

    @Override
    public void draw() {
        System.out.println("원을 그립니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Circle c = new Circle("빨강", 5);
        c.printInfo();  // 색상: 빨강, 넓이: 78.53...
        c.draw();       // 원을 그립니다.
    }
}
```

---

## 주의할 점

- 추상클래스와 인터페이스 모두 직접 객체 생성 불가
- 추상 메서드가 하나라도 있으면 클래스에 `abstract`를 붙여야 한다
- 인터페이스를 구현한 클래스는 모든 추상 메서드를 구현해야 한다
- 인터페이스는 `extends`가 아닌 `implements`로 구현한다

---

## 면접 단골 질문

**Q. 추상클래스와 인터페이스의 차이는?**
추상클래스는 공통 기능을 공유할 목적으로 단일 상속, 일반 메서드와 필드를 가질 수 있음.
인터페이스는 기능 명세를 정의하며 다중 구현 가능, 상수와 추상 메서드만 가질 수 있음 (Java 8부터 default 메서드 추가)

**Q. 인터페이스를 사용하는 이유는?**
다중 구현을 통해 유연한 설계가 가능하고, 클래스 간 결합도를 낮춰 유지보수가 쉬워짐

**Q. 추상클래스는 언제 사용하는가?**
공통 속성과 기능을 여러 자식 클래스가 공유해야 할 때, IS-A 관계가 명확할 때 사용

---

## 참고 링크

- [Java 추상클래스 공식 문서](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Java 인터페이스 공식 문서](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)
