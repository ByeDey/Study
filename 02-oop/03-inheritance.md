# 상속 (Inheritance)

## 개념 요약

**상속**은 부모 클래스의 필드와 메서드를 자식 클래스가 물려받는 것입니다.
코드 재사용성을 높이고 계층 구조를 표현할 때 사용합니다.

---

## 상속 기본 문법

`extends` 키워드를 사용합니다.

```java
// 부모 클래스 (슈퍼 클래스)
public class Animal {
    String name;

    void eat() {
        System.out.println(name + "이(가) 먹는다.");
    }
}

// 자식 클래스 (서브 클래스)
public class Dog extends Animal {
    void bark() {
        System.out.println(name + "이(가) 짖는다.");
    }
}

Dog dog = new Dog();
dog.name = "멍멍이";
dog.eat();   // 멍멍이이(가) 먹는다. (부모 메서드)
dog.bark();  // 멍멍이이(가) 짖는다. (자식 메서드)
```

Java는 **단일 상속**만 지원합니다 (부모 클래스는 하나만 지정 가능).

---

## 메서드 오버라이딩 (Method Overriding)

부모 클래스의 메서드를 자식 클래스에서 **재정의**하는 것입니다.
`@Override` 어노테이션을 붙여 명시적으로 표시합니다.

```java
public class Animal {
    void sound() {
        System.out.println("...");
    }
}

public class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("왈왈!");
    }
}

public class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("야옹~");
    }
}

Animal dog = new Dog();
Animal cat = new Cat();

dog.sound();  // 왈왈!
cat.sound();  // 야옹~
```

---

## super 키워드

`super`는 부모 클래스를 참조하는 키워드입니다.

```java
public class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    void info() {
        System.out.println("동물 이름: " + name);
    }
}

public class Dog extends Animal {
    String breed;

    Dog(String name, String breed) {
        super(name);  // 부모 생성자 호출 (반드시 첫 줄)
        this.breed = breed;
    }

    @Override
    void info() {
        super.info();  // 부모 메서드 호출
        System.out.println("견종: " + breed);
    }
}

Dog dog = new Dog("멍멍이", "골든 리트리버");
dog.info();
// 동물 이름: 멍멍이
// 견종: 골든 리트리버
```

---

## 상속과 접근제어자

| 접근제어자 | 같은 클래스 | 같은 패키지 | 자식 클래스 | 외부 |
|-----------|------------|------------|------------|------|
| `public` | O | O | O | O |
| `protected` | O | O | O | X |
| (default) | O | O | X | X |
| `private` | O | X | X | X |

`private` 멤버는 자식 클래스에서 직접 접근할 수 없습니다.

---

## final 클래스와 메서드

- `final` 클래스 - 상속 불가
- `final` 메서드 - 오버라이딩 불가

```java
public final class String { ... }     // 상속 불가

public class Animal {
    final void breathe() { ... }      // 오버라이딩 불가
}
```

---

## 코드 예제

```java
public class Shape {
    String color;

    Shape(String color) {
        this.color = color;
    }

    double area() {
        return 0;
    }

    void printInfo() {
        System.out.println("색상: " + color + ", 넓이: " + area());
    }
}

public class Circle extends Shape {
    double radius;

    Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    double width, height;

    Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }

    @Override
    double area() {
        return width * height;
    }
}

public class Main {
    public static void main(String[] args) {
        Circle c = new Circle("빨강", 5);
        Rectangle r = new Rectangle("파랑", 4, 6);

        c.printInfo();  // 색상: 빨강, 넓이: 78.53...
        r.printInfo();  // 색상: 파랑, 넓이: 24.0
    }
}
```

---

## 주의할 점

- Java는 **단일 상속**만 지원 (부모 클래스는 하나만 가능)
- `super()`는 자식 생성자의 **첫 번째 줄**에 작성해야 한다
- `private` 멤버는 상속되지만 자식 클래스에서 직접 접근 불가
- `@Override`는 생략 가능하지만 명시하는 것이 권장됨

---

## 면접 단골 질문

**Q. 오버로딩(Overloading)과 오버라이딩(Overriding)의 차이는?**
오버로딩은 같은 클래스 내에서 메서드명이 같고 매개변수가 다른 메서드를 여러 개 정의하는 것,
오버라이딩은 부모 클래스의 메서드를 자식 클래스에서 재정의하는 것

**Q. Java가 다중 상속을 지원하지 않는 이유는?**
다이아몬드 문제(두 부모에 같은 메서드가 있을 때 어느 것을 상속받을지 모호해지는 문제)를 방지하기 위해

**Q. `super`와 `this`의 차이는?**
`this`는 현재 객체를 참조, `super`는 부모 클래스를 참조

---

## 참고 링크

- [Java 상속 공식 문서](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
