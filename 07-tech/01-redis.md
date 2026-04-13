# Redis

## 개념 요약

**Redis(Remote Dictionary Server)** 는 메모리 기반의 키-값(Key-Value) 저장소입니다.
데이터를 메모리에 저장하기 때문에 매우 빠르며, 캐시, 세션, 실시간 데이터 처리에 주로 사용됩니다.

---

## Redis의 특징

- **In-Memory** - 데이터를 메모리에 저장해 디스크 기반 DB보다 훨씬 빠름
- **다양한 자료구조** - String, List, Hash, Set, Sorted Set 등 지원
- **영속성** - RDB(스냅샷), AOF(명령어 로그) 방식으로 디스크에 저장 가능
- **TTL(Time To Live)** - 키마다 만료 시간 설정 가능
- **싱글 스레드** - 명령어를 순차적으로 처리해 동시성 문제가 없음
- **Pub/Sub** - 발행/구독 메시지 패턴 지원

---

## 주요 자료구조

### String
가장 기본적인 타입. 문자열, 숫자, 직렬화된 객체 등을 저장합니다.

```
SET key value
GET key
INCR counter       # 숫자 1 증가
EXPIRE key 3600    # 3600초(1시간) 후 만료
TTL key            # 남은 만료 시간 확인
```

### List
순서가 있는 문자열 목록. 스택/큐로 활용할 수 있습니다.

```
RPUSH mylist "a" "b" "c"   # 오른쪽에 추가
LPUSH mylist "z"            # 왼쪽에 추가
LRANGE mylist 0 -1          # 전체 조회
LPOP mylist                 # 왼쪽에서 꺼내기
RPOP mylist                 # 오른쪽에서 꺼내기
```

### Hash
필드-값 쌍의 집합. 객체를 저장할 때 유용합니다.

```
HSET user:1 name "홍길동" age 25
HGET user:1 name           # "홍길동"
HGETALL user:1             # 모든 필드-값 조회
HDEL user:1 age            # 특정 필드 삭제
```

### Set
중복 없는 문자열 집합. 집합 연산(합집합, 교집합, 차집합)이 가능합니다.

```
SADD myset "a" "b" "c"
SMEMBERS myset             # 전체 조회
SISMEMBER myset "a"        # 포함 여부 확인
SINTER set1 set2           # 교집합
SUNION set1 set2           # 합집합
```

### Sorted Set
점수(Score)를 기준으로 자동 정렬되는 Set. 랭킹 구현에 자주 사용됩니다.

```
ZADD leaderboard 100 "홍길동"
ZADD leaderboard 200 "김철수"
ZRANGE leaderboard 0 -1 WITHSCORES   # 오름차순 조회
ZREVRANGE leaderboard 0 -1            # 내림차순 조회
ZRANK leaderboard "홍길동"            # 순위 조회
```

---

## Spring Boot에서 Redis 사용

**의존성 추가 (pom.xml)**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

**설정 (application.yml)**

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
```

**RedisTemplate 사용**

```java
@Service
public class RedisService {
    private final RedisTemplate<String, String> redisTemplate;

    public RedisService(RedisTemplate<String, String> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }

    // String
    public void set(String key, String value) {
        redisTemplate.opsForValue().set(key, value);
    }

    public String get(String key) {
        return redisTemplate.opsForValue().get(key);
    }

    // TTL 설정
    public void setWithTTL(String key, String value, long seconds) {
        redisTemplate.opsForValue().set(key, value, seconds, TimeUnit.SECONDS);
    }

    // Set
    public void addToSet(String key, String value) {
        redisTemplate.opsForSet().add(key, value);
    }

    public Set<String> getSet(String key) {
        return redisTemplate.opsForSet().members(key);
    }

    // Hash
    public void setHash(String key, String field, String value) {
        redisTemplate.opsForHash().put(key, field, value);
    }

    public Object getHash(String key, String field) {
        return redisTemplate.opsForHash().get(key, field);
    }
}
```

---

## 캐싱 활용 패턴

### Cache-Aside (Look-Aside) 패턴
가장 많이 사용되는 캐싱 전략입니다.

```
1. 캐시에서 데이터 조회
2. 캐시에 있으면 → 캐시 데이터 반환
3. 캐시에 없으면 → DB에서 조회 후 캐시에 저장 → 반환
```

```java
public String getUserName(int userId) {
    String key = "user:" + userId + ":name";
    String cached = redisTemplate.opsForValue().get(key);

    if (cached != null) return cached;  // 캐시 히트

    // 캐시 미스 → DB 조회
    String name = userRepository.findById(userId).getName();
    redisTemplate.opsForValue().set(key, name, 3600, TimeUnit.SECONDS);
    return name;
}
```

### Write-Through 패턴
DB에 쓸 때 캐시에도 함께 씁니다. 캐시와 DB의 데이터 일관성을 유지합니다.

---

## Chat 프로젝트에서의 Redis 활용

| 용도 | 키 예시 | 자료구조 |
|------|---------|---------|
| 온라인 유저 관리 | `room:{roomId}:online` | Set |
| 채팅방 목록 캐시 | `rooms` | Hash / String |
| 최근 메시지 캐시 | `room:{roomId}:messages` | List |
| Rate Limiting | `rate:{userId}` | String (INCR + TTL) |
| 서버 재시작 후 복원 | `room:{roomId}:participants` | Set |

---

## 주의할 점

- Redis는 메모리 기반이므로 **용량 관리**가 중요하다 → TTL 설정 권장
- **직렬화** 설정을 명확히 해야 한다 (기본 JDK 직렬화는 가독성이 낮아 JSON 직렬화 권장)
- 싱글 스레드이므로 **시간이 오래 걸리는 명령어**(`KEYS *` 등)는 서버 전체에 영향을 준다
- 중요한 데이터는 Redis에만 저장하지 말고 **DB와 함께 사용**해야 한다

---

## 면접 단골 질문

**Q. Redis를 캐시로 사용하는 이유는?**
메모리 기반으로 디스크 I/O 없이 데이터를 조회하므로 DB 부하를 줄이고 응답 속도를 높이기 위해

**Q. Redis의 영속성 방법 두 가지는?**
RDB는 일정 주기로 스냅샷을 저장, AOF는 모든 쓰기 명령어를 로그로 저장. AOF가 더 안전하지만 파일 크기가 커짐

**Q. Cache-Aside 패턴이란?**
캐시에서 먼저 조회하고, 없으면 DB에서 가져온 후 캐시에 저장하는 패턴. 읽기가 많은 서비스에 적합

---

## 참고 링크

- [Redis 공식 문서](https://redis.io/docs/)
- [Spring Data Redis 공식 문서](https://docs.spring.io/spring-data/redis/docs/current/reference/html/)
