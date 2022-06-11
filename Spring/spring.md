## 스프링 프레임워크
### 공부해야 하는 이유는?
    
    자바 진영의 가장 중요한 기술 중 하나다.
    
    스프링을 공부함으로써 객체지향을 이해할 수 있다.
    
    프레임워크를 이해할 수 있다.
    
### 정의는?
    
    좋은 객체 지향 어플리케이션을 개발할 수 있게 도와주는 프레임워크.
    
    다형성을 극대화 시켜주어 마치 레고 블럭 조립하듯이 구현을 편리하게 변경할 수 있다.
    
    ---
    
    김영한, 스프링 핵심 원리 - 기본편, 인프런,  스프링이란?파트
    
### 왜 나왔을까?
    
    자바 진영에서 표준으로 내세우던 EJB를 이용한 서버 어플리케이션 개발 방식이 너무 복잡하고 어려워서 나왔다.
    
    ---
    
    김영한, 스프링 핵심 원리 - 기본편, 인프런,  이야기 - 자바 진영의 추운 겨울과 스프링의 탄생 파트
    
### 어떤 방식으로 동작할까?
    
    DI를 통해 OCP와 DIP를 지키게 해준다.
    
### 단점은?
    
    잘 사용하려면 스프링을 배워야 하고 러닝커브가 꽤 높다.
    
    EJB보다 쉽다고 해도 여전히 복잡하다..
    
    ---
    
    [https://www.javatpoint.com/java-spring-pros-and-cons](https://www.javatpoint.com/java-spring-pros-and-cons)

## 스프링 부트

### 공부해야 하는 이유는?
    
    요즘엔 거의 스프링 부트를 이용해 스프링을 사용한다.
    
### 스프링 부트란 대체 무엇일까? 
    
    스프링의 확장판이다.

    스프링 기술을 그대로 사용할 수 있으며 스프링 앱 개발을 훨씬 쉽게 만들어준다.

    ---

    https://www.interviewbit.com/blog/spring-vs-spring-boot/#:~:text=To sum up-,Spring Boot contains all of the functionality of the standard Spring framework while also making application development much easier.,-When compared to
    
### 사용해야 하는 이유는?
    
    스프링에 복잡한 설정들을 관례에 따라 간결하게 설정할 수 있도록 해준다.
    
    자동으로 호환되는 라이브러리들을 찾아준다.
    
    톰캣 서버를 내장하고 있어 단독적으로 서버 어플리케이션 실행이 가능하다.
    
    ---
    
    https://www.ibm.com/cloud/learn/java-spring-boot#:~:text=Java Spring Boot
    
### 왜 나왔을까?
    
    빠르게 스프링을 이용한 앱 개발을 시작할 수 있게 만들어 새로운 유저들으 스프링에 많이 유입시키기 위해서 
    
    ---
    
    https://spring.io/blog/2013/08/06/spring-boot-simplifying-spring-for-everyone/#:~:text=Spring Boot aims to make it easy to create Spring-powered%2C production-grade applications and services with minimum fuss. It takes an opinionated view of the Spring platform so that new and existing users can quickly get to the bits they need
    
### 어떤 방식으로 동작할까?
    
    classpath와 bean들을 확인하고 필요한 것들을 추측한 후에 자동으로 넣어준다.
    
    예를 들어 Spring MVC가 클래스패스에 있다면 항상 필요한 빈들이 몇까지 있는데 이 빈들을 자동으로 넣어준다.
    
    ---
    
    https://spring.io/guides/gs/spring-boot/#:~:text=Spring Boot offers,a servlet container%2C
### 단점은?
    
    사용되지 않는 많은 의존성들이 추가될 수 있고 이는 배치 파일의 크기를 키운다.
    
