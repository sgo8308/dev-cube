## 패키지 구조는 Layer 우선 방식과 모듈 우선 방식 중 어떤 방식을 사용할까?
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

### local, dev, staging, prod 환경의 차이는?
    - local - 로컬 개발 환경
        
        각자 개발자 PC의 환경. 모든 개발자가 같은 개발 환경을 사용해야 한다.
        
    - dev - 서버 개발 환경
        
        개발자들이 만든 코드를 합 쳐서 서버 환경에서 테스트해볼 수 있는 환경
        
        prod 환경보다 훨씬 작다. prod가 많은 서버로 구성된다면, 개발 환경은 한 두 개의 서버로 기능 구현이 가능한 정도로 구축한다.
        
    - staging
        
        운영 환경과 거의 동일한 환경을 만들어 놓고, 운영환경으로 이전하기 전에, 여러 가지 비 기능적인 부분 (Security, 성능, 장애등)을 검증하는 환경이다.
        
    - prod - 실제 운영 환경
    
    ---
    
    [https://bcho.tistory.com/759](https://bcho.tistory.com/759)

## 스프링 부트 외부 설정
### 외부 설정이란?
    
    애플리케이션 코드 밖에서 애플리케이션에 대한 설정을 하는 것을 말한다. 
    같은 코드를 사용하더라도 다른 환경(개발, 테스트, 운영)에 있을 때 다른 방식으로 작동시킬 수 있다.
    
    애플리케이션 설정 파일을 application-dev.properties, applciation-prod.properties 두 개를 만들고 
    애플리케이션의 프로파일을 운영에 사용되는 application에는 prod 개발에 사용되는 애플리케이션에는 dev라고 지정하면 
    각각 설정 파일로 application-dev.properties, applciation-prod.properties가 적용된다.
    
    이런 식으로 손쉽게 운영 환경과 개발 환경에서의 설정 파일을 따로 적용할 수가 있다.
    
    ---
    
    [https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config)
    
### Profile이란?
    
    개요라는 뜻으로 이 코드가 운영 환경인지, 개발 환경인지 등을 나타내는데 사용된다. 
    Profile을 통해 각 환경에 맞게 데이터베이스 접속 계정 및 옵션, 리소스, 로그 관리 정책 등을 다르게 가져갈 수 있다.
    
    ---
    
    [https://jsonobject.tistory.com/220](https://jsonobject.tistory.com/220)
    
### Environment란?
    
    스프링의 모든 설정이 map 형태로 담겨 있는 구현체들의 인터페이스
    
    이것을 이용해 설정 정보를 얻을 수 있다

## application.properties
### BASIC

### 얼마나 공부해야할까?
    
    application.properties가 무엇인지 정확히 이해하고 잘 설정 할 수 있는 정도
    
### 정의는?
    
    프로퍼티라는 것은 단순히 key와 value 형태로 제공하는 데이터이다.
    
    이러한 데이터를 애플리케이션 상에서 참고할 수 있다. 
    
    스프링 부트는 이 값을 설정할 수 있는 파일로 application.properties, application.yml을 지원하며 이외 다른 방식도 지원한다.
    
    스프링 부트 앱에 설정들을 적어놓는 곳이다. DB 커넥션 관련 설정이나 로그에 관한 설정 등을 적는다. 스프링부트가 애플리케이션을 구동할 때 자동으로 로딩한다. 
    
### 잘 사용하려면 어떻게 해야할까?
    
    스프링 부트는 기본적으로 application.properties에 있는 모든 속성을 로드한다. 그 후 active된 Profile에 맞는 application-dev.properties와 같은 속성을 로드한다.
    
    그러므로 profile에 상관없는 공통적인 속성은 application.properties에 놓고 profile-specific한 속성은 application-dev.properties와 같은 파일에 놓자.
    
    ---
    
    [https://www.baeldung.com/spring-profiles](https://www.baeldung.com/spring-profiles)
    
### 단점은?
    
    같은 prefix를 가지고 있는 설정들도 설정마다 모두 적어줘야 하므로 가독성이 떨어지고 귀찮다.
    
    ex.
    
    application.server.ip=’127.0.0.1’
    
    application.servier.path=’/path1’
    
### application.yml이란?
    
    application.yml는 계층 구조로 표현할 수 있어 가독성이 좋기 때문에 application.properties 대신 사용된다.

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
    
### 무엇을 로깅해야할까?
    DEBUG : 개발 과정에서 체크하고 싶은 것들 

    INFO : 서비스의 목적 달성을 성공적으로 하는지 분석 및 확인하는 용도.

    ERROR : ‘의도하지 않은’ 오류를 명시적으로 표현. 예외 상황이지만 의도한 상황이라면 INFO로 작성.
             이렇게 할 경우 의도하지 않은 경우만 존재하기 때문에 빠르게 ERROR만 확인함으로써 장애 원인 파악이 가능하다.

    ---

    https://blog.lulab.net/programmer/what-should-i-log-with-an-intention-method-and-level/
    
### 어떤 방식으로 동작할까?
### 잘 사용하려면 어떻게 해야할까?
    1. System.out.println()을 하기보다는 로깅 라이브러리를 사용한다.
    2. 로그가 저장되는 저장소의 용량과 로그 삭제 계획을 명확하게 수립하여 디스크 용량 부족과 같은 갑작스러운 장애를 방지한다.
    3. 민감정보를 로그에 적지 않는다. 개인정보법을 위배할 수도 있다.
    
    ---
    
    https://blog.lulab.net/programmer/what-should-i-log-with-an-intention-method-and-level/
    
### slf4j(Simple Logging Facade for Java)란?
    
    다양한 로깅 라이브러리가 있다. 이 라이브러리들을 통일된 인터페이스로 사용할 수 있도록(어댑터 등을 다 구현해서)나온 것이 slf4j이다.
    
    다양한 로깅 프레임워크의 통일된 인터페이스이다.
    
### logback이란?
    
    slf4j의 구현체이다. 실무에서 스프링 부트가 기본으로 제공한다.


