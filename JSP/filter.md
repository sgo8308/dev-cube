## BASIC

### 공부해야 하는 이유는?
    
    스프링 시큐리티에 경우 서블릿 필터를 내부적으로 사용하기도 하고
    
    ---
    
    [https://docs.spring.io/spring-security/site/docs/3.0.x/reference/security-filter-chain.html](https://docs.spring.io/spring-security/site/docs/3.0.x/reference/security-filter-chain.html)
    
### 정의는?
    
    일종의 수문장이라고 할 수 있다.
    
    싱글톤 객체의 형태로 존재하면서 클라이언트에서 오는 요청과 JSP나 서블릿 사이에서 클라이언트의 요청 정보를 알맞게 변경하기도 하고 다음 단계로 보내지 않고 다른 자원의 결과를 클라이언트에게 전송할 수도 있다.
    
    또한 최종 자원과 클라이언트로 가는 응답 사이에 위치하여 최종 자원의 요청 결과를 알맞게 변경할 수도 있다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e4318b69-3e25-489d-b189-7decf1a96089/Untitled.png)
    
    ---
    
    김영한, 스프링 MVC 2편, 인프런, 서블릿 필터 - 소개 파트 
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 544-545,2021
    
### 사용해야 하는 이유는?
    
    필터를 사용하면 JSP/서블릿 등을 실행하기 이전에 요청이 올바른지 또는 자원에 접근할 수 있는 권한을 가졌는지의 여부를 미리 처리할 수 있다.
    
    필터를 사용하지 않는다면 모든 get이나 post 등 요청을 받는 곳에서 인증 여부등을 체크해야 할 것이다. 이것은 개발자가 실수할 여지도 있고 코드도 깔끔하지 못하다.
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 545p,2021
    
    김영한, 스프링 MVC 2편, 인프런, 서블릿 필터 - 소개 파트 
    
### 어떤 방식으로 동작할까?
    
    javax.servlet.Filter 인터페이스를 상속한 후 원하는 로직을 doFilter()메소드에 구현한다. 그 후 이 필터를 web.xml 또는 @WebFilter 어노테이션(필터 순서 조절이 안됨)을 이용하여 등록하면 서블릿 컨테이너가 필터를 싱글톤 객체로 생성하고 관리한다. 스프링 부트를 사용한다면 configuration 클래스에 FilterRegistrationBean을 사용해서 등록하면 된다. 

    WAS는 서블릿보다 필터를 먼저 호출한다. 정확히는 필터의 doFilter를 호출한다. 이 후 필터 체인에 모든 필터를 거치고 더 이상 필터가 없으면 서블릿이 마지막 필터에 의해 호출된다.

    URL로 매핑할 수도 있고 서블릿 이름으로 매핑할 수도 있다.

    요청 처리  

    : HTTP 요청 → WAS →  A 필터 →  B 필터(A 필터 중간에 호출) → 서블릿 → 컨트롤러

    응답 처리 

    :  HTTP 응답 ← WAS ←  A 필터(A 필터 끝) ←  B 필터(B 필터 끝) ← 서블릿 ← 컨트롤러

    이 때 사용되는 코드는 이와 같다.

    public void doFilter(ServletRequest request, ServletResponse response,
                             FilterChain chain)
                             throws IOException, ServletException{
    //0. Http 뿐만 아니라 다른 요청을 위해 ServletRequest로 되어 있으므로 더 많은
    //   쓰기 위해 HttpRequest로 다운 캐스팅한다.
    HttpServletRequest httpRequest = (HttpServletRequest) request;
    //1. httpRequest를 이용하여 요청의 필터 작업 수행
    ...

    //2. 체인의 다음 필터 처리. 다음 필터가 있으면 다음 필터를 호출하고 없으면 서블릿을 호출
    chain.doFilter(request, response);

    //3. response를 이용하여 응답의 필터링 작업 수행
    ...

    위와 같이 이루어지기 때문에 필터링은 요청과 응답 때 동일한 필터 객체가 담당하고 필터 적용 순서는 반대로 된다.
    
    ---
    
    김영한, 스프링 MVC 2편, 인프런, 서블릿 필터 - 소개 파트 
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 547-548, 2021
    
### 주로 어디에 사용될까?
    
    사용자 인증
    
    캐싱 필터
    
    자원 접근에 대한 로깅
    
    응답 데이터 변환(HTML 변환, 응답 헤더 변환(ex캐릭터 인코딩 처리), 데이터 암호화 등)
    
    공통 기능 실행
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 557p,2021
    

## Advanced

### AOP를 사용해도 되는데 굳이 필터를 사용하는 이유는?
    
    웹과 관련된 공통 관심사는 HTTP의 헤더나 URL의 정보들이 필요한데, 서블릿 필터나 스프링 인터셉터는 ‘HttpServletRequest’를
    제공하고 그외에 여러가지 웹과 관련된 부가기능을 제공해주기 때문에 웹과 관련되었을 때는 AOP보다는 필터를 사용하는게 좋다. 
    
    ---
    
    김영한, 스프링 MVC 2편, 인프런, 서블릿 필터 - 소개 파트