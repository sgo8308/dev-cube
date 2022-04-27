### 서블릿이란?
    
    서블릿은 JSP 표준이 나오기 전에 만들어진 표준으로 자바로 웹 어플리케이션을 개발할 수 있도록 하기 위해 만들어졌다. 
    
    서블릿을 지원하는 WAS는 TCP 커넥션을 연결하고 HTTP 메시지를 파싱하고 Request Response 객체를 만들어 요청 URL에 맞는 서블릿 객체에 집어넣어준다.
    
    자바 파일이며 HTML이 아닌 자바 코드가 중심이 되기 때문에 Data processing에 좋아서 MVC 아키텍쳐에서 Controller로 사용된다.
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 498p,2021
    
    김영한, 스프링 MVC 1편, 인프런, 서블릿 파트
    
### 서블릿을 배워야 하는 이유는?
    
    MVC 패턴을 지원하는 프레임워크를 만들어야 하는 경우 서블릿으로 기반 코드를 개발하는 경우가 많기 때문에, 서블릿 코드를 직접 구현하지는 않더라도 웹 개발을 배울 때 서블리 자체에 대해서 이해하는 것은 중요하다.
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 499p,2021
    
###언제 annotation 방식을 쓰고 언제 web.xml 설정 방식을 써야할까?
    
    annotation 방식은 간편한 반면 서블릿으로 처리해야 할 url이 변경될 경우 urlPatterns의 내용을 변경한 후 다시 컴파일 해야 한다. 그러나 web.xml 설정 방식을 사용할 경우 web.xml 파일만 변경하면 된다. 따라서 처리해야 할 url이 자주 바뀌냐에 기준이 있을 수 있다.
    
    또한 web.xml은 설정이 한 눈에 보이는 장점이 있고 annotation 방식은 코드와 설정이 같이 있어 누군가에게는 보기 편할 수도 있다.
    
    상황과 기호에 맞춰 선택하면 된다.
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 505p,2021
    
### doGet이나 doPost 같은 메소드는 어차피 오버라이딩해서 쓰는데 부모 메서드에 바디가 존재하는 이유는 뭘까?
### 서블릿 생성과 초기화에 시간이 많이 걸리는데 자주쓰는 서블릿은 미리 생성 해놓으면 좋지 않을까?

### 서블릿 컨텍스트란?
    
    웹 어플리케이션 당 하나씩 존재하는 서블릿 컨테이너와 소통하기 위한 메소드들을 정의해놓은 클래스. 
    
    웹 컨테이너에 의해서 프로젝트를 배포할 때 생성된다.
    
    서블릿 컨텍스트를 통해 web.xml 파일의 설정 정보를 읽을 수 있다. 이런 특성을 이용해서 여러 서블릿이 정보를 교환할 수도 있다.
    
    ---
    
    [https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html)
    
    [https://www.javatpoint.com/servletcontext](https://www.javatpoint.com/servletcontext)
    
### 서블릿 컨텍스트 리스너란?
    
    서블릿 컨텍스트의 라이브 사이클 변화에 대해서 통지를 받는 인터페이스이다.
    
    웹 앱이 초기화될 때 통지를 받아서 어떤 필터나 서블릿보다도 먼저 초기화하고 싶은 부분을 초기화할 수 있다.
    
    또한 서블릿컨텍스트가 종료되려고 할 때 통지를 받아서 모든 서블릿과 필터가 destroy되고 난 후에 하고 싶은 작업을 정의할 수 있다.
    
    ---
    
    [https://docs.oracle.com/javaee/6/api/javax/servlet/ServletContextListener.html](https://docs.oracle.com/javaee/6/api/javax/servlet/ServletContextListener.html)