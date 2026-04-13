# 배열 (Array)

## 개념 요약

**배열**은 **같은 자료형의 데이터를 연속된 공간에 저장**하는 자료구조입니다.
인덱스(index)를 통해 각 요소에 접근하며, 인덱스는 **0부터 시작**합니다.

---

## 배열 선언과 생성

```java
// 선언 후 생성
int[] arr = new int[5];

// 선언과 동시에 초기화
int[] arr2 = {10, 20, 30, 40, 50};

// new 키워드와 함께 초기화
int[] arr3 = new int[]{10, 20, 30, 40, 50};
```

배열을 생성만 하고 초기화하지 않으면 **기본값**으로 자동 초기화됩니다.

| 자료형 | 기본값 |
|--------|--------|
| `int`, `long` 등 정수형 | `0` |
| `double`, `float` 실수형 | `0.0` |
| `boolean` | `false` |
| `String`, 객체 | `null` |

---

## 배열 요소 접근

```java
int[] arr = {10, 20, 30, 40, 50};

// 읽기
System.out.println(arr[0]);  // 10 (첫 번째 요소)
System.out.println(arr[4]);  // 50 (마지막 요소)

// 쓰기
arr[2] = 99;
System.out.println(arr[2]);  // 99
```

| 인덱스 | 0 | 1 | 2 | 3 | 4 |
|--------|---|---|---|---|---|
| 값 | 10 | 20 | 30 | 40 | 50 |

---

## 배열의 길이

```java
int[] arr = {10, 20, 30, 40, 50};
System.out.println(arr.length);  // 5
```

`length`는 **필드**입니다. 괄호 없이 사용합니다. (`arr.length` O, `arr.length()` X)

---

## 배열 반복문으로 순회

```java
int[] arr = {10, 20, 30, 40, 50};

// 일반 for문
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// 향상된 for문
for (int num : arr) {
    System.out.println(num);
}
```

---

## 다차원 배열 (2D 배열)

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};

System.out.println(matrix[0][0]);  // 1
System.out.println(matrix[1][2]);  // 6

// 2중 for문으로 순회
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
// 출력:
// 1 2 3
// 4 5 6
```

---

## Arrays 클래스 주요 메서드

```java
import java.util.Arrays;

int[] arr = {5, 1, 3, 2, 4};

// 정렬
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));  // [1, 2, 3, 4, 5]

// 배열 출력
System.out.println(Arrays.toString(arr));  // [1, 2, 3, 4, 5]

// 배열 복사
int[] copy = Arrays.copyOf(arr, arr.length);

// 배열 채우기
int[] filled = new int[5];
Arrays.fill(filled, 7);
System.out.println(Arrays.toString(filled));  // [7, 7, 7, 7, 7]
```

---

## 코드 예제 - 합계와 평균

```java
public class ArrayExample {
    public static void main(String[] args) {
        int[] scores = {85, 92, 78, 95, 88};

        int sum = 0;
        int max = scores[0];
        int min = scores[0];

        for (int score : scores) {
            sum += score;
            if (score > max) max = score;
            if (score < min) min = score;
        }

        double avg = (double) sum / scores.length;

        System.out.println("합계: " + sum);    // 438
        System.out.println("평균: " + avg);    // 87.6
        System.out.println("최댓값: " + max);  // 95
        System.out.println("최솟값: " + min);  // 78
    }
}
```

---

## 주의할 점

- 인덱스는 **0부터 시작** (크기가 5면 마지막 인덱스는 4)
- 범위를 벗어난 인덱스 접근 시 `ArrayIndexOutOfBoundsException` 발생
- 배열의 크기는 **생성 후 변경 불가** (크기 변경이 필요하면 ArrayList 사용)
- `System.out.println(arr)`은 주소값이 출력됨 → `Arrays.toString()` 사용

---

## 면접 단골 질문

**Q. 배열과 ArrayList의 차이는?**
배열은 크기가 고정, ArrayList는 동적으로 크기 변경 가능

**Q. ArrayIndexOutOfBoundsException이란?**
배열의 범위를 벗어난 인덱스에 접근할 때 발생하는 런타임 예외

**Q. 배열의 기본값은?**
int → 0, double → 0.0, boolean → false, 객체 → null

---

## 참고 링크

- [Java Arrays 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html)
