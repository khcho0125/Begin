# Spring boot로 Netty Socket io 삽질하기

### 목록

- ### [AckRequest](#acksender)

- ### [SocketIo Configuration](#socket-configuration)

- ### [Client](#socketioclient)

------

- ## AckSender

```java
	private DataListener<ChatMessage> onMessage() {
        return (client, data, ackSender) -> {
            namespace.getRoomOperations(data.getRoomCode()).sendEvent("newMessage", client, data);
            messageRepository.save(data);
        };
    }
```

#### Event를 생성할 때 client와 data, ackSender 인자를 받는다.

#### Client와 data는 알겠는데... ackSender은 뭐지?

<img src="https://user-images.githubusercontent.com/82090641/167765831-0f3731f0-19a3-40f2-8852-9674381e46f5.png" width="700">

> ##### AckRequest의 일부이다. *(ackSender Object)*

#### 위 내용을 해석해보면

#### <ins>isAckRequest() 메서드로 확인할 수 있고 sendAckData() 메서드를 Event 도중 사용할 수 있습니다. sendAckData가 호출 되지 않은 경우 빈 인자로 호출됩니다.</ins>

#### 어딘가에서 호출이 되어야 하나보다...

<img src="https://user-images.githubusercontent.com/82090641/167765416-3a28e2ac-57e4-43a9-85a7-29dc0cae5880.png" width="550">

> ##### 출처 : Socket.io docs

#### Client에서 Event를 보낼 때 Function을 인자로 넣으라고 한다.

```javascript
socket.emit("userJoin", {
        "roomCode": roomCode,
        "username": username,
        "sessionId": socket.id
    }, () => {
        console.log("Hello") // test 부분
    });
```

```java
log.info("{}", ackSender.isAckRequested());
```

#### 이벤트에 isAckRequested() 메소드를 사용해 보았다.

<img src="https://user-images.githubusercontent.com/82090641/167767638-34895466-216f-4730-8e35-38a27732d442.png">

#### 로그에 true가 출력이 되었다!

#### 그럼 Function를 어떻게 호출할까..?

<img src="https://user-images.githubusercontent.com/82090641/167767853-9949fc13-9db3-4f8f-adce-d98697d137b1.png" width="700">

#### isAckRequested() 메소드는 비어있는지 아닌지 확인하는 메소드이고... sendAckData() 메소드는 Object를 객체로 넣어주는데...

#### 혹시 함수의 매개변수를 넘겨주는 것인가??

```java
ackSender.sendAckData();
```

<img src="https://user-images.githubusercontent.com/82090641/167768349-c7f4ef1b-b190-44e9-9ef5-4902aff9d45e.png" width="450">

#### 정답이였다!

#### sendAckData() 메소드를 사용하면 client.send() 메서드를 통해 Client에서 함수가 호출되는 방식이였다.

#### ackSender도 꽤 유용하게 사용할 수 있을 것 같다.

------

- ## Socket Configuration

> #### Config 기본 세팅

```java
@Bean
public SocketIOServer socketIOServer() {
    com.corundumstudio.socketio.Configuration config = new com.corundumstudio.socketio.Configuration(); 
    SocketConfig socketConfig = new SocketConfig();
    config.setSocketConfig(socketConfig);
    config.setHostname(properties.getHost());
    config.setPort(properties.getPort());
    return new SocketIOServer(config);
}
```

#### 이번엔 Configuration를 더 깊이 알아보도록 하겠다.

#### 굉장히 많은 양의 코드들이 있을 것이다.

```java
	private ExceptionListener exceptionListener = new DefaultExceptionListener();
	// Event Exception 처리를 위한 Listener 설정

    private String context = "/socket.io";

    private List<Transport> transports = Arrays.asList(Transport.WEBSOCKET, Transport.POLLING);
	//  Websocket과 Polling 연결 허용 설정

    private int bossThreads = 0; // 0 = current_processors_amount * 2
    private int workerThreads = 0; // 0 = current_processors_amount * 2
	// boss/workerThread 의 연결 수를 설정한다.

    private boolean useLinuxNativeEpoll;
	/* 서버가 루프백 인터페이스에 바인딩된 경우 유닉스 소켓을 활성화한다.
	   Loopback interface : 물리적으로 연결하는 Port가 존재하지 않는 Port이다.*/

    private boolean allowCustomRequests = false;
	/* 사용자 지정 요청을 처리할 수 있도록 허용한다
	   이 경우 연결이 끊기지 않도록 Handler를 처리하는 CustomHandler를 추가해야한다.*/

    private int upgradeTimeout = 10000;
	// Protocol upgrade 시간 초과 설정 (밀리초)

    private int pingTimeout = 60000;
	/* Ping message 시간 초과 설정 (밀리초)
	   이 시간내에 heartbeat message가 수신되지 않으면 Timeout Event가 전송된다.*/

    private int pingInterval = 25000;
	// heartbeat message가 전송되는 주기 (밀리초)

    private int firstDataTimeout = 5000;
	// Channel 개방 후 첫번째 data의 전송 사이의 시간초과 설정 (밀리초)
	

    private int maxHttpContentLength = 64 * 1024;
	// httpContent의 길이 설정

    private int maxFramePayloadLength = 64 * 1024;
	// 데이터의 최대 길이 설정 (빅데이터를 사용한 공격을 예방하기 위해)

    private String packagePrefix;
	// 전체 클래스 이름 없이 클라이언트에서 json-object를 보내기 위한 패키지 접두사이다.

    private String hostname;
    private int port = -1;

    private String sslProtocol = "TLSv1";
	// SSL Protocol 설정

    private String keyStoreFormat = "JKS";
    private InputStream keyStore;
    private String keyStorePassword;
	// SSL 클라이언트 인증 키 설정

    private String trustStoreFormat = "JKS";
    private InputStream trustStore;
    private String trustStorePassword;
	// SSl 서버 인증 키 설정

    private String keyManagerFactoryAlgorithm = KeyManagerFactory.getDefaultAlgorithm();
	// Key 관리 알고리즘 설정

    private boolean preferDirectBuffer = true;
	/* direct buffer가 인코딩된 메시지의 대상으로 사용되어야 하는 경우 true
	   false를 사용하면 heap buffer를 할당하고 바이트 배열에 의해 백업한다.*/

    private SocketConfig socketConfig = new SocketConfig();
	// TCP Socket 설정

    private StoreFactory storeFactory = new MemoryStoreFactory();
	// 세션 데이터를 저장하고 분산 pubsub를 구현하는 데 사용된다.

    private JsonSupport jsonSupport;
	// Json 설정

    private AuthorizationListener authorizationListener = new SuccessAuthorizationListener();
	// 인증 Listener 설정

    private AckMode ackMode = AckMode.AUTO_SUCCESS_ONLY;

    private boolean addVersionHeader = true;
	// Header, http response에 라이브러리 버전 추가 설정

    private String origin;
	// Access-Control-Allow-Origin header 설정

    private boolean httpCompression = true;
	// http Protocol 압축을 활성화/비활성화

    private boolean websocketCompression = true;
	// websocket Protocol 압축을 활성화/비활성화

    private boolean randomSession = false;
	// randomSession 설정
```

- #### AckMode

  - #### AUTO

    ##### 패킷 처리 후 확인 응답(ack) 자동 전송

  - #### AUTO_SUCCESS_ONLY

    ##### 패킷 처리가 성공한 후에만 확인 응답(ack) 자동 전송

  - #### MANUAL

    ##### ack 전송을 수동으로 전환한다.

#### 이렇게 다양한 Configuration 옵션에 대해 알아보았다. 

------

## SocketIoClient

#### Client 자체에도 많은 데이터가 있다.

- #### getHandshakeData()

  ##### Event와 함께 보내진 Param, Header 등의 정보가 담겨있는 HandshakeData를 반환한다.

- #### getTransport()

  ##### Event가 전송되어온 Transport를 반환한다. Ex) Polling

- #### sendEvent(String name, AckCallback<?> ackCallback, Object ...data)

  ##### client에 Event를 전송한다. ack 콜백함수를 호출할 수도 있다.

- #### send(Packet packet, AckCallback<?> ackCallback)

  ##### client에 콜백 함수와 함께 패킷을 전달한다.

- #### getNamespace()

  ##### client가 연결되어 있는 namespace를 반환한다.

- #### getSessionId()

  ##### client의 SessionId를 반환한다.

- #### getRemoteAddress()

  ##### client가 연결되어있는 주소를 반환한다.

  ##### getRemoteAddress() = getHandshakeData().getAddress()

- #### isChannelOpen()

  ##### 열려있는 채널이라면 true를 반환하고 아니라면 false를 반환한다.

- #### joinRoom(String room)

  ##### Room에 연결한다.

- #### leaveRoom()

  ##### Room에서 연결해제한다.

- #### getAllRooms()

  ##### join되어있는 모든 Room을 반환한다.

### 상황에 맞게 SocketIoClient 객체의 데이터를 가져와 사용하면 된다.

------

> #### 참고자료

- https://socket.io/docs/v3
- https://github.com/mrniko/netty-socketio
- https://myhappyman.tistory.com/172
- https://intrepidgeeks.com/tutorial/implementation-of-spring-boot-netty-socketi-serverside-messaging
- https://intrepidgeeks.com/tutorial/detailed-description-of-springboot-netty-socketio-project
- http://btsweet.blogspot.com/2014/06/tls-ssl.html
- https://javacoding.tistory.com/149
- https://blog.karatos.in/a?ID=01500-d6761b01-d8a9-40ad-a617-d6b814762690
- https://itot.tistory.com/22

