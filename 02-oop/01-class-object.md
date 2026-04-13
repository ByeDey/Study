# 클래스와 객체 (Class & Object)

## 개념 요약

**클래스**는 객체를 만들기 위한 설계도입니다.
**객체**는 클래스를 기반으로 실제 메모리에 생성된 인스턴스입니다.

---

## 클래스와 객체의 관계

| 구분 | 설명 | 예시 |
|------|------|------|
| 클래스 | 설계도 | `자동차` 설계도 |
| 객체 | 설계도로 만든 실체 | 실제 `자동차` |

하나의 클래스로 여러 개의 객체를 만들 수 있습니다.

---

## 클래스 구성 요소

```java
public class Car {
    // 필드 (속성)
    String brand;
    int speed;

    // 메서드 (행동)
    void accelerate(int amount) {
        speed += amount;
    }

    void printInfo() {
        System.out.println("브랜드: " + brand + ", 속도: " + speed);
    }
}
```

- **필드(Field)** - 객체의 상태(속성)를 저장하는 변수
- **메서드(Method)** - 객체의 행동(기능)을 정의하는 함수

---

## 객체 생성과 사용

```java
public class Main {
    public static void main(String[] args) {
        // 객체 생성 (new 키워드 사용)
        Car car1 = new Car();
        Car car2 = new Car();

        // 필드 접근
        car1.brand = "현대";
        car1.speed = 0;

        car2.brand = "기아";
        car2.speed = 0;

        // 메서드 호출
        car1.accelerate(50);
        car1.printInfo();  // 브랜드: 현대, 속도: 50

        car2.accelerate(80);
        car2.printInfo();  // 브랜드: 기아, 속도: 80
    }
}
```

---

## this 키워드

`this`는 현재 객체 자신을 가리킵니다.
필드명과 매개변수명이 같을 때 구분하기 위해 사용합니다.

```java
public class Car {
    String brand;
    int speed;

    void setBrand(String brand) {
        this.brand = brand;  // this.brand = 필드, brand = 매개변수
    }
}
```

---

## static 키워드

`static`이 붙은 필드와 메서드는 객체를 생성하지 않고 **클래스 이름으로 직접 접근**할 수 있습니다.

```java
public class MathUtil {
    static int add(int a, int b) {
        return a + b;
    }
}

// 객체 생성 없이 바로 호출
int result = MathUtil.add(3, 5);  // 8
```

| 구분 | 접근 방법 | 메모리 |
|------|----------|--------|
| 인스턴스 필드/메서드 | 객체를 통해 접근 | 객체마다 별도 생성 |
| static 필드/메서드 | 클래스명으로 접근 | 모든 객체가 공유 |

---

## 코드 예제

```java
public class Student {
    String name;
    int age;
    double grade;

    void printInfo() {
        System.out.println("이름: " + name + ", 나이: " + age + ", 학점: " + grade);
    }

    boolean isPassed() {
        return grade >= 2.0;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student();
        s1.name = "홍길동";
        s1.age = 20;
        s1.grade = 3.5;

        Student s2 = new Student();
        s2.name = "김철수";
        s2.age = 22;
        s2.grade = 1.5;

        s1.printInfo();  // 이름: 홍길동, 나이: 20, 학점: 3.5
        s2.printInfo();  // 이름: 김철수, 나이: 22, 학점: 1.5

        System.out.println(s1.isPassed());  // true
        System.out.println(s2.isPassed());  // false
    }
}
```

---

## 주의할 점

- 객체 생성 없이 인스턴스 필드/메서드에 접근하면 컴파일 오류 발생
- `static` 메서드 내에서는 `this` 사용 불가
- 객체를 선언만 하고 생성하지 않으면 `null` 상태 → `NullPointerException` 발생 가능

---

## 면접 단골 질문

**Q. 클래스와 객체의 차이는?**
클래스는 객체를 만들기 위한 설계도, 객체는 클래스를 기반으로 실제 메모리에 생성된 인스턴스

**Q. 인스턴스 변수와 static 변수의 차이는?**
인스턴스 변수는 객체마다 독립적으로 생성, static 변수는 모든 객체가 공유

**Q. `this`란 무엇인가?**
현재 객체 자신을 참조하는 키워드로, 필드와 매개변수명이 같을 때 구분하기 위해 사용

---

## 참고 링크

- [Java 클래스 공식 문서](https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html)
