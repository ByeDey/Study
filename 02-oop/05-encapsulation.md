# 캡슐화 & 접근제어자 (Encapsulation & Access Modifiers)

## 개념 요약

**캡슐화**는 객체의 데이터(필드)를 외부에서 직접 접근하지 못하게 숨기고,
메서드를 통해서만 접근하도록 하는 것입니다.
**정보 은닉**을 통해 객체의 무결성을 보호합니다.

---

## 접근제어자 (Access Modifiers)

| 접근제어자 | 같은 클래스 | 같은 패키지 | 자식 클래스 | 외부 |
|-----------|------------|------------|------------|------|
| `public` | O | O | O | O |
| `protected` | O | O | O | X |
| (default) | O | O | X | X |
| `private` | O | X | X | X |

---

## 캡슐화 구현 - getter / setter

필드를 `private`으로 선언하고, `public` 메서드(getter/setter)를 통해 접근합니다.

```java
public class Person {
    private String name;  // 외부 직접 접근 불가
    private int age;

    // getter - 값 읽기
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    // setter - 값 변경 (유효성 검사 가능)
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        if (age < 0) {
            System.out.println("나이는 0 이상이어야 합니다.");
            return;
        }
        this.age = age;
    }
}

Person p = new Person();
p.setName("홍길동");
p.setAge(-1);   // 나이는 0 이상이어야 합니다.
p.setAge(25);

System.out.println(p.getName());  // 홍길동
System.out.println(p.getAge());   // 25
```

setter에 유효성 검사를 추가하면 잘못된 값이 들어오는 것을 방지할 수 있습니다.

---

## getter / setter 네이밍 규칙

```java
private String name;
private boolean active;

// getter
public String getName() { return name; }
public boolean isActive() { return active; }  // boolean은 is로 시작

// setter
public void setName(String name) { this.name = name; }
public void setActive(boolean active) { this.active = active; }
```

`boolean` 타입의 getter는 `get` 대신 `is`로 시작합니다.

---

## 코드 예제

```java
public class BankAccount {
    private String owner;
    private int balance;

    public BankAccount(String owner, int initialBalance) {
        this.owner = owner;
        this.balance = initialBalance;
    }

    public String getOwner() { return owner; }
    public int getBalance() { return balance; }

    public void deposit(int amount) {
        if (amount <= 0) {
            System.out.println("입금액은 0보다 커야 합니다.");
            return;
        }
        balance += amount;
        System.out.println(amount + "원 입금. 잔액: " + balance);
    }

    public void withdraw(int amount) {
        if (amount > balance) {
            System.out.println("잔액이 부족합니다.");
            return;
        }
        balance -= amount;
        System.out.println(amount + "원 출금. 잔액: " + balance);
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("홍길동", 10000);

        account.deposit(5000);   // 5000원 입금. 잔액: 15000
        account.withdraw(3000);  // 3000원 출금. 잔액: 12000
        account.withdraw(20000); // 잔액이 부족합니다.

        // account.balance = 999999;  // 컴파일 오류 (private)
    }
}
```

---

## 주의할 점

- 필드는 원칙적으로 `private`으로 선언하는 것이 좋다
- setter 없이 getter만 제공하면 **읽기 전용** 필드를 만들 수 있다
- `public` 필드는 외부에서 자유롭게 수정 가능하므로 무결성 보장이 어렵다

---

## 면접 단골 질문

**Q. 캡슐화란 무엇이고 왜 사용하는가?**
객체의 데이터를 외부로부터 숨기고 메서드를 통해서만 접근하게 하는 것.
데이터 무결성 보호, 유지보수성 향상, 잘못된 값 입력 방지를 위해 사용

**Q. getter/setter를 사용하는 이유는?**
필드를 직접 노출하지 않고 메서드를 통해 접근하여 유효성 검사나 부가 로직을 추가할 수 있기 때문

**Q. `private`과 `protected`의 차이는?**
`private`은 같은 클래스 내에서만 접근 가능, `protected`는 같은 패키지와 자식 클래스에서도 접근 가능

---

## 참고 링크

- [Java 접근제어자 공식 문서](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)
