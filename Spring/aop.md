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

### 다이내믹 프록시란 무엇이고 무슨 문제 때문에 사용할까?
    
#귀찮음 #중복 #Dispatcher Servlet

프록시 패턴을 이용해서 프록시 객체를 생성하는 방식에는 3가지 문제가 있다.

1. 프록시 대상 인터페이스에 모든 메소드에 대해서 위임하는 메소드를 작성하는 것이 매우 귀찮고, 추후에 새로운 메소드가 추가될 때마다 이 프록시 클래스 또한 같이 추가해주어야 한다.
2. 프록시 대상 인터페이스의 여러 메소드에 대해 동일한 부가 기능이나 접근 제어를 하는 경우가 많기 때문에 중복되는 로직이 프록시 클래스의 각 메소드마다 존재할 수 있다. 또한 같은 부가 기능을 쓰고 싶은 다른 클래스도 존재할텐데 그 때마다 중복되는 로직을 그 해당 클래스의 프록시 클래스에 다시 작성해야 한다.

이러한 문제를 해결하기 위해 다이내믹 프록시라는 것이 사용된다.

다이내믹 프록시는 직접 프록시 클래스를 만드는 것이 아니라 프록시 팩토리를 이요애 런타임에 바로 생성하는 프록시 객체를 말한다. 자바에서는 리플렉션 API에서 제공하는 Proxy 클래스가 프록시 팩토리의 역할을 한다.

Proxy 클래스가 만든 다이내믹 프록시는 모든 메소드를 InvocationHandler의 Invoke(Object proxy, Method method, Object[] args) 메소드로 위임한다. 실제 객체의 메소드에 더해지는 여러 부가 기능들은 이 InvocationHandler의 Invoke메소드를 구현하는 방식으로 구현할 수 있다. 

모든 메소드들이 이 Invoke()메소드 하나로 집중되니 중복되는 로직은 이 메소드 하나에만 적어주면 모든 메소드에 적용하게 된다. 만약 메소드의 리턴 타입이나 이름에 따라서 다른 부가 기능 로직을 적용하고 싶다면 마찬가지로 이 Invoke 메소드에서 분기문을 나누어 적용할 수 있다.

위처럼 다이내믹 프록시를 이용하면 타깃 인터페이스를 구현하는 클래스를 일일이 만드는 번거로움을 제거할 수 있다. 하나의 핸들러 메소드를 구현하는 것만으로도 수많은 메소드에 부가기능을 부여해줄 수 있으니 부가기능 코드의 중복 문제도 사라진다.  또한 InvocationHandler로 부가기능을 더하는 로직을 분리함으로써 이 로직을 사용하고 싶은 다른 여러 클래스의 다이내믹 프록시에도 재사용해서 적용할 수 있게 되었다.

---

토비의 스프링 6.3.2 
    
### 어떻게 스프링이 다이내믹 프록시를 빈으로 만들 수 있게 할까?
    
#팩토리빈

스프링은 FactoryBean이라는 것을 제공한다. 이 FactoryBean 인터페이스를 구현해서 Bean 만드는 로직을 개발자가 만들어줄 수 있다. 이렇게 만들어진 FactoryBean을 빈으로 등록하면, 스프링은 이 FactoryBean에 빈 생성 로직을 이용하여 빈을 만든다. 이 때 타입은 FactoryBean이 아닌 FactoryBean에 의해서 생성된 클래스가 된다.

xml 설정을 보면 이해가 쉽다.

```xml
<bean id="userService" class="springbook.user.service.TxProxyFactoryBean" >
    <property name="target" ref="userServicelmpl" />
    <Property name="transactionManager" ref="transactionManager" />
    <Property name="pattern" value="upgradelevels" />
    <property name="servicelnterface" value="springbook.user.service.UserService" />
</bean>
```

위 xml 설정을 통해 TxProxyFactoryBean이라는 FactoryBean은 “userService”라는 이름과 UserService.class라는 타입을 가진 빈을 생성한다.

따라서 이 FactoryBean의 getObject()라는 메소드를 override하여 Proxy 클래스를 이용한 다이내믹 프록시를 만들어서 return해주도록 구현하면 된다.

이 빈은 TxProxyFactoryBean 타입으로 등록되는 빈이 아니기 때문에 TxProxyFactoryBean을 다른 클래스에도 재사용이 가능하다.

**한계** 

#설정 파일 관리 #중복

트랜잭션과 같이 여러 클래스에서 같은 부가기능을 사용하고 싶을 때가 있다. 이 때는 각각의 클래스에 대해서 팩토리빈을 모두 등록해주어야 한다.  부가 기능을 여러개 사용한다면 그에 따른 팩토리빈을 또 등록해주어야 하기 떄문에 1개의 타겟 클래스를 위해서 여러개의 팩토리빈 설정 코드가 추가될 것이다. 즉 설정 파일이 매우 비대해져 관리하기 어려워진다.

또한 하나의 부가 기능을 더해주는 InvocationHandler를 팩토리빈 마다 각각 만들어주어야 하는 중복 문제도 존재한다. InvocationHandler가 target 클래스를 내부에 인스턴스 변수로 갖고 있기 때문이다.

---

토비의 스프링 6.3.4
