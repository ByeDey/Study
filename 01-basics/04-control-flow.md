# 제어문 (Control Flow)

## 개념 요약

**제어문**은 프로그램의 실행 흐름을 제어하는 문장입니다.
크게 **조건문**과 **반복문**으로 나뉩니다.

---

## 조건문

**if 문**

조건이 `true`일 때만 실행됩니다.

```java
int score = 85;

if (score >= 90) {
    System.out.println("A학점");
} else if (score >= 80) {
    System.out.println("B학점");
} else if (score >= 70) {
    System.out.println("C학점");
} else {
    System.out.println("F학점");
}
// 출력: B학점
```

**switch 문**

하나의 변수가 특정 값과 일치할 때 실행됩니다.
`if-else if`가 많아질 때 더 깔끔하게 작성할 수 있습니다.

```java
int day = 3;

switch (day) {
    case 1:
        System.out.println("월요일");
        break;
    case 2:
        System.out.println("화요일");
        break;
    case 3:
        System.out.println("수요일");
        break;
    default:
        System.out.println("기타");
        break;
}
// 출력: 수요일
```

`break`를 빠뜨리면 다음 `case`까지 실행되는 **fall-through** 현상이 발생합니다.

Java 14 이상에서는 화살표 문법도 사용할 수 있습니다.

```java
switch (day) {
    case 1 -> System.out.println("월요일");
    case 2 -> System.out.println("화요일");
    case 3 -> System.out.println("수요일");
    default -> System.out.println("기타");
}
```

---

## 반복문

**for 문** - 횟수가 정해진 반복에 적합

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
// 출력: 1 2 3 4 5
```

**while 문** - 조건이 true인 동안 반복, 횟수가 불명확할 때 적합

```java
int i = 1;

while (i <= 5) {
    System.out.println(i);
    i++;
}
```

**do-while 문** - 최소 한 번은 실행 후 조건 확인

```java
int i = 1;

do {
    System.out.println(i);
    i++;
} while (i <= 5);
```

| 반복문 | 언제 사용? |
|--------|-----------|
| `for` | 반복 횟수를 알 때 |
| `while` | 조건이 true인 동안 반복 |
| `do-while` | 최소 1번은 실행해야 할 때 |

---

## 향상된 for문 (for-each)

배열이나 컬렉션의 요소를 순서대로 꺼낼 때 사용합니다.

```java
int[] numbers = {10, 20, 30, 40, 50};

for (int num : numbers) {
    System.out.println(num);
}
```

---

## break와 continue

**break** - 반복문을 즉시 종료

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) break;
    System.out.println(i);
}
// 출력: 1 2 3 4
```

**continue** - 현재 반복을 건너뛰고 다음 반복으로 진행

```java
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) continue;  // 짝수 건너뜀
    System.out.println(i);
}
// 출력: 1 3 5 7 9
```

---

## 코드 예제 - 구구단

```java
public class MultiplicationTable {
    public static void main(String[] args) {
        for (int i = 2; i <= 9; i++) {
            for (int j = 1; j <= 9; j++) {
                System.out.println(i + " x " + j + " = " + (i * j));
            }
        }
    }
}
```

---

## 주의할 점

- `switch`에서 `break`를 빠뜨리면 다음 case도 실행됨 (fall-through)
- `while(true)` 무한루프 사용 시 반드시 탈출 조건 필요
- `for-each`는 인덱스 접근이나 요소 수정 불가 → 그럴 땐 일반 `for` 사용
- 이중 `for`문에서 `break`는 안쪽 반복문만 탈출함

---

## 면접 단골 질문

**Q. for문과 while문의 차이는?**
for문은 반복 횟수가 명확할 때, while문은 조건 기반의 반복에 적합

**Q. break와 continue의 차이는?**
break는 반복문 전체 종료, continue는 현재 반복만 건너뛰고 다음으로 진행

**Q. switch에서 break를 안 쓰면 어떻게 되나?**
fall-through 현상이 발생해 다음 case의 코드까지 실행됨

---

## 참고 링크

- [Java 제어문 공식 문서](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/flow.html)
