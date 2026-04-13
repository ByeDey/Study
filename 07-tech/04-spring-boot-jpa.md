# Spring Boot & Spring Data JPA

## 개념 요약

**Spring Boot**는 Spring 프레임워크를 기반으로 복잡한 설정을 자동화하여 빠르게 애플리케이션을 개발할 수 있게 해주는 프레임워크입니다.
**Spring Data JPA**는 JPA를 추상화하여 반복적인 데이터 접근 코드를 줄여주는 라이브러리입니다.

---

## Spring Boot

### 핵심 특징

- **Auto Configuration** - 의존성을 추가하면 관련 설정이 자동으로 구성됨
- **Embedded Server** - Tomcat, Jetty 등이 내장되어 별도 서버 설치 불필요
- **Starter 의존성** - 관련 라이브러리를 묶어서 제공 (`spring-boot-starter-web` 등)
- **Actuator** - 애플리케이션 모니터링 및 관리 엔드포인트 제공

### 주요 어노테이션

| 어노테이션 | 설명 |
|-----------|------|
| `@SpringBootApplication` | 메인 클래스에 사용, Auto Configuration + Component Scan 활성화 |
| `@RestController` | `@Controller` + `@ResponseBody` |
| `@Controller` | MVC 컨트롤러, View 반환 |
| `@Service` | 비즈니스 로직 클래스 |
| `@Repository` | 데이터 접근 클래스 |
| `@Component` | 일반 Spring Bean 등록 |
| `@Configuration` | 설정 클래스 |
| `@Bean` | 메서드의 반환 객체를 Bean으로 등록 |

### 의존성 주입 (DI)

```java
// 생성자 주입 (권장)
@Service
public class ChatService {
    private final ChatRepository chatRepository;
    private final RedisTemplate<String, String> redisTemplate;

    public ChatService(ChatRepository chatRepository,
                       RedisTemplate<String, String> redisTemplate) {
        this.chatRepository = chatRepository;
        this.redisTemplate = redisTemplate;
    }
}

// @Autowired 필드 주입 (권장하지 않음)
@Service
public class ChatService {
    @Autowired
    private ChatRepository chatRepository;
}
```

생성자 주입을 권장하는 이유 - 불변성 보장, 테스트 용이, 순환 참조 방지

---

## JPA & Spring Data JPA

### JPA란?

**JPA(Java Persistence API)** 는 Java 객체와 관계형 DB 테이블을 매핑하는 ORM 표준 명세입니다.
**Hibernate**가 대표적인 JPA 구현체입니다.

### 엔티티 정의

```java
@Entity
@Table(name = "rooms")
public class Room {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 100)
    private String name;

    private String creatorId;

    @CreatedDate
    private LocalDateTime createdAt;

    @OneToMany(mappedBy = "room", cascade = CascadeType.ALL)
    private List<Message> messages = new ArrayList<>();
}

@Entity
@Table(name = "messages")
public class Message {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String content;
    private String sender;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "room_id")
    private Room room;
}
```

### 주요 JPA 어노테이션

| 어노테이션 | 설명 |
|-----------|------|
| `@Entity` | JPA 엔티티 클래스 |
| `@Table` | 매핑할 테이블 이름 지정 |
| `@Id` | 기본 키 지정 |
| `@GeneratedValue` | 기본 키 자동 생성 전략 |
| `@Column` | 컬럼 세부 설정 |
| `@OneToMany` | 1:N 관계 |
| `@ManyToOne` | N:1 관계 |
| `@ManyToMany` | N:N 관계 |
| `@JoinColumn` | 외래 키 컬럼 지정 |

### Spring Data JPA - Repository

```java
public interface RoomRepository extends JpaRepository<Room, Long> {
    // 메서드 이름으로 쿼리 자동 생성
    List<Room> findByCreatorId(String creatorId);
    Optional<Room> findByName(String name);
    boolean existsByName(String name);

    // JPQL 직접 작성
    @Query("SELECT r FROM Room r WHERE r.createdAt >= :date")
    List<Room> findRecentRooms(@Param("date") LocalDateTime date);

    // Native Query
    @Query(value = "SELECT * FROM rooms ORDER BY created_at DESC LIMIT 10",
           nativeQuery = true)
    List<Room> findTop10Recent();
}
```

**JpaRepository 기본 제공 메서드**

| 메서드 | 설명 |
|--------|------|
| `save(entity)` | 저장 또는 수정 |
| `findById(id)` | ID로 조회 (Optional 반환) |
| `findAll()` | 전체 조회 |
| `delete(entity)` | 삭제 |
| `count()` | 전체 개수 |
| `existsById(id)` | 존재 여부 확인 |

### 영속성 컨텍스트

JPA가 엔티티를 관리하는 1차 캐시 공간입니다.

```
영속성 컨텍스트
 ├── 1차 캐시      → 같은 트랜잭션 내 동일 엔티티 재조회 시 DB 접근 없이 캐시 반환
 ├── 변경 감지     → 트랜잭션 종료 시 엔티티 변경사항 자동 감지 후 UPDATE 쿼리 실행
 ├── 지연 로딩     → @ManyToOne(fetch = LAZY) 설정 시 실제 사용 시점에 쿼리 실행
 └── 쓰기 지연     → 트랜잭션 커밋 전까지 SQL을 모아서 한 번에 실행
```

```java
@Transactional
public void updateRoomName(Long id, String newName) {
    Room room = roomRepository.findById(id).orElseThrow();
    room.setName(newName);  // 변경 감지 → 별도 save() 호출 없이 UPDATE 실행
}
```

---

## N+1 문제

연관 관계에서 자주 발생하는 성능 문제입니다.

```java
// 문제 상황
List<Room> rooms = roomRepository.findAll();  // SELECT * FROM rooms (1번)
for (Room room : rooms) {
    room.getMessages();  // SELECT * FROM messages WHERE room_id = ? (N번)
}
// 총 1 + N번의 쿼리 발생
```

**해결 방법**

```java
// Fetch Join으로 한 번에 조회
@Query("SELECT r FROM Room r JOIN FETCH r.messages")
List<Room> findAllWithMessages();

// @EntityGraph 사용
@EntityGraph(attributePaths = {"messages"})
List<Room> findAll();
```

---

## 주의할 점

- `@Transactional`이 없으면 변경 감지(Dirty Checking)가 동작하지 않는다
- `FetchType.EAGER`는 N+1 문제를 유발할 수 있어 `LAZY` 사용 권장
- 지연 로딩 사용 시 **트랜잭션 밖에서 연관 엔티티 접근 시 LazyInitializationException** 발생
- `save()`는 새 엔티티면 INSERT, 기존 엔티티면 UPDATE를 실행

---

## 면접 단골 질문

**Q. JPA를 사용하는 이유는?**
SQL을 직접 작성하지 않고 객체 중심으로 DB를 다룰 수 있어 생산성이 높고, DB 변경 시 코드 수정을 최소화할 수 있음

**Q. N+1 문제란?**
연관 관계 조회 시 1번의 쿼리로 N개의 엔티티를 가져온 뒤, 각 엔티티의 연관 데이터를 N번 추가 조회하는 문제

**Q. 영속성 컨텍스트란?**
JPA가 엔티티를 관리하는 1차 캐시 공간. 변경 감지, 지연 로딩, 쓰기 지연 등의 기능을 제공

**Q. `@Transactional`이란?**
트랜잭션 범위를 지정하는 어노테이션. 메서드 실행 중 예외 발생 시 롤백, 정상 종료 시 커밋

---

## 참고 링크

- [Spring Boot 공식 문서](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Spring Data JPA 공식 문서](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
