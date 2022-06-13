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


## 서블릿 예외처리
### 공부해야 하는 이유는?
    
    스프링의 예외처리는 서블릿의 예외처리를 위에서 돌아가기 때문에 서블릿의 예외처리 방식을 알아야 여러가지 오류 상황에 대처할 수 있다.
    
### 순수한 서블릿 컨테이너는 예외를 어떤 식으로 처리할까?
    
    컨트롤러에서 발생한 예외가 순수 서블릿 컨테이너까지 전파되었을 때는 상태코드 500과 HTTP Status 500 - Internal Server Error라는 메시지를 화면에 뿌려준다.
    
    컨트롤러에서 response.sendError(404, “404오류!”) 와 같이 메소드를 사용해서 예외를 등록하면 서블릿 컨테이너는 response 객체에서 에러가 등록되었는지 확인하고 등록된 상태코드와 그에 맞는 메시지를 화면에 뿌려준다.
    
### 순수한 서블릿 컨테이너에서 원하는 예외 페이지를 일관되게 보여주려면 어떻게 할까?
    
    서블릿이 제공하는 기능을 사용하면 된다.
    
    web.xml에 <error-page>라는 태그를 통해 오류코드와 errorpage를 설정하면, 설정된 오류코드에 맞는 errorpage를 서블릿 컨테이너가 요청을 한다.
    
    또는 스프링 부트를 사용한다면 다음과 같은 방식으로 설정이 가능하다.
    
    //서블릿 컨테이너를 커스터마이징해주는 클래스
    @Component
    public class WebServerCustomizer implements WebServerFactoryCustomizer<ConfigurableWebServerFactory> {
    
        //서블릿 컨테이너가 오류코드에 맞는 에러 페이지를 나타내도록 설정하는 메소드
        @Override
        public void customize(ConfigurableWebServerFactory factory) {
            ErrorPage errorPage404 = new ErrorPage(HttpStatus.NOT_FOUND, "error-page/404");
            ErrorPage errorPage500 = new ErrorPage(HttpStatus.INTERNAL_SERVER_ERROR, "error-page/500");
            ErrorPage errorPageEx = new ErrorPage(RuntimeException.class, "error-page/500");
            factory.addErrorPages(errorPage404, errorPage500, errorPageEx);
        }
    }
    
    이 때 동작 흐름은 다음과 같다.
    
    WAS → 필터 → 서블릿(예외발생 혹은 sendError()) → 필터 → WAS(오류코드 확인 후 해당하는 error-page 요청) 
    → 필터 → 서블릿(errorpage에 해당하는) → 필터 → WAS → 고객
    
    이 때 필터가 처음에만 호출되고 error 페이지를 다시 요청할 때는 안 거쳐도 되는데 중간에도 계속 중복으로 거치게 된다.
    
    이 때 DipatcherType이라는 것으로 이 요청이 고객의 요청인지 아니면 Error페이지를 요청하는 것인지를 구분할 수 있고 필터는 이 DispathcerType을 통해 필터링을 진행할 것인지 아닌지 결정할 수 있다.
    
    한 편 스프링을 사용하면 중간에 인터셉터를 거치게 되고 이 인터셉터 또한 중복으로 호출되는 문제가 있다. 하지만 인터셉터는 정교하게 urlpattern을 정할 수 있으므로 errorpage가 있는 url은 거르는 식으로 문제를 해결할 수 있다.