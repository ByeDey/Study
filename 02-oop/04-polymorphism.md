# 다형성 (Polymorphism)

## 개념 요약

**다형성**은 하나의 타입으로 여러 종류의 객체를 다룰 수 있는 성질입니다.
부모 타입의 참조변수로 자식 객체를 참조할 수 있습니다.

---

## 업캐스팅 (Upcasting)

자식 객체를 부모 타입으로 참조하는 것입니다. 자동으로 이루어집니다.

```java
Animal dog = new Dog();   // 업캐스팅 (자동)
Animal cat = new Cat();   // 업캐스팅 (자동)

dog.sound();  // 왈왈! (Dog의 오버라이딩된 메서드 호출)
cat.sound();  // 야옹~ (Cat의 오버라이딩된 메서드 호출)
```

업캐스팅 후에는 **부모 클래스에 정의된 메서드만 호출 가능**합니다.
단, 오버라이딩된 메서드는 자식 클래스의 것이 호출됩니다.

---

## 다운캐스팅 (Downcasting)

부모 타입을 자식 타입으로 변환하는 것입니다. 명시적으로 작성해야 합니다.

```java
Animal animal = new Dog();  // 업캐스팅
Dog dog = (Dog) animal;     // 다운캐스팅 (명시적)

dog.bark();  // Dog 고유 메서드 호출 가능
```

잘못된 다운캐스팅은 `ClassCastException`을 발생시킵니다.

---

## instanceof 연산자

다운캐스팅 전에 타입을 확인할 때 사용합니다.

```java
Animal animal = new Dog();

if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
    dog.bark();
}

// Java 16 이상: 패턴 매칭으로 간결하게 작성 가능
if (animal instanceof Dog dog) {
    dog.bark();
}
```

---

## 다형성 활용 예제

```java
public class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    void sound() {
        System.out.println("...");
    }
}

public class Dog extends Animal {
    Dog(String name) { super(name); }

    @Override
    void sound() { System.out.println(name + ": 왈왈!"); }
}

public class Cat extends Animal {
    Cat(String name) { super(name); }

    @Override
    void sound() { System.out.println(name + ": 야옹~"); }
}

public class Main {
    public static void main(String[] args) {
        // 배열 하나로 여러 타입의 객체 관리
        Animal[] animals = {
            new Dog("멍멍이"),
            new Cat("나비"),
            new Dog("바둑이")
        };

        for (Animal animal : animals) {
            animal.sound();  // 각 객체의 오버라이딩된 메서드 호출
        }
        // 멍멍이: 왈왈!
        // 나비: 야옹~
        // 바둑이: 왈왈!
    }
}
```

---

## 주의할 점

- 업캐스팅 후에는 부모 클래스에 없는 자식 고유 메서드를 호출할 수 없다
- 다운캐스팅 전에는 반드시 `instanceof`로 타입을 확인하는 것이 안전하다
- 실제 객체 타입과 다른 타입으로 다운캐스팅하면 `ClassCastException` 발생

---

## 면접 단골 질문

**Q. 다형성이란 무엇인가?**
하나의 타입으로 여러 종류의 객체를 다룰 수 있는 성질. 코드의 유연성과 재사용성을 높여줌

**Q. 업캐스팅과 다운캐스팅의 차이는?**
업캐스팅은 자식 → 부모 타입으로의 자동 변환, 다운캐스팅은 부모 → 자식 타입으로의 명시적 변환

**Q. instanceof는 언제 사용하는가?**
다운캐스팅 전 객체의 실제 타입을 확인하여 `ClassCastException`을 방지할 때 사용

---

## 참고 링크

- [Java 다형성 공식 문서](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)
