# 생성자 (Constructor)

## 개념 요약

**생성자**는 객체가 생성될 때 자동으로 호출되는 특수한 메서드입니다.
주로 필드를 초기화하는 용도로 사용합니다.

---

## 생성자의 특징

- 클래스명과 **이름이 동일**해야 한다
- **반환 타입이 없다** (void도 쓰지 않음)
- 객체 생성 시 `new` 키워드와 함께 자동 호출된다

```java
public class Car {
    String brand;
    int speed;

    // 생성자
    Car(String brand, int speed) {
        this.brand = brand;
        this.speed = speed;
    }
}

// 객체 생성 시 생성자 호출
Car car = new Car("현대", 0);
```

---

## 기본 생성자 (Default Constructor)

생성자를 직접 정의하지 않으면 컴파일러가 **기본 생성자**를 자동으로 추가합니다.

```java
public class Car {
    String brand;
    // 생성자를 정의하지 않으면 아래가 자동 추가됨
    // Car() {}
}

Car car = new Car();  // 기본 생성자 호출
```

단, 생성자를 하나라도 직접 정의하면 기본 생성자는 자동으로 추가되지 않습니다.

---

## 생성자 오버로딩

매개변수의 개수나 타입이 다른 생성자를 여러 개 정의할 수 있습니다.

```java
public class Car {
    String brand;
    int speed;
    String color;

    // 생성자 1
    Car(String brand) {
        this.brand = brand;
        this.speed = 0;
        this.color = "흰색";
    }

    // 생성자 2
    Car(String brand, int speed) {
        this.brand = brand;
        this.speed = speed;
        this.color = "흰색";
    }

    // 생성자 3
    Car(String brand, int speed, String color) {
        this.brand = brand;
        this.speed = speed;
        this.color = color;
    }
}

Car c1 = new Car("현대");
Car c2 = new Car("기아", 100);
Car c3 = new Car("BMW", 200, "검정");
```

---

## this() - 생성자 내에서 다른 생성자 호출

`this()`를 사용하면 같은 클래스의 다른 생성자를 호출할 수 있어 코드 중복을 줄일 수 있습니다.
반드시 **생성자의 첫 번째 줄**에 작성해야 합니다.

```java
public class Car {
    String brand;
    int speed;
    String color;

    Car(String brand) {
        this(brand, 0, "흰색");  // 생성자 3 호출
    }

    Car(String brand, int speed) {
        this(brand, speed, "흰색");  // 생성자 3 호출
    }

    Car(String brand, int speed, String color) {
        this.brand = brand;
        this.speed = speed;
        this.color = color;
    }
}
```

---

## 코드 예제

```java
public class Person {
    String name;
    int age;
    String job;

    Person(String name, int age) {
        this(name, age, "무직");
    }

    Person(String name, int age, String job) {
        this.name = name;
        this.age = age;
        this.job = job;
    }

    void printInfo() {
        System.out.println(name + " / " + age + "세 / " + job);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("홍길동", 20);
        Person p2 = new Person("김철수", 30, "개발자");

        p1.printInfo();  // 홍길동 / 20세 / 무직
        p2.printInfo();  // 김철수 / 30세 / 개발자
    }
}
```

---

## 주의할 점

- 생성자를 하나라도 정의하면 기본 생성자는 자동으로 추가되지 않는다
- `this()`는 반드시 생성자 첫 번째 줄에 위치해야 한다
- 생성자는 반환 타입을 쓰지 않는다 (void도 안 됨)

---

## 면접 단골 질문

**Q. 생성자와 메서드의 차이는?**
생성자는 클래스명과 같고 반환 타입이 없으며 객체 생성 시 자동 호출, 메서드는 명시적으로 호출해야 함

**Q. 기본 생성자란?**
매개변수가 없는 생성자로, 직접 정의한 생성자가 없을 때 컴파일러가 자동으로 추가해주는 생성자

**Q. 생성자 오버로딩이란?**
매개변수의 개수 또는 타입이 다른 생성자를 여러 개 정의하는 것

---

## 참고 링크

- [Java 생성자 공식 문서](https://docs.oracle.com/javase/tutorial/java/javaOO/constructors.html)
