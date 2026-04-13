# Kafka

## 개념 요약

**Apache Kafka**는 대용량 실시간 데이터를 처리하기 위한 **분산 메시지 스트리밍 플랫폼**입니다.
Producer가 메시지를 발행하고, Consumer가 메시지를 구독하는 Pub/Sub 구조로 동작합니다.

---

## Kafka의 특징

- **고성능** - 디스크 순차 I/O와 배치 처리로 높은 처리량
- **내구성** - 메시지를 디스크에 저장해 유실 없이 보관
- **확장성** - 파티션과 브로커를 추가해 수평 확장 가능
- **내결함성** - 복제(Replication)를 통해 브로커 장애 시에도 데이터 유지
- **재처리 가능** - 메시지를 일정 기간 보관하여 재처리 가능
- **순서 보장** - 파티션 내에서 메시지 순서 보장

---

## 핵심 구성 요소

### Producer
메시지를 생성하여 Kafka Topic에 발행하는 주체입니다.

### Consumer
Topic을 구독하여 메시지를 가져와 처리하는 주체입니다.

### Topic
메시지를 저장하는 논리적인 채널입니다. 이메일의 수신함 같은 개념입니다.

### Partition
Topic을 물리적으로 나눈 단위입니다. 파티션이 많을수록 병렬 처리 성능이 높아집니다.
같은 파티션 내에서는 메시지 순서가 보장됩니다.

### Broker
Kafka 서버 노드입니다. 메시지를 저장하고 Producer/Consumer 요청을 처리합니다.

### Consumer Group
여러 Consumer가 하나의 그룹을 이루어 파티션을 나눠서 처리합니다.
같은 그룹 내에서 하나의 파티션은 하나의 Consumer만 처리합니다.

### Offset
파티션 내에서 메시지의 위치를 나타내는 번호입니다.
Consumer는 Offset을 통해 어디까지 읽었는지 추적합니다.

---

## 전체 구조

```
[Producer]
    │  메시지 발행
    ▼
[Kafka Broker]
    ├── Topic A
    │    ├── Partition 0  [msg0] [msg1] [msg2] ...
    │    ├── Partition 1  [msg0] [msg1] ...
    │    └── Partition 2  [msg0] ...
    └── Topic B
         └── ...
              │  메시지 구독
              ▼
[Consumer Group]
    ├── Consumer 1  ← Partition 0 담당
    ├── Consumer 2  ← Partition 1 담당
    └── Consumer 3  ← Partition 2 담당
```

---

## Kafka vs 일반 메시지 큐 (RabbitMQ 등)

| 구분 | Kafka | RabbitMQ |
|------|-------|----------|
| 메시지 보관 | 디스크에 일정 기간 보관 | 소비 후 삭제 |
| 재처리 | 가능 (Offset 조정) | 불가 |
| 처리량 | 매우 높음 | 상대적으로 낮음 |
| 순서 보장 | 파티션 내 보장 | 큐 단위 보장 |
| 사용 목적 | 대용량 스트리밍, 로그 | 작업 큐, 라우팅 |

---

## Spring Boot에서 Kafka 사용

**의존성 추가 (pom.xml)**

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

**설정 (application.yml)**

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    consumer:
      group-id: chat-group
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
```

**Producer**

```java
@Service
public class ChatProducer {
    private final KafkaTemplate<String, String> kafkaTemplate;

    public ChatProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
    }
}
```

**Consumer**

```java
@Service
public class ChatConsumer {
    private final SimpMessagingTemplate messagingTemplate;

    public ChatConsumer(SimpMessagingTemplate messagingTemplate) {
        this.messagingTemplate = messagingTemplate;
    }

    @KafkaListener(topics = "chat-topic", groupId = "chat-group")
    public void consume(String message) {
        // Kafka에서 메시지 수신 → WebSocket으로 브로드캐스트
        messagingTemplate.convertAndSend("/topic/chat", message);
    }
}
```

---

## Chat 프로젝트에서의 Kafka 활용

```
[Browser] → WebSocket → [ChatController]
                              │
                         KafkaProducer
                              │  chat-topic에 발행
                         [Kafka Broker]
                              │  chat-topic 구독
                         KafkaConsumer
                              │
                         WebSocket 브로드캐스트
                              │
                         [모든 Browser]
```

Kafka를 중간에 두면 메시지 처리를 **비동기**로 분리할 수 있고,
서버가 여러 대로 확장될 때도 모든 서버가 같은 메시지를 수신할 수 있습니다.

---

## 주의할 점

- 파티션 수는 **줄일 수 없다** (늘리는 것만 가능) → 초기 설계가 중요
- Consumer 수가 파티션 수보다 많으면 **유휴 Consumer**가 생긴다
- 메시지 순서가 중요한 경우 **같은 키(Key)** 를 사용해 같은 파티션으로 보내야 한다
- Zookeeper는 Kafka 2.8 이전까지 필수였으나 이후 버전에서는 **KRaft 모드**로 대체 가능

---

## 면접 단골 질문

**Q. Kafka를 사용하는 이유는?**
서비스 간 결합도를 낮추고, 대용량 메시지를 비동기로 처리하며, 메시지를 보관해 재처리가 가능하기 때문

**Q. Kafka의 파티션이란?**
Topic을 물리적으로 나눈 단위로, 파티션이 많을수록 병렬 처리 성능이 높아짐. 파티션 내에서는 순서가 보장됨

**Q. Consumer Group이란?**
여러 Consumer가 하나의 그룹을 이루어 파티션을 나눠 처리하는 구조. 같은 그룹 내에서 하나의 파티션은 하나의 Consumer만 처리

**Q. Kafka와 RabbitMQ의 차이는?**
Kafka는 디스크에 메시지를 보관해 재처리가 가능하고 처리량이 높음. RabbitMQ는 소비 후 삭제되며 복잡한 라우팅에 적합

---

## 참고 링크

- [Apache Kafka 공식 문서](https://kafka.apache.org/documentation/)
- [Spring Kafka 공식 문서](https://docs.spring.io/spring-kafka/docs/current/reference/html/)
