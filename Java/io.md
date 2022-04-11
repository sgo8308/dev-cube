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
    
    Buffered Stream은 버퍼라고 불리는 메모리 영역이 완전히 빌 경우 시스템 콜을 호출하여 데이터를 읽고, 완전히 찰 경우 버퍼가 가득 찰 경우 데이터를 내보낸다.
    
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