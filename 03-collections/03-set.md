# Set (HashSet, TreeSet, LinkedHashSet)

## 개념 요약

**Set**은 **중복을 허용하지 않는** 컬렉션입니다.
순서가 없으며, 동일한 값을 여러 번 추가해도 하나만 저장됩니다.

---

## HashSet

가장 많이 사용되는 Set 구현체입니다.
내부적으로 HashMap을 사용하며 순서를 보장하지 않습니다.

```java
import java.util.HashSet;

HashSet<String> set = new HashSet<>();

// 추가
set.add("사과");
set.add("바나나");
set.add("딸기");
set.add("사과");  // 중복 → 무시됨

System.out.println(set);        // [바나나, 딸기, 사과] (순서 랜덤)
System.out.println(set.size()); // 3 (중복 제거)

// 포함 여부 확인
System.out.println(set.contains("사과"));    // true
System.out.println(set.contains("포도"));    // false

// 삭제
set.remove("바나나");

// 순회
for (String item : set) {
    System.out.println(item);
}
```

---

## TreeSet

내부적으로 **레드-블랙 트리**를 사용합니다.
요소를 **자동으로 정렬(오름차순)** 합니다.

```java
import java.util.TreeSet;

TreeSet<Integer> set = new TreeSet<>();
set.add(5);
set.add(1);
set.add(3);
set.add(2);
set.add(4);

System.out.println(set);          // [1, 2, 3, 4, 5] (자동 정렬)
System.out.println(set.first());  // 1 (최솟값)
System.out.println(set.last());   // 5 (최댓값)
```

---

## LinkedHashSet

삽입 순서를 보장하는 Set입니다.

```java
import java.util.LinkedHashSet;

LinkedHashSet<String> set = new LinkedHashSet<>();
set.add("바나나");
set.add("사과");
set.add("딸기");

System.out.println(set);  // [바나나, 사과, 딸기] (삽입 순서 유지)
```

---

## Set 종류 비교

| 구분 | HashSet | TreeSet | LinkedHashSet |
|------|---------|---------|---------------|
| 내부 구조 | 해시 테이블 | 레드-블랙 트리 | 해시 테이블 + 연결 리스트 |
| 순서 | 보장 안 함 | 정렬 순서 | 삽입 순서 |
| 성능 | O(1) | O(log n) | O(1) |
| null 허용 | O | X | O |

---

## Set 활용 - 중복 제거

```java
import java.util.ArrayList;
import java.util.HashSet;

ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(2);
list.add(3);
list.add(3);
list.add(3);

// List → Set으로 중복 제거
HashSet<Integer> set = new HashSet<>(list);
System.out.println(set);  // [1, 2, 3]

// 다시 List로 변환
ArrayList<Integer> deduped = new ArrayList<>(set);
```

---

## Set 집합 연산

```java
HashSet<Integer> setA = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
HashSet<Integer> setB = new HashSet<>(Arrays.asList(3, 4, 5, 6, 7));

// 합집합
HashSet<Integer> union = new HashSet<>(setA);
union.addAll(setB);
System.out.println("합집합: " + union);  // [1, 2, 3, 4, 5, 6, 7]

// 교집합
HashSet<Integer> intersection = new HashSet<>(setA);
intersection.retainAll(setB);
System.out.println("교집합: " + intersection);  // [3, 4, 5]

// 차집합
HashSet<Integer> difference = new HashSet<>(setA);
difference.removeAll(setB);
System.out.println("차집합: " + difference);  // [1, 2]
```

---

## 주의할 점

- Set은 인덱스로 접근하는 `get(index)` 메서드가 없다
- 순서가 필요하면 `LinkedHashSet`, 정렬이 필요하면 `TreeSet` 사용
- 커스텀 객체를 저장할 때 중복 판단을 위해 `equals()`와 `hashCode()`를 재정의해야 한다

---

## 면접 단골 질문

**Q. Set과 List의 차이는?**
List는 순서 있고 중복 허용, Set은 순서 없고 중복 불허

**Q. HashSet에서 중복 여부를 판단하는 방법은?**
`hashCode()`로 버킷 위치를 찾고, `equals()`로 실제 값을 비교하여 중복 판단

**Q. HashSet, TreeSet, LinkedHashSet의 차이는?**
HashSet은 순서 없음, TreeSet은 정렬 순서 유지, LinkedHashSet은 삽입 순서 유지

---

## 참고 링크

- [Java HashSet 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)
- [Java TreeSet 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)
