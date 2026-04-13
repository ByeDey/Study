# List (ArrayList, LinkedList)

## 개념 요약

**List**는 순서가 있고 중복을 허용하는 컬렉션입니다.
배열과 달리 크기가 동적으로 변합니다.

---

## ArrayList

내부적으로 **배열**을 사용합니다.
인덱스로 빠르게 접근할 수 있지만, 중간 삽입/삭제가 느립니다.

```java
import java.util.ArrayList;

ArrayList<String> list = new ArrayList<>();

// 추가
list.add("사과");
list.add("바나나");
list.add("딸기");
list.add(1, "포도");  // 인덱스 1 위치에 삽입

// 조회
System.out.println(list.get(0));    // 사과
System.out.println(list.size());    // 4
System.out.println(list.contains("딸기"));  // true

// 수정
list.set(0, "망고");

// 삭제
list.remove(0);         // 인덱스로 삭제
list.remove("바나나");  // 값으로 삭제

// 순회
for (String fruit : list) {
    System.out.println(fruit);
}
```

---

## LinkedList

내부적으로 **노드(Node)** 를 연결하는 구조입니다.
중간 삽입/삭제가 빠르지만, 인덱스 접근이 느립니다.
`Queue`나 `Deque`로도 사용할 수 있습니다.

```java
import java.util.LinkedList;

LinkedList<String> list = new LinkedList<>();

list.add("A");
list.add("B");
list.addFirst("Z");  // 맨 앞에 추가
list.addLast("C");   // 맨 뒤에 추가

System.out.println(list.getFirst());  // Z
System.out.println(list.getLast());   // C

list.removeFirst();
list.removeLast();
```

---

## ArrayList vs LinkedList

| 구분 | ArrayList | LinkedList |
|------|-----------|------------|
| 내부 구조 | 배열 | 연결 노드 |
| 인덱스 접근 | 빠름 O(1) | 느림 O(n) |
| 중간 삽입/삭제 | 느림 O(n) | 빠름 O(1) |
| 메모리 | 적게 사용 | 많이 사용 (노드 포인터) |
| 주 사용 목적 | 조회가 많을 때 | 삽입/삭제가 많을 때 |

일반적인 경우에는 **ArrayList**를 사용하는 것이 권장됩니다.

---

## List 주요 메서드 정리

| 메서드 | 설명 |
|--------|------|
| `add(value)` | 맨 뒤에 추가 |
| `add(index, value)` | 특정 위치에 추가 |
| `get(index)` | 특정 위치 값 조회 |
| `set(index, value)` | 특정 위치 값 수정 |
| `remove(index)` | 특정 위치 값 삭제 |
| `remove(Object)` | 특정 값 삭제 |
| `size()` | 크기 반환 |
| `contains(value)` | 포함 여부 확인 |
| `indexOf(value)` | 값의 인덱스 반환 |
| `isEmpty()` | 비어있는지 확인 |
| `clear()` | 전체 삭제 |

---

## 코드 예제 - 학생 점수 관리

```java
import java.util.ArrayList;
import java.util.Collections;

public class ScoreManager {
    public static void main(String[] args) {
        ArrayList<Integer> scores = new ArrayList<>();

        scores.add(85);
        scores.add(92);
        scores.add(78);
        scores.add(95);
        scores.add(88);

        System.out.println("전체 점수: " + scores);
        System.out.println("학생 수: " + scores.size());

        // 최댓값, 최솟값
        System.out.println("최고점: " + Collections.max(scores));
        System.out.println("최저점: " + Collections.min(scores));

        // 정렬
        Collections.sort(scores);
        System.out.println("정렬 후: " + scores);

        // 합계와 평균
        int sum = 0;
        for (int score : scores) sum += score;
        System.out.println("평균: " + (double) sum / scores.size());
    }
}
```

---

## 주의할 점

- `ArrayList<int>`는 불가 → 기본 자료형 대신 **래퍼 클래스** 사용 (`Integer`, `Double` 등)
- `remove(int index)`와 `remove(Object o)`를 혼동하지 않도록 주의
- 인덱스 범위를 벗어나면 `IndexOutOfBoundsException` 발생

---

## 면접 단골 질문

**Q. ArrayList와 배열의 차이는?**
배열은 크기가 고정, ArrayList는 동적으로 크기가 변함. ArrayList는 기본 자료형을 직접 저장할 수 없음

**Q. ArrayList와 LinkedList의 차이는?**
ArrayList는 배열 기반으로 인덱스 접근이 빠르고, LinkedList는 노드 연결 구조로 삽입/삭제가 빠름

**Q. ArrayList의 시간복잡도는?**
인덱스 접근 O(1), 맨 뒤 추가 O(1), 중간 삽입/삭제 O(n)

---

## 참고 링크

- [Java ArrayList 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)
- [Java LinkedList 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)
