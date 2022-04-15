### 웹 서버란?
    
    하드웨어적으로는 웹서버 소프트웨어와 HTML, CSS, JS 파일 등을 갖고 있는 컴퓨터를 말한다.
    
    소프트웨어적으로는 URL을 이해하고 HTTP 프로토콜을 이해해서 유저의 요청을 처리하고 유저가 원하는 컨텐츠를 전달해줄 수 있는 프로그램을 말한다.
    
    ---
    
    [https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
    
### 정적 웹 서버와 동적 웹 서버의 차이는?
    
    요청을 받았을 때 가지고 있는 파일을 그대로 보내주면 정적 웹 서버다.
    
    동적 웹 서버는 정적 웹 서버와 추가적인 소프트웨어로 구성되어 있다. 주로 application server와 database이다. application server가 가지고 있는 파일을 업데이트 하여 보내준다.
    
    ---
    
    [https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_web_server)
    
### 일반 웹 서버와 WAS의 차이는?
    - 일반 웹 서버
        
        일반 웹 서버는 주로 동적인 컨텐츠를 전달하는 정적 웹 서버를 말할 때 쓰인다.
        
        Apache Server, Nginx, IIS(Windows 전용 Web 서버) 등이 있다.
        
    - WAS
        
        Web Application Server는 동적 웹 서버를 말할 때 쓰인다.
        
        “웹 컨테이너(Web Container)” 혹은 “서블릿 컨테이너(Servlet Container)”라고도 불린다.
        
        Tomcat, JBoss, Jeus, Web Sphere 등이 있다.
        
    
    일반적으로 둘이 협력하는데, 정적인 컨텐츠 요청은 일반 웹 서버가 처리하고 동적인 컨텐츠는 웹 서버가 application server로 건네서 처리한다.
    
    ---
    
    [https://www.nginx.com/resources/glossary/application-server-vs-web-server/](https://www.nginx.com/resources/glossary/application-server-vs-web-server/)
    
    [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
    
### 왜 웹 서버와 WAS를 같이 쓸까? WAS만 있으면 되지 않을까?
    - 기능을 분리하여 WAS의 부하를 방지할 수 있다.
        
        Web Server는 정적 컨텐츠만 처리 WAS는 동적 컨텐츠만 처리
        
    - 여러 대의 WAS를 사용하여 부하 분산을 할 수 있다.
        
        이 때 Web Server는 Load Balancing하는데 사용된다.
        
    - 장애 극복을 쉽게 해주고 무중단 운영이 가능하게 해준다.
        
        대용량 웹 어플리케이션에서 하나의 WAS가 죽으면 앞단에 Web Server가 이 WAS를 이용 못하게 하고 WAS를 재시작하는 방식으로 쉽게 장애 극복이 가능하다.
        
        덕분에 무중단 운영이 가능하다.
        
        ---
        
        [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
        
### 웹 서비스 아키텍쳐는 어떤 구조를 가질 수 있을까?
    
    다양한 구조를 가질 수 있다.
    
    1. Client -> Web Server -> DB
    
    2. Client -> WAS -> DB
    
    3. Client -> Web Server -> WAS -> DB
    
    3번 구조의 동작 과정
    
    1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
    2. Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
    3. WAS는 관련된 Servlet을 메모리에 올린다.
    4. WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다. (Thread Pool 이용)
    5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
        1. Thread는 Servlet의 service() 메서드를 호출한다.
        2. service() 메서드는 요청에 맞게 doGet() 또는 doPost() 메서드를 호출한다.
        protected doGet(HttpServletRequest request, HttpServletResponse response)
    6. doGet() 또는 doPost() 메서드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
    7. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
    8. 생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.
    
    ---
    
    [https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
    
### 웹 애플리케이션이란?
    
    웹 서버 위에서 동작하는 프로그램.
    
    ---
    
    [https://en.wikipedia.org/wiki/Web_application](https://en.wikipedia.org/wiki/Web_application)
    
### 한 컴퓨터에서 80번 포트로 웹 서버를 두 개 띄울 수 있을까?
    
    포트는 한 컴퓨터에 있는 프로세스를 구분할 수 있는 번호인데 다른 프로그램 두 개가 동시에 사용할 수는 없을 것 같다.

# Tomcat
### 톰캣이란?
    
    톰캣 9와 그 전 버전까지는 Java EE, 10과 그 이후부터는 Jarkarta EE의 open source implementation of the [Jakarta Servlet](https://projects.eclipse.org/projects/ee4j.servlet), [Jakarta Server Pages](https://projects.eclipse.org/projects/ee4j.jsp), [Jakarta Expression Language](https://projects.eclipse.org/projects/ee4j.el), [Jakarta WebSocket](https://projects.eclipse.org/projects/ee4j.websocket), [Jakarta Annotations](https://projects.eclipse.org/projects/ee4j.ca) and [Jakarta Authentication](https://projects.eclipse.org/projects/ee4j.authentication) specifications.
    
    - 톰캣이 서블릿이나 JSP의 구현이란게 무슨 말이지?
    
    WAS로써 HTTP 요청에 응답할 수 있다.
    
    Java code가 돌아갈 수 있는  자바 HTTP 웹 서버 환경을 제공해준다.
    
    JSP/ Servlet의 컨테이너로 요청에 해당하는 JSP나 Servlet을 생성할 수 있다.
    
    ---
    
    [https://tomcat.apache.org/](https://tomcat.apache.org/)
    
    [https://en.wikipedia.org/wiki/Apache_Tomcat](https://en.wikipedia.org/wiki/Apache_Tomcat)
    
### 톰캣은 왜 8080이 기본 포트일까?
    
    유저 권한은 1024번 포트 이하에는 바인딩 할 수 없고, 톰캣은 보안을 이유로 리눅스나 유닉스에서 유저 권한으로 가동되기 때문이다.
    
    때문에 톰캣이 해킹 당하더라도 유저 권한에 해당하는 부분까지만 접근 가능하다.
    
    반면에 아파치는 80 포트가 기본인데, 이를 위해 아파치는 관리자 권한으로 실행된다.
    
    대신에 아파치는 80번 포트로 요청을 받기만 하고 관련 처리는 유저 권한인 child process를 통해서 하는 듯하다.
    
    ---
    
    [https://brocess.tistory.com/158](https://brocess.tistory.com/158)
    
    [https://superuser.com/questions/316705/running-apache-as-a-different-user](https://superuser.com/questions/316705/running-apache-as-a-different-user)