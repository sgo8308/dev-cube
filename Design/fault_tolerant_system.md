### Fault Tolerant System이란?
    
    장애 내성 시스템이라는 뜻이다.
    
    시스템의 구성요소 중에 하나에 장애가 생겨도 정상적으로 작동할 수 있는 시스템을 말한다.  이런 시스템은 장애의 정도와 성능 저하의 정도가 비례한다. 이와 반대되는 시스템은 사소한 장애에도 시스템 전체가 무너지게 되는 시스템을 말한다.
    
### Fail-Fast란?
    
    실패의 징조가 있을 때 그 실패에 대한 보고를 바로하거나 바로 실패를 일으켜버리는 시스템을 말한다.
    
    **의존성**에 관한 문제 때문에 발생하는 문제를 해결하기 위해 나온 개념이다.
    
    여러 모듈로 구성된 시스템에서 어떤 모듈에 문제가 생기면 그 문제는 다른 모듈로 전파가 된다. 이 때 문제가 생긴 모듈을 의존하는 모듈이 뭔가 심상치 않음을 감지하면 (5번 요청이 실패한다거나) 이 요청을 실패시키고 더 이상 요청을 진행하지 않는 것을 말한다.
    
    자신도 장애가 생기기 전에 문제 있는 요청을 빠르게 실패해버리는 것이다.
    
    예를 들면 서킷 브레이커로 구현 가능하다.
    
    ---
    
    [https://en.wikipedia.org/wiki/Fail-fast](https://en.wikipedia.org/wiki/Fail-fast)
    
## 서킷 브레이커
    
    #의존성 #상태 머신
    
### 서킷 브레이커란?
    
    과잉의 전류가 흘렀을 때 과열로 인해 손상되는 것을 방지하기 위해 회로를 차단하는 장치를 부르는 말이다.
    
    서버 맥락에서 보면 서킷 브레이커는 한 서버가 어떤 서버로 요청을 진행하는데 이 요청의 실패가 많아져 특정 threshold를 넘게 되면 더 이상 요청을 하지 않게 만들어준다.
    
    **의존성** 때문에 발생하는 문제를 해결하기 위한 방법이다.
    
    A서버가 B서버에 의존하고 있을 때 B서버에서 장애가 발생하거나 더 이상 요청을 받을 수 없는 경우에 A서버에서 B서버로의 요청은 계속해서 실패하게 된다. 
    이 경우 A서버 또한 장애로 이어지게 된다. 이 때 A서버에서 서킷 브레이커로 감싸서 요청을 실행하면 B서버로의 요청이 특정 횟수 이상 실패하게 되면 요청을 멈춘다. 
    서킷 브레이커가 발동했을 때 특정 방식으로 처리하게끔 구현하고 알람을 받는 식으로 문제 상황을 바로 파악할 수 있다.
    
    서킷 브레이커는 Closed Open Half-Open 3가지의 상태를 갖는다. 
    서킷 브레이커가 Closed 상태일 때는 정상적으로 요청이 진행되며 Open은 서킷 브레이커가 작동한 상태로 서버가 요청 메소드를 실행해도 요청이 정상적으로 진행되지 않는다. 
    Half-Open은 반만 진행된 상태로 이 때 원격 서버로 요청을 진행했는데 요청이 성공한다면 Closed로 바뀌고 실패하면 다시 Open으로 돌아간다.
    
    
    ---
    
    [https://martinfowler.com/bliki/CircuitBreaker.html](https://martinfowler.com/bliki/CircuitBreaker.html)
    
### 어떤 식으로 사용할까?
    
    Iface helloClient = new ClientBuilder("tbinary+http://127.0.0.1:8080/hello")
                         .decorator(
                          CircuitBreakerClient.newDecorator(
                              new CircuitBreakerBuilder("hello").build()
                          )
                         )
                         .build(Iface.class);
    
    이런 식으로 helloCloent에 “hello”라는 메소드에 서킷 브레이커를 건다.
    
    try {
        helloClient.hello("line");
    } catch (TException e) {
        // error handling
    } catch (FailFastException e) {
       // fallback code
    }
    
    서킷브레이커가 발동되면 메소드가 진행되지 않고 FailFastExcepion과 같은 예외를 던지며 이러한 예외를 처리하는 코드를 적어주면 된다.
    
    ---
    
    [https://engineering.linecorp.com/ko/blog/circuit-breakers-for-distributed-services/#:~:text=Circuit Breaker for Armeria](https://engineering.linecorp.com/ko/blog/circuit-breakers-for-distributed-services/#:~:text=Circuit%20Breaker%20for%20Armeria)
        
### Rate Limit이란?
    
    Rate(비율,속도)를 Limit(제한)하는 것을 의미한다. 서버 맥락에서 하나의 서버가 너무 많은 요청을 받게 되면 죽을 수 있다. 
    이 때 일정량 이상의 요청을 넘어서면 더 이상 요청을 받지 않도록 하거나 큐에 넣어 놓고 나중에 처리한다던지 하는 방식으로 처리하는 것을 의미한다. 
    요청을 받는 것만 얘기했지만 요청을 보내는 것을 제한하는 것과도 관련이 있다.
    
    예시로는 nginx의 rate limit이나 resilience4j의 rate limiter등이 있다.
    
    확장성 문제와 연관이 있다. 서버가 요청을 많이 받을 경우에 대비해 서버가 충분히 확장이 되어 있거나 요청이 많이 받을 때 확장이 되야하는데 
    그러지 못한 경우에 차선책으로 삼을 만한 방안.
    
    ---
    
    [https://resilience4j.readme.io/docs/ratelimiter](https://resilience4j.readme.io/docs/ratelimiter)
    
    [https://en.wikipedia.org/wiki/Rate_limiting](https://en.wikipedia.org/wiki/Rate_limiting)