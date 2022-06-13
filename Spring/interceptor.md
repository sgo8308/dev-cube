### BASIC

### 스프링 인터셉터란?
    
    서블릿 필터와 같이 웹과 관련된 공통 관심 사항을 효과적으로 해결할 수 있는 스프링 MVC가 제공하는 기술.
    
### 사용해야 하는 이유는?
    
    필터와 마찬가지로 웹과 관련된 공통 관심 사항을 효과적으로 해결하기 위해 사용한다.
    
### 어떤 방식으로 동작할까?
    
    인터셉터를 Configuration 클래스에 등록하면 DispatcherServlet은 컨트롤러(핸들러)를 호출하기 전에 for문을 돌면서 인터셉터의 prehandle을 호출하고 더 이상 인터셉터가 없다면 컨트롤러(핸들러)를 호출한다.
    
    컨트롤러의 처리가 다 끝나면 DispatcherServlet은 ModelAndView를 반환받는다.
    
    그 후 DispatcherServlet은 View를 호출하기 전에 인터셉터의 postHandle()을 등록된 순서의 반대로 호출한다.
    (따라서 필터와 마찬가지 순서로 필터링이 적용된다)
    
    그 후 View가 렌더링 된 이후에 DispatcherServlet은 인터셉터의 afterCompletion()을 호출한다.
    
    이 때 만약 컨트롤러(핸들러)에서 예외가 발생했다면 postHandle은 호출되지 않고 afterCompletion()만 호출된다.
    
    ---
    
    org.springframework.web.servlet.DispatcherServlet doDispatch() 코드
    

### ADVANCED

### 잘 사용하려면 어떻게 해야할까?
    
    예외가 발생하면 postHandle()이 호출되지 않으므로 예외와 무관하게 공통처리를 하고 싶다면 afterCompletion()에 코드를 작성한다.
    
    preHandle() → postHandle() → afterCompletion()  방향으로 정보를 주고 받고 싶다면 request 객체에 정보를 세팅해준다.
    
### 필터 vs 인터셉터
    
    필터는 Request와 Response 객체를 다른 Request Response 객체로 바꿔치기 할 수 있다.
    (chain.doFilter()를 할 때 인자고 다른 Request, Response를 전달하면 된다)
    
    필터를 사용하면 Spring MVC에 종속적이지 않을 수 있다.
    
    인터셉터는 url 패턴 지정시 원하지 않는 url 패턴을 지정하기가 쉽다.
    
    인터셉터는 어떤 컨트롤러(handler)가 호출되는지 정보를 받을 수 있고 어떤 modelAndView가 반환되는지도 받을 수 있다.
    
    **결론**
    
    인터셉터는 스프링 MVC에 특화된 필터 기능을 제공한다고 보면 된다. 
    
    예룰 들어 공통적인 핸들러 코드를 뽑거나 권한 체크 같은 미세한 handler 관련 전후처리를 하고 싶다면 인터셉터를 사용하고, multipart 형태나 GZIP 압축 같이 리퀘스트 컨텐트나 뷰 컨텐트를 핸들링 하는 작업은 필터를 사용한다.
    
    하지만 스프링 MVC를 사용하고, 특별히 필터를 꼭 사용해야 하는 상황이 아니라면 인터셉터를 사용하는 것이 편리하다.
    
    ---
    
    [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerInterceptor.html#:~:text=As a basic,to all requests](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerInterceptor.html#:~:text=As%20a%20basic,to%20all%20requests).
    

---

김영한, 스프링 MVC 2편, 인프런, 스프링 인터셉터 파트