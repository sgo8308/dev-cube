### Web Reactive와 Web Servlet의 차이를 블록킹, 논블록킹, 동기화, 비동기화의 관점에서 설명해주세요.
    
    Web Reactive는 Spring에서 web 애플리케이션을 만들기 위해 지원하는 기술 중 Reactive API를 사용하는 Webflux를 가르킨다.
    
    Webflux는 non bolcking io를 지원하여 적은 쓰레드와 적은 하드웨어 자원으로 동시성을 처리한다.
    
    blocking synchronyous 방식을 사용하는 Web Servlet - Spring MVC에서 쓰레드를 보통 코어보다 많게 만드는 이유는
    쓰레드가 I/O를 만나면 block되고 스위칭될 다른 쓰레드가 없다면 I/O 작업이 끝날 때까지 이 쓰레드를 담당하는 코어가 놀기 때문이다.
    
    하지만 non blocking 방식은 block될 일이 없어 모든 쓰레드가 항상 코어를 사용한다.
    즉 코어가 놀 일이 없어져 코어 갯수정도의 쓰레드만으로 CPU자원을 최대한 사용할 수 있다.