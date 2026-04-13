# 멀티스레드 (Multithreading)

## 개념 요약

**스레드(Thread)** 는 프로세스 내에서 실행되는 작업의 단위입니다.
**멀티스레드**는 하나의 프로세스에서 여러 스레드가 동시에 실행되는 것입니다.

---

## 프로세스 vs 스레드

| 구분 | 프로세스 | 스레드 |
|------|---------|--------|
| 정의 | 실행 중인 프로그램 | 프로세스 내 실행 단위 |
| 메모리 | 독립적인 메모리 공간 | 같은 메모리 공간 공유 |
| 생성 비용 | 높음 | 낮음 |
| 통신 | IPC 필요 | 공유 메모리로 직접 통신 |

---

## 스레드 생성 방법

**방법 1: Thread 클래스 상속**

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(getName() + ": " + i);
        }
    }
}

MyThread t = new MyThread();
t.start();  // run()을 직접 호출하면 안 됨, start()로 호출
```

**방법 2: Runnable 인터페이스 구현 (권장)**

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + ": " + i);
        }
    }
}

Thread t = new Thread(new MyRunnable());
t.start();

// 람다식으로 간결하게
Thread t2 = new Thread(() -> {
    for (int i = 0; i < 5; i++) {
        System.out.println(i);
    }
});
t2.start();
```

Runnable을 권장하는 이유 - Java는 단일 상속만 지원하므로 Thread를 상속하면 다른 클래스를 상속할 수 없습니다.

---

## 스레드 상태

```
NEW → RUNNABLE → RUNNING → TERMINATED
                    ↕
                 WAITING / BLOCKED / TIMED_WAITING
```

| 상태 | 설명 |
|------|------|
| NEW | 스레드 생성 후 start() 전 |
| RUNNABLE | 실행 가능 상태 (실행 중 포함) |
| BLOCKED | 락(lock) 획득 대기 중 |
| WAITING | 다른 스레드의 신호 대기 |
| TIMED_WAITING | 일정 시간 대기 (`sleep`, `join`) |
| TERMINATED | 실행 완료 |

---

## 동기화 (Synchronization)

여러 스레드가 공유 자원에 동시에 접근하면 **경쟁 조건(Race Condition)** 이 발생할 수 있습니다.
`synchronized` 키워드로 한 번에 하나의 스레드만 접근하도록 제어합니다.

```java
public class Counter {
    private int count = 0;

    // synchronized 메서드
    public synchronized void increment() {
        count++;
    }

    // synchronized 블록
    public void decrement() {
        synchronized (this) {
            count--;
        }
    }

    public int getCount() { return count; }
}
```

---

## 주요 스레드 메서드

| 메서드 | 설명 |
|--------|------|
| `start()` | 스레드 시작 |
| `sleep(ms)` | 지정 시간(ms) 동안 일시 정지 |
| `join()` | 해당 스레드가 끝날 때까지 대기 |
| `interrupt()` | 스레드에 인터럽트 신호 전송 |
| `isAlive()` | 스레드 실행 중 여부 확인 |

```java
Thread t = new Thread(() -> {
    try {
        Thread.sleep(2000);  // 2초 대기
        System.out.println("작업 완료");
    } catch (InterruptedException e) {
        System.out.println("인터럽트 발생");
    }
});

t.start();
t.join();  // t 스레드가 끝날 때까지 메인 스레드 대기
System.out.println("메인 스레드 종료");
```

---

## ExecutorService (스레드 풀)

스레드를 직접 생성하는 대신 **스레드 풀**을 사용하면 스레드 재사용으로 성능을 높일 수 있습니다.

```java
import java.util.concurrent.*;

ExecutorService executor = Executors.newFixedThreadPool(4);  // 스레드 4개

for (int i = 0; i < 10; i++) {
    final int taskId = i;
    executor.submit(() -> {
        System.out.println("Task " + taskId + " - " + Thread.currentThread().getName());
    });
}

executor.shutdown();  // 새 작업 받지 않음
executor.awaitTermination(10, TimeUnit.SECONDS);  // 완료 대기
```

---

## 주의할 점

- `run()`을 직접 호출하면 새 스레드가 생성되지 않고 현재 스레드에서 실행됨 → 반드시 `start()` 사용
- 공유 자원에 여러 스레드가 접근하면 동기화 처리 필요
- `synchronized` 과다 사용 시 **데드락(Deadlock)** 발생 가능
- 스레드를 직접 생성하는 것보다 `ExecutorService` 사용 권장

---

## 면접 단골 질문

**Q. Thread와 Runnable의 차이는?**
Thread는 클래스 상속 방식으로 다른 클래스 상속 불가, Runnable은 인터페이스 구현 방식으로 유연하게 사용 가능

**Q. 데드락(Deadlock)이란?**
두 스레드가 서로 상대방이 가진 락을 기다리며 영원히 대기하는 상태

**Q. synchronized와 volatile의 차이는?**
`synchronized`는 메서드/블록 단위로 상호 배제와 가시성 보장, `volatile`은 변수 단위로 가시성만 보장

---

## 참고 링크

- [Java 스레드 공식 문서](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html)
