### BASIC

### 공부해야 하는 이유는?
    
    이해하고 필요할 때 떠올리고 적용하기 위해서
    
### 정의는?
    
    어떤 메소드에 로직이 있을 때 이 로직을 여러 관점으로 분리한다. 
    
    핵심적인 관점 부가적인 관점 등. 
    
    그 후 관점에 따라 모듈화하여 프로그래밍하는 것을 의미한다.
    
### 사용해야 하는 이유는?
    
    모든 메소드에 공통적으로 적용되는 로직이 있을 때 중복이 생긴다.
    
     AOP를 사용하면 이러한 중복을 없애줄 수 있고 공통된 로직을 한 곳에서 관리할 수 있다.
    
    따라서 코드가 깔끔해지고 책임도 분리가 되어 코드의 변경이 용이해진다.
    
### 어떤 방식으로 동작할까?
    
    데코레이터 패턴을 이용한다. AOP 객체는 데코레이터로써 중간에서 부가적인 기능을 더해준다.
    
### 단점은?
    
    비지니스 로직이 AOP 코드와 섞이기 때문에 AOP가 적용된 코드는 디버깅하는 것이 살짝 힘들다.
    
### AOP 관련 용어
    - PointCut이란?
        
        AOP를 적용할 지점이라는 뜻이다. 
        
        다음과 같은 AOP 메소드가 있으면
        
        @Before("execution(public void aop002.Bot.runSomething())")
        public void before(JoinPoint joinPoint)
        
        public void aop002.Bot.runSomething() 이 부분이 PointCut이다.
        
        PointCut 표현식에 따라 다양한 범위로 지정 가능하다.
        
    - Advice
        
        ‘언제’, ‘무엇을’에 대한 것이다.
        
        ```java
        @Before("execution(public void aop002.Bot.runSomething())")
        public void before(JoinPoint joinPoint)
        ```
        
        위 코드 에서 @Before부분과 public void before(JoinPoint joinPoint)부분을 합쳐서 Advice라고 한다.
        
    - JoinPoint란?
        
        Advice가 적용될 수 있는 지점. 예를 들면 매소드 실행, 생성자 호출, 필드값 접근.
        
        스프링 AOP에서는 프록시 방식을 사용하고 프록시 방식은 인터페이스를 이용하기 때문에 메소드에만 적용이 가능하다. (다른 AOP 프레임워크를 사용하면 위의 예시와 같은 다른 지점에도 적용이 가능하다.)
        
        스프링 AOP에서는 스프링 프레임워크가 관리하는 빈의 모든 메서드가 해당된다.
        
    - Aspect란?
        
        흩어진 관심사를 모듈화한 것
        
        @Aspect가 붙어있는 클래스?
        
        여러 Advice들과 여러 Pointcut들을 모듈화한 것.
        
    - Advisor란?
        
        Advice와 Pointcut이 한개만 존재하는 모듈
        
        스프링 AOP에서만 사용 되는 용어
        
        Aspect가 나왔기 때문에 스프링에서 쓰지 말라고 권고한다.
        
    - Target이란?
        
        Aspect를 적용하는 대상 클래스