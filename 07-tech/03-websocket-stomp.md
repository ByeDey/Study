# WebSocket & STOMP

## 개념 요약

**WebSocket**은 클라이언트와 서버 간 **양방향 실시간 통신**을 가능하게 하는 프로토콜입니다.
**STOMP**는 WebSocket 위에서 동작하는 메시지 프로토콜로, 목적지(destination) 기반으로 메시지를 라우팅합니다.

---

## HTTP vs WebSocket

| 구분 | HTTP | WebSocket |
|------|------|-----------|
| 연결 방식 | 요청-응답 후 연결 종료 | 연결 유지 (Persistent) |
| 통신 방향 | 단방향 (클라이언트 → 서버) | 양방향 |
| 실시간성 | 폴링/롱폴링 필요 | 즉시 푸시 가능 |
| 오버헤드 | 매 요청마다 헤더 포함 | 초기 핸드셰이크 후 최소화 |
| 사용 사례 | 일반 웹 요청 | 채팅, 알림, 게임 |

---

## WebSocket 동작 과정

```
1. HTTP 핸드셰이크 (업그레이드 요청)
   Client → Server: "Upgrade: websocket"

2. 연결 수립
   Server → Client: "101 Switching Protocols"

3. 양방향 통신
   Client ↔ Server: 자유롭게 메시지 교환

4. 연결 종료
   Client or Server: Close Frame 전송
```

---

## STOMP (Simple Text Oriented Messaging Protocol)

WebSocket은 단순히 텍스트/바이너리 데이터를 주고받는 저수준 프로토콜입니다.
STOMP는 그 위에서 **메시지 형식과 라우팅 규칙**을 정의합니다.

**STOMP 메시지 구조**

```
COMMAND
header1:value1
header2:value2

Body^@
```

**주요 STOMP 명령어**

| 명령어 | 방향 | 설명 |
|--------|------|------|
| `CONNECT` | Client → Server | 연결 요청 |
| `SUBSCRIBE` | Client → Server | 특정 목적지 구독 |
| `SEND` | Client → Server | 메시지 전송 |
| `MESSAGE` | Server → Client | 구독자에게 메시지 전달 |
| `DISCONNECT` | Client → Server | 연결 종료 |

---

## SockJS

WebSocket을 지원하지 않는 브라우저를 위한 **폴백(Fallback) 라이브러리**입니다.
WebSocket이 가능하면 WebSocket을 사용하고, 불가능하면 롱폴링 등으로 대체합니다.

---

## Spring Boot에서 WebSocket + STOMP 설정

**의존성 추가 (pom.xml)**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

**WebSocket 설정**

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        // 클라이언트가 구독할 경로 prefix
        config.enableSimpleBroker("/topic", "/queue");
        // 클라이언트가 서버로 메시지를 보낼 경로 prefix
        config.setApplicationDestinationPrefixes("/app");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        // WebSocket 연결 엔드포인트
        registry.addEndpoint("/ws")
                .setAllowedOriginPatterns("*")
                .withSockJS();  // SockJS 폴백 활성화
    }
}
```

**Controller**

```java
@Controller
public class ChatController {
    private final ChatService chatService;

    // 클라이언트가 /app/chat으로 메시지를 보내면 처리
    @MessageMapping("/chat")
    public void handleMessage(ChatMessage message) {
        chatService.processMessage(message);
    }

    // REST API로 채팅방 목록 조회
    @GetMapping("/api/rooms")
    @ResponseBody
    public List<Room> getRooms() {
        return chatService.getRooms();
    }
}
```

**서버 → 클라이언트 메시지 전송**

```java
@Service
public class ChatService {
    private final SimpMessagingTemplate messagingTemplate;

    // 특정 토픽 구독자에게 브로드캐스트
    public void broadcastToRoom(String roomId, Object message) {
        messagingTemplate.convertAndSend("/topic/room/" + roomId, message);
    }

    // 특정 유저에게만 전송
    public void sendToUser(String userId, Object message) {
        messagingTemplate.convertAndSendToUser(userId, "/queue/messages", message);
    }
}
```

**클라이언트 (JavaScript)**

```javascript
const socket = new SockJS('/ws');
const stompClient = Stomp.over(socket);

stompClient.connect({}, () => {
    // 채팅방 구독
    stompClient.subscribe('/topic/room/1', (message) => {
        console.log(JSON.parse(message.body));
    });

    // 메시지 전송
    stompClient.send('/app/chat', {}, JSON.stringify({
        roomId: 1,
        content: 'Hello!'
    }));
});
```

---

## Chat 프로젝트에서의 WebSocket 활용

```
[Browser]
    │  SockJS 연결 (/ws)
    ▼
[Spring Boot]
    │
    ├── @MessageMapping("/chat")      ← 메시지 수신
    ├── SessionRegistry               ← 세션-닉네임 매핑 관리
    │
    └── SimpMessagingTemplate
         └── /topic/room/{roomId}     ← 채팅방 구독자에게 브로드캐스트
```

---

## 주의할 점

- WebSocket은 HTTP와 달리 **상태를 유지**하므로 서버 메모리 관리가 중요하다
- 로드밸런서 환경에서는 **Sticky Session** 또는 외부 메시지 브로커(Redis, Kafka) 필요
- `@MessageMapping`은 `/app` prefix가 붙은 경로를 처리한다 (`/app/chat` → `@MessageMapping("/chat")`)
- SockJS는 WebSocket 지원 여부를 자동으로 감지해 폴백 방식을 선택한다

---

## 면접 단골 질문

**Q. WebSocket과 HTTP의 차이는?**
HTTP는 요청-응답 후 연결이 끊기는 단방향 통신, WebSocket은 한 번 연결 후 양방향으로 지속적으로 통신 가능

**Q. STOMP를 사용하는 이유는?**
WebSocket은 단순 데이터 전송만 가능하므로, STOMP로 메시지 형식과 목적지 기반 라우팅을 추가해 구조적인 메시지 처리가 가능해짐

**Q. SockJS란?**
WebSocket을 지원하지 않는 환경을 위한 폴백 라이브러리. WebSocket이 가능하면 WebSocket을, 불가능하면 롱폴링 등으로 자동 대체

---

## 참고 링크

- [Spring WebSocket 공식 문서](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#websocket)
- [STOMP 프로토콜 명세](https://stomp.github.io/stomp-specification-1.2.html)
