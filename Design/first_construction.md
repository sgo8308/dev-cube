### 패키지 구조는 Layer 우선 방식과 모듈 우선 방식 중 어떤 방식을 사용할까?
    - Layer 우선 방식
        - 정의
            
            Service, Domain, Repository와 같은 계층(Layer)에 따른 분류를 먼저한다. 
            
            예를 들면 다음과 같은 방식이다.
            
            - com.jiwoo.domain.modulename
            - com.jiwoo.service.modulename
            - com.jiwoo.repository.modulename
        - 장점
            - ?
        - 단점
            - MSA와 같이 모듈 단위로 분리할 때 어려움이 있다.
            - 하나의 기능에 포함되는 클래스들이 여러 폴더에 분산되어 코드를 탐색하는데 시간이 많이 들어 생산성이 떨어진다.
    - 모듈 우선 방식
        - 정의
            
            order, member와 같이 모듈에 따른 분류를 먼저 한다.
            
            예를 들면 다음과 같은 방식이다.
            
            - com.jiwoo.order.domain
            - com.jiwoo.order.service
            - com.jiwoo.member.domain
            - com.jiwoo.member.service
        - 장점
            - MSA와 같이 모듈 단위로 분리할 때 유리하다.
            - 변경의 주기가 같은 것들이 묶여서 변경이 일어났을 때, 이 폴더 저 폴더로 옮겨 다니면서 작업할 필요가 없어 생산성을 올려준다.
        - 단점
            ?

    장점이 많은 모듈 우선 방식을 사용하는 것이 좋을 것 같다.
    
    대용량 서비스는 MSA로 나아갈 확률이 크고 그 때 모듈 우선 방식이 훨씬 MSA를 적용하기 쉽기 때문이다. 
    
    ---
    
    [http://www.javapractices.com/topic/TopicAction.do?Id=205](http://www.javapractices.com/topic/TopicAction.do?Id=205)


## 로깅
### 공부해야 하는 이유는?
    
    로그를 잘 적기 위해서
### 어느 정도까지 공부해야할까?
    
    왜 로깅을 하는지 로깅을 어떻게 하면 잘할 수 있을지 알고 잘 로깅할 수 있는 정도.
    
### 정의는?
    
    소프트웨어 상에서 일어나는 일들에 대한 기록.
    
### 로깅해야 하는 이유는?
    1. 서비스의 동작 상태 파악해서 성능에 관한 통계와 정보를 제공할 수 있다.
    2. 장애가 생겼을 때 장애가 생긴 당시의 정보를 제공해준다. 
    3. 로그에 특정 메시지가 나타날 때 알림을 받을 수 있다.
    
    ---
    
    [https://blog.lulab.net/programmer/what-should-i-log-with-an-intention-method-and-level/](https://blog.lulab.net/programmer/what-should-i-log-with-an-intention-method-and-level/)
    
- 무엇을 로깅해야할까?
    
    
### 어떤 방식으로 동작할까?
### 잘 사용하려면 어떻게 해야할까?
    1. System.out.println()을 하기보다는 로깅 라이브러리를 사용한다.
    2. 로그가 저장되는 저장소의 용량과 로그 삭제 계획을 명확하게 수립하여 디스크 용량 부족과 같은 갑작스러운 장애를 방지한다.
    3. 민감정보를 로그에 적지 않는다. 개인정보법을 위배할 수도 있다.
    
    ---
    
    [https://blog.lulab.net/programmer/what-should-i-log-with-an-intention-method-and-level/](https://blog.lulab.net/programmer/what-should-i-log-with-an-intention-method-and-level/)
    
### slf4j(Simple Logging Facade for Java)란?
    
    다양한 로깅 라이브러리가 있다. 이 라이브러리들을 통일된 인터페이스로 사용할 수 있도록(어댑터 등을 다 구현해서)나온 것이 slf4j이다.
    
    다양한 로깅 프레임워크의 통일된 인터페이스이다.
    
### logback이란?
    
    slf4j의 구현체이다. 실무에서 스프링 부트가 기본으로 제공한다.