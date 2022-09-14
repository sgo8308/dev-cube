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

### 프록시 팩토리 빈은 무엇이고 팩토리 빈을 직접 구현하는 것과 비교하여 어떤 문제를 해결해줄까?
  
#재사용 #확장

팩토리 빈을 직접 구현하여 다이내믹 프록시를 적용하는 방법은 InvocationHandler를 구현한 TransactionHandler와 같은 것들을 모든 팩토리 빈에서 생성해주어야 한다. 왜냐하면 TransactionHandler가 target 클래스를 직접 참조하고 있기 때문이다.  

또한 어떤 메소드에 이 부가기능을 적용할지에 대한 알고리즘이 각 부가기능 로직을 가진 InvocationHandler마다 중복해서 적어주어야 한다.

스프링에서 제공하는 ProxyFactoryBean을 이용하면 이 단점을 극복할 수 있다. 

이 ProxyFactoryBean은 InvocationHandler대신 비슷한 역할을 하는 MethodInterceptor라는 것을 사용한다. 그리고 MethodInterceptor는 Invoke메소드의 파라미터로 Method 대신 MethodInvocation을 받는다. MethodInvocation은 자신이 직접 target를 참조하고 위임하는 역할을 하게 된다. MethodInterceptory는 일종의 템플릿이 되고 MethodInvocation은 콜백인 셈이다. 이로 인해 MethodInterceptor는 순수하게 부가 기능을 더해주는 역할만 하게 되고 target으로부터 자유로워진다.

따라서  MethodInterceptor를 스프링의 빈으로 등록하고 재사용할 수 있다. MethodInterceptor는 Advice라는 인터페이스를 상속하며 이로 인해 ‘Advice’라고 불린다.

한 편 클래스의 어떤 메소드를 선택할지 정하는 부분 또한 InvocationHandler에서 구현하고 있었는데, InvocationHandler를 대신하는 MethodInterceptor가 스프링 빈으로 싱글톤의 형태로 등록됨으로써 위 기능을 할 수가 없어졌다. 공유 객체가 하나의 클래스에 의존하는 기능을 가질 순 없기 때문이다. 때문에 어떤 메소드에 부가 기능을 적용할지 선택하는 클래스로 따로 불리해냈고 이를 ‘Pointcut’이라고 부른다. 

Advice와 Pointcut을 조합하여 사용하게 됨으로써 위에서 제시했던 메소드 선택 알고리즘 중복 문제 또한 해결하게 되었다.

Advice는 단독으로 전달되거나 Pointcut과 묶여서 Advisor의 형태로 전달된다.

---

토비의 스프링 6.4

### 빈 후처리기(BeanPostProcessor)란 무엇이고 무슨 문제 때문에 사용할까?
    
#중복 #복잡

빈 후처리기는 빈이 생성된 후에 실제로 빈 저장소에 등록되기 전에 빈에 대한 조작을 할 수 있는 빈이다. 객체에 메소드를 호출하거나 다른 객체로 바꿔치기 또한 가능하다.

빈 후처리기를 사용하면 프록시 팩토리빈이 가졌던 2가지 문제를 해결해줄 수 있다.

1. Componant Scan 방식에는 프록시를 사용할 수 없었던 문제.
2. 프록시 적용을 원하는 클래스마다 설정을 추가로 해주어야 하는 귀찮고 중복되는 문제. xml이라면 프록시 팩토리 빈을 등록해주어야 하고 자바 코드 설정 방식이라면 ProxyFactory를 이용해서 설정해주는 코드를 각 클래스 생성 코드마다 넣어주어야 함.

빈 후처리기를 이용하면 모든 빈들이 일단 만들어진 후 이 빈후처리기로 들어오기 때문에 프록시로 생성하고 싶은 빈들을 골라서 프록시로 대체할 수 있다. 

다음과 같이 사용한다.

```java
public class PackageLogTraceProxyPostProcessor implements BeanPostProcessor {
 private final String basePackage;
 private final Advisor advisor;
 public PackageLogTraceProxyPostProcessor(String basePackage, Advisor advisor) {
     this.basePackage = basePackage;
     this.advisor = advisor;
 }

 @Override
 public Object postProcessAfterInitialization(Object bean, String beanName)
     throws BeansException { //객체 초기화가 일어난 후 처리하는 메소드
 
     //프록시 적용 대상 여부 체크
     //프록시 적용 대상이 아니면 원본을 그대로 반환
     String packageName = bean.getClass().getPackageName();
     if (!packageName.startsWith(basePackage)) {
         return bean;
     }
 
    //프록시 대상이면 프록시를 만들어서 반환
     ProxyFactory proxyFactory = new ProxyFactory(bean);
     proxyFactory.addAdvisor(advisor);
     Object proxy = proxyFactory.getProxy();
     return proxy;
 }
} 
```

참고로 @PostConstruct는 빈후처리기가 처리해준다.

---

  인프런, 김영한, 스프링 핵심 원리 고급편 - 빈 후처리기
    
### 스프링이 직접 제공하는 프록시용 빈후처리기는 어떤 식으로 동작할까?
    
스프링 부트는 AopAutoConfiguration을 통해 자동으로 AutoProxyCreator라는 프록시용 빈후처리기를 등록한다. 

이 빈후처리기는 등록된 Advisor들을 모두 가져온 후 포인트컷을 이용해서 각각 빈마다 적용할 메소드가 있는지 파악한다. 만약 적용할 메소드가 있다면 이 빈은 프록시로 생성하고 Advisor를 프록시에 등록해준다.

중요한 점은 포인트컷이 부가기능을 적용할 메소드를 선정하는 역할과 동시에 프록시로 만들 클래스를 정하는 역할도 수행한다는 점이다. 

예를 들면 이런 식이다.

```java
@Bean
public Advisor advisor3(LogTrace logTrace) {
 AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
 pointcut.setExpression("execution(* hello.proxy.app..*(..)) && //어떤 클래스에 적용할지
  !execution(*hello.proxy.app..noLog(..))"); // 어떤 클래스를 뺄지
 LogTraceAdvice advice = new LogTraceAdvice(logTrace);
 //advisor = pointcut + advice
 return new DefaultPointcutAdvisor(pointcut, advice);
}
```

또한 여러 포인트컷에 의해 프록시로 만들 빈으로 여러번 선택되더라도 프록시는 하나만 생성된다는 점이다. 그 후 Advisor 리스트에 적용될 Advisor를 추가하게 된다.

---

 인프런, 김영한, 스프링 핵심 원리 고급편 - 빈 후처리기
    
### JDK Proxy vs CGLIB
    
둘 다 다이내믹 프록시를 만드는데 사용되는 기술이다.

JDK Proxy는 인터페이스 기반으로 프록시를 만들어주며 CGLIB는 구체 클래스 기반으로 프록시를 만들어준다. 따라서 CGLIB에는 여러가지 제약사항이 있다. 만약 프록시 대상 클래스가 final이거나 기본 생성자가 없다면 프록시를 만들 수 없다.
    
### 프록시 팩토리란 무엇이고 무슨 문제를 해결해줄까?
    
**문제 상황**

- 프록시를 상황 별로 다른 방식을 통해 만들어야 하고(복잡함) 부가기능을 제공하는 클래스를 중복(귀찮음)으로 만들어야 한다.
    
    프록시를 만드는 방법에는 2가지가 있다. 
    
    1. 타겟 클래스가 구현한 인터페이스를 implements해서 만드는 법.
    2. 타겟 클래스를 바로 상속해서 만드는 법.
    
    1번 은 JDK Proxy의 방식이고 2번은 CGLIB의 방식이다. 타겟 클래스의 상황에 맞춰서 선택해야 한다.
    
    또 1번은 JDK Proxy는 InvocationHandler을, CGLIB은 MethodInterceptor를 부가기능을 더해주는 클래스로 사용한다. 따라서 동일한 부가 기능에 대해 2가지 종류의 클래스를 중복해서 만들어야 한다.
    

**해결**

- PrxoyFactory로 프록시 만드는 방식을 추상화하고 Advice를 통해 부가기능을 제공하는 방식을 추상화했다.
    
    비슷한 기능을 제공하지만 구현 방법이 다르고 인터페이스 또한 다른 상황이다.
    스프링은 통일된 방식으로 상황에 따라 적절한 방법으로 프록시를 만들기 위해 ProxyFactory라는 클래스로 프록시 만드는 기능을 추상화했다. ProxyFactory는 타겟 클래스 상황에 따라 JDK Proxy 또는 CGLIB을 이용하여 프록시를 만든다.
    
    또한 InvocationHandler와 MethodInterceptor 또한 Advice라는 클래스로 추상화했다. 개발자는 Advice에 로직을 한번만 만들어두면 된다. InvocationHandler나 MethodHandler가 내부적으로 Advice를 호출한다.
      