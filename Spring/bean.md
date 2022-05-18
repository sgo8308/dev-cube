### BASIC

### 공부해야 하는 이유는?
    
    스프링의 핵심 개념이다.
    
    스프링을 이해하는데 도움을 준다.
    
### 정의는?
    
    스프링 빈이란 스프링 IOC 컨테이너에 의해 관리되는 객체를 의미한다.
    
    ---
    
    [https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction)
    
### 왜 나왔을까?
### 어떤 방식으로 동작할까?
    
    프로그램이 실행되면 스프링 컨테이너(Application Context)가 생성되고 내부에 빈 저장소라고하는 것을 갖고 있다.
    
    빈들은 각각 빈의 이름을 key 참조 객체를 value로 이 빈 저장소에 저장된다.
    

### ADVANCED

### 스프링 컨테이너는 어떻게 빈을 싱글톤으로 관리할까?
    
    바이트코드 조작 라이브러리인 CGLIB을 통해서 Configuration을 담당하는 클래스에 빈 생성 메소드를 조작한 프록시 객체를 만든다.
    
    그리고 이 객체는 만약에 빈이 이미 생성되어 있으면 빈 저장소에 등록되어 있는 빈을 반환하고
    생성되어 있지 않다면 실제 Configuration 객체의 메소드를 실행해서 나온 객체를 반환하라. 와 같은 로직을 갖고 있을 것이다. 
    
    ---
    
    김영한, 스프링 핵심 원리 - 기본편, 인프런, @Configuration과 바이트코드 조작의 마법 파트