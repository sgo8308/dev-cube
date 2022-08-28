### I/O 과정은 어떻게 될까?
    
    프로세스는 시스템 콜을 통해 운영체제에게 데이터가 빠져나가도록 요청하거나 들어오도록 요청한다.
    
    커널 영역에는 버퍼가 존재하고 이 버퍼로 프로세스로부터 데이터가 오고 나가고, 다른 디바이스로부터 데이터가 오고 프로세스로 들어간다.
    
    그리고 이 커널 영역의 버퍼를 가상메모리를 통해 각 프로세스들은 공유한다.
    
    ---
    
    [https://howtodoinjava.com/java/io/how-java-io-works-internally/]
    
### I/O는 왜 공부해야 할까?
    
    프로그램에 있는 어떤 내용을 파일에 읽거나, 저장하거나, 다른 서버나 디바이스로 보낼 일이 있을 때 잘 사용하기 위해서
    
    ---
    
    이상민, 자바의 신, 2판 1쇄, 로드북, 706p, 2017

### Serialization과 Transiant 키워드를 같이 설명해주세요. ★
    
    Serialization이란 Serializable 인터페이스를 구현한 오브젝트와 그 오브젝트가 참조하는 오브젝트를 바이트 스트림으로 인코딩하는 기능이다. 
    
    인코딩된 바이트 스트림은 다른 곳으로 전달하는데 쓰이고 다시 Deserialization이 **가능하다.
    
    이 때 transient가 붙은 데이터는 인코딩 대상에서 제외한다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/serialization/index.html](https://docs.oracle.com/javase/8/docs/technotes/guides/serialization/index.html)
    
### try with resource란?
    
    tyr 구문에 소괄호가 생기는데 이 부분에 하나 또는 그 이상의 리소스를 선언하면 자동적으로 try구문이 끝날 때 리소스를 close해주는 자바에서 지원하는 문법이다.
    
    마찬가지로 catch와 finally 구문을 같이 쓸 수 있는데 이 구문들은 리소스가 close되고 넘어간다.
    
    ---
    
    [https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)
    
### stream은 왜 stream일까?
    
    개천이나 줄기라는 뜻으로 끊기지 않고 연속적인 데이터를 말한다.
    
    ---
    
    이상민, 자바의 신, 2판 1쇄, 로드북, 706p, 2017
    
### I/O에서 스트림이 어떤건지 설명해주세요. ★
    
    스트림이란 데이터를 운반하는데 사용되는 연결통로이다. 
    스트림은 단방향 통신만 가능하기 때문에 입력과 출력을 위한 스트림 2개가 필요하다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 868p, 2016
    
    - Stream은 왜 단방향일까?
        
        각각의 Stream은 큐로 구성되어서 FIFO형태를 지닌다.
        
        이런 상황에서 하나의 Stream으로 입력 출력을 다 사용하는 것보다 따로 구성하는게 더 효율적이라서 그런게 아닐까?
        
        동시에 입력 출력도 가능하고.. 
        
        근데 여기 나온 스트림이 그 스트림이 맞는지 모르겠다.
        
        ---
        
        [https://docs.oracle.com/cd/E26502_01/html/E35856/overvw1-38936.html#overvw1-3]
        
### Buffered Stream이란?
    
    일반적인 stream의 성능을 향상시키기 위해 나온 보조 스트림으로 내부적으로 일반 stream을 갖고 있다.
    
    Buffered Stream은 버퍼라고 불리는 메모리 영역이 완전히 빌 경우 시스템 콜을 호출하여 데이터를 읽고,버퍼가 가득 찰 경우 데이터를 내보낸다.
    
    즉 한번에 읽고 씀으로써 성능을 향상시킨다.
    
    개별 캐릭터가 아닌 보다 넓은 단위로 I/O할 수 있는 기능을 제공한다.
    
    ---
    
    [https://docs.oracle.com/javase/tutorial/essential/io/buffers.html]
    
### Buffered Stream을 사용하는 이유는?
    
    일반적인 stream들은 매 read나 write 요청마다 OS에 의해 직접적으로 처리된다. 
    그런 요청들은 disk 접근이나 네트워크 활동 또는 다른 비싼 작업을 요청하기 때문에 효율적이지 않다.
    
    위와 같은 오버헤드를 줄이기 위하여 Buffered Stream을 사용한다.
    
    ---
    
    [https://docs.oracle.com/javase/tutorial/essential/io/buffers.html]
    
### 바이트 기반 스트림으로 왜 문자를 처리하지 못할까?
    
    처리할 수는 있다.
    
    하지만 캐릭터 기반 스트림은 들어오는 인풋이나 아웃풋을 유니코드 컨벤션에 맞게 자동으로 변환해주기 때문에 더 사용하기 편한 장점이 있다.
   
    --- 
    [https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html](https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html)
    [https://docs.oracle.com/javase/tutorial/essential/io/bytestreams.html]
    
### 스트림은 왜 반드시 close()해주어야 할까?
    
    스트림은 데이터가 향하는 어떤 리소스를 잡고 있다.
    
    이 리소스를 다 사용하고 close 해주지 않은 경우를 resource leak이라 한다. 
    
    이 resource leak은 resource starvation을 일으켜서 시스템에 심각한 성능 저하와 불안전성, 크래시를 발생시킬 수 있다.
    
    - resource starvation이란?
        
        프로세스가 영구적으로 작업을 진행하기 위한 리소스를 얻지 못하는 현상. 
        
        병렬 컴퓨팅에서 주로 발생하나 리소스 릭으로도 발생할 수 있다.
        
        ---
        
        [https://en.wikipedia.org/wiki/Starvation_(computer_science)]
        
    - 리소스란?
        
        컴퓨터 시스템 안에 있는 물리적 또는 가상의 제한된 요소이다.
        
        파일 디스크립터, 네트워크 소켓, 메모리 영역 등이 있다. 
        
        스트림을 통해서 작업할 수 있는 모든 것은 리소스라 생각하면 된다.
        
        ---
        
        [https://en.wikipedia.org/wiki/System_resource]
        
        이상민, 자바의 신, 2판 1쇄, 로드북, 719p, 2017

# NIO

### NIO란?
    
    NIO란 non-bloking I/O를 줄인 말이다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/io/enhancements.html](https://docs.oracle.com/javase/8/docs/technotes/guides/io/enhancements.html)

### NIO를 이용한 서버는 어떤식으로 작동할까?
    
    요약 : while문을 돌면서 Selector를 통해 I/O Operation이 준비된 채널들을 기다리고 준비된 채널이 있다면 그에 맞는 작업을 진행해준다.
    
    public class EchoServer {
    
        private static final String POISON_PILL = "POISON_PILL";
    
        public static void main(String[] args) throws IOException {
            Selector selector = Selector.open();  // Selector 열기
            ServerSocketChannel serverSocket = ServerSocketChannel.open(); // 서버소켓을 열고 세팅해주기
            serverSocket.bind(new InetSocketAddress("localhost", 5454));
            serverSocket.configureBlocking(false);
            serverSocket.register(selector, SelectionKey.OP_ACCEPT);
            ByteBuffer buffer = ByteBuffer.allocate(256); // 데이터를 읽고 쓸 버퍼 생성
    
            while (true) {
                selector.select(); // selector가 주시하는 채널들로부터 이벤트 확인
                Set<SelectionKey> selectedKeys = selector.selectedKeys(); // 이벤트가 일어난 채널을 갖고 있는 key 갖고오기 
                Iterator<SelectionKey> iter = selectedKeys.iterator();
                while (iter.hasNext()) { // 루프를 돌면서 이벤트에 따라 처리해주기
    
                    SelectionKey key = iter.next();
    
                    if (key.isAcceptable()) {
                        register(selector, serverSocket); // 클라이언트와 연결이 되면(accept) 클라이언트 소켓 채널을 만들고 selector에 등록
                    }
    
                    if (key.isReadable()) {
                        answerWithEcho(buffer, key); // 데이터를 읽을 수 있다면 읽어서 echo해주기
                    }
                    iter.remove();
                }
            }
        }
    
        private static void answerWithEcho(ByteBuffer buffer, SelectionKey key)
          throws IOException {
     
            SocketChannel client = (SocketChannel) key.channel();
            client.read(buffer);
            if (new String(buffer.array()).trim().equals(POISON_PILL)) {
                client.close();
                System.out.println("Not accepting client messages anymore");
            }
            else {
                buffer.flip();
                client.write(buffer);
                buffer.clear();
            }
        }
    
        private static void register(Selector selector, ServerSocketChannel serverSocket)
          throws IOException {
     
            SocketChannel client = serverSocket.accept();
            client.configureBlocking(false);
            client.register(selector, SelectionKey.OP_READ);
        }
    
    SelectionKey는 말그대로 열쇠와 같다. Selector에 채널들을 등록하면 얻을 수 있으며 SelectionKey로부터 등록과 관련된 다양한 정보를 얻을 수 있다. 예를들면 연결된 채널이나 Selector를 얻을 수 있고 key에 원하는 오브젝트를 붙여줄 수도 있다.
    
    ---
    
    [https://www.baeldung.com/java-nio-selector](https://www.baeldung.com/java-nio-selector)
    
### IO와 NIO의 차이점은?
    - IO는 입출력 방식으로 스트림을 사용하고 NIO는 채널을 사용한다.
        
        스트림은 단방향이고 채널은 양방향이다.
        
    - IO는 넌버퍼 방식이 NIO는 버퍼 방식이다.
        
        IO는 대신에 BufferedReader같이 버퍼 기능이 있는 보조 스트림이 있다.
        
    - IO는 동기 방식이고 NIO는 동기 / 비동기 모두 지원한다.
    - IO는 블로킹 방식이고 NIO는 블로킹 / 논블로킹 모두 지원한다.
        
        입력 스트림의 read() 메소드나 출력 stream의 wirte()를 호출하면 데이터가 입력이나 출력되기 전까지 스레드는 블로킹 됨.
        
        스레드는 interrupt()를 해도 블로킹을 빠져나올 수 없으며 유일한 방법은 스트림을 닫는 것이다.
        
        반면에 NIO의 블로킹은 스레드가 interrupt되면 빠져나올 수 있다.
        
        넌블로킹 방식은 입출력 작업시 스레드가 블로킹도지 않는다.
        
    
    ---
    
    유튜브 - 이것이 자바다 19.1 
    
### 동기 비동기 블로킹 논블로킹의 차이는?
    
    상황에 따라 다르게 쓰일 때가 있지만 주로 이런 의미이다.
    
    동기와 블로킹 방식은 같다. 쓰레드가 요청을 처리할 때까지 대기한다.
    
    논블로킹 방식은 블록되지 않고 만약 결과를 받을 수 없다면 바로 에러를 뱉고 받을 수 있으면 결고를 받는다. 따라서 결과가 준비되어있는지 확인할 방법이 필요하다.
    
    비동기 방식은 결과를 요청하고 블록되지 않고 쓰레드는 진행하고 요청된 결과를 백그라운드에서 처리된다.
    
    ---
    
    [https://stackoverflow.com/questions/2625493/asynchronous-and-non-blocking-calls-also-between-blocking-and-synchronous#:~:text=A nonblocking call returns immediately,complete at some future time](https://stackoverflow.com/questions/2625493/asynchronous-and-non-blocking-calls-also-between-blocking-and-synchronous#:~:text=A%20nonblocking%20call%20returns%20immediately,complete%20at%20some%20future%20time).
    
### 언제 IO 방식을 쓰고 언제 NIO 방식을 써야할까?
    - 연결 클라이언트 수가 적고 전송되는 데이터가 대용량이면서 순차적으로 처리될 필요가 있을 경우 IO를 선택한다.
        
        왜냐하면 NIO는 버퍼를 쓰는데 버퍼의 크기는 정해져 있고 무한정 크게 만들 수 없기 때문에 데이터를 받아서 즉시 처리하는 것이 낫다.
        
    - 연결 클라이언트의 수가 많고, 전송되는 데이터 용량이 적으면서, 입출력 작업 처리가 빨리 끝나는 경우 NIO를 선택한다.
        
        넌블로킹 방식이기 때문에 연결 클라이언트의 수가 많아도 소수의 쓰레드로 처리가능하다.
        
        버퍼를 사용하기 때문에 입출력 성능이 좋다.
        
        왜냐하면 NIO는 쓰레드풀을 사용하는데 하나의 쓰레드가 입출력 작업을 너무 오래 처리하면 다른 일을 할 수가 없기 때문이다.
        
        ---
        
        유튜브 - 이것이 자바다 19.1
        
        [https://alkhwa-113.tistory.com/entry/NIO?category=921157](https://alkhwa-113.tistory.com/entry/NIO?category=921157)
        
### 버퍼란?
    
    버퍼란 읽고 쓰기가 가능한 메모리 배열로 Non-Direct와 Direct로 구분된다.
    
    ---
    
    [https://alkhwa-113.tistory.com/entry/NIO?category=921157](https://alkhwa-113.tistory.com/entry/NIO?category=921157)
    
### Non-Driect 버퍼와 Dircet 버퍼의 차이는?
    
    Direct 버퍼는 JVM 외부에 생성하기 위하여 커널 모드로 전환해야 되기 떄문에 생성시 시간이 오래 걸린다.
    
    대신에 큰 공간을 할당할 수 있고 JVM이 입출력시 OS와 소통하기 위해 중간 버퍼에 non-direct 버퍼등의 내용을 복사해서 옮기는 등에 작업이 필요없기 때문에 빠르다.
    
    일반적으로 Direct 버퍼는 프로그램 성능에 측정 가능할만한 이득을 얻을 수 있을 때 할당하는게 좋다. 
    
    ---
    
    [https://alkhwa-113.tistory.com/entry/NIO?category=921157](https://alkhwa-113.tistory.com/entry/NIO?category=921157)
    
    [https://docs.oracle.com/javase/7/docs/api/java/nio/ByteBuffer.html](https://docs.oracle.com/javase/7/docs/api/java/nio/ByteBuffer.html)