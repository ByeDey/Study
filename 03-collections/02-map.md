# Map (HashMap, TreeMap)

## 개념 요약

**Map**은 **키(Key)와 값(Value)의 쌍**으로 데이터를 저장하는 컬렉션입니다.
키는 중복을 허용하지 않으며, 키를 통해 값을 빠르게 조회할 수 있습니다.

---

## HashMap

가장 많이 사용되는 Map 구현체입니다.
내부적으로 **해시 테이블**을 사용하며 순서를 보장하지 않습니다.
`null` 키와 `null` 값을 허용합니다.

```java
import java.util.HashMap;

HashMap<String, Integer> map = new HashMap<>();

// 추가
map.put("사과", 1000);
map.put("바나나", 500);
map.put("딸기", 3000);

// 조회
System.out.println(map.get("사과"));       // 1000
System.out.println(map.get("포도"));       // null (없는 키)
System.out.println(map.getOrDefault("포도", 0));  // 0 (기본값 지정)

// 존재 여부 확인
System.out.println(map.containsKey("바나나"));    // true
System.out.println(map.containsValue(3000));      // true

// 수정 (같은 키로 put하면 덮어씀)
map.put("사과", 1200);

// 삭제
map.remove("바나나");

// 크기
System.out.println(map.size());  // 2
```

---

## Map 순회

```java
HashMap<String, Integer> map = new HashMap<>();
map.put("사과", 1000);
map.put("바나나", 500);
map.put("딸기", 3000);

// 방법 1: keySet() - 키만 순회
for (String key : map.keySet()) {
    System.out.println(key + " : " + map.get(key));
}

// 방법 2: entrySet() - 키와 값을 함께 순회 (권장)
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " : " + entry.getValue());
}

// 방법 3: values() - 값만 순회
for (int value : map.values()) {
    System.out.println(value);
}
```

---

## TreeMap

내부적으로 **레드-블랙 트리**를 사용합니다.
키를 **자동으로 정렬(오름차순)** 합니다.
삽입/삭제/조회가 HashMap보다 느리지만 정렬이 필요할 때 유용합니다.

```java
import java.util.TreeMap;

TreeMap<String, Integer> map = new TreeMap<>();
map.put("바나나", 500);
map.put("사과", 1000);
map.put("딸기", 3000);

// 키 기준 자동 정렬
System.out.println(map);  // {바나나=500, 사과=1000, 딸기=3000} (알파벳순)

System.out.println(map.firstKey());  // 첫 번째 키
System.out.println(map.lastKey());   // 마지막 키
```

---

## HashMap vs TreeMap

| 구분 | HashMap | TreeMap |
|------|---------|---------|
| 내부 구조 | 해시 테이블 | 레드-블랙 트리 |
| 순서 | 보장 안 함 | 키 기준 정렬 |
| 성능 | O(1) | O(log n) |
| null 키 | 허용 | 불허 |
| 사용 목적 | 빠른 조회 | 정렬된 키 필요 시 |

---

## 코드 예제 - 단어 빈도 카운트

```java
import java.util.HashMap;

public class WordCount {
    public static void main(String[] args) {
        String[] words = {"apple", "banana", "apple", "cherry", "banana", "apple"};

        HashMap<String, Integer> countMap = new HashMap<>();

        for (String word : words) {
            countMap.put(word, countMap.getOrDefault(word, 0) + 1);
        }

        for (Map.Entry<String, Integer> entry : countMap.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue() + "회");
        }
        // apple  : 3회
        // banana : 2회
        // cherry : 1회
    }
}
```

---

## 주의할 점

- 존재하지 않는 키로 `get()`하면 `null`을 반환한다 → `getOrDefault()` 사용 권장
- 같은 키로 `put()`하면 기존 값이 덮어씌워진다
- HashMap은 순서를 보장하지 않는다 (순서 보장이 필요하면 `LinkedHashMap` 사용)
- 기본 자료형은 직접 사용 불가 → 래퍼 클래스 사용 (`Integer`, `String` 등)

---

## 면접 단골 질문

**Q. HashMap의 동작 원리는?**
키의 `hashCode()`를 계산해 버킷(배열 인덱스)을 결정하고, 해당 버킷에 값을 저장.
해시 충돌 시 체이닝(LinkedList 또는 TreeNode) 방식으로 처리

**Q. HashMap과 TreeMap의 차이는?**
HashMap은 해시 테이블 기반으로 O(1) 성능이지만 순서 없음.
TreeMap은 레드-블랙 트리 기반으로 O(log n)이지만 키 기준 정렬 유지

**Q. HashMap과 HashTable의 차이는?**
HashTable은 동기화(thread-safe)를 지원하지만 느림. HashMap은 동기화를 지원하지 않지만 빠름.
멀티스레드 환경에서는 `ConcurrentHashMap` 사용을 권장

---

## 참고 링크

- [Java HashMap 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
- [Java TreeMap 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)
