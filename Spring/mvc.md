### Web Reactive와 Web Servlet의 차이를 블록킹, 논블록킹, 동기화, 비동기화의 관점에서 설명해주세요.
    
    Web Reactive는 Spring에서 web 애플리케이션을 만들기 위해 지원하는 기술 중 Reactive API를 사용하는 Webflux를 가르킨다.
    
    Webflux는 non bolcking io를 지원하여 적은 쓰레드와 적은 하드웨어 자원으로 동시성을 처리한다.
    
    blocking synchronyous 방식을 사용하는 Web Servlet - Spring MVC에서 쓰레드를 보통 코어보다 많게 만드는 이유는
    쓰레드가 I/O를 만나면 block되고 스위칭될 다른 쓰레드가 없다면 I/O 작업이 끝날 때까지 이 쓰레드를 담당하는 코어가 놀기 때문이다.
    
    하지만 non blocking 방식은 block될 일이 없어 모든 쓰레드가 항상 코어를 사용한다.
    즉 코어가 놀 일이 없어져 코어 갯수정도의 쓰레드만으로 CPU자원을 최대한 사용할 수 있다.


### 스프링 MVC 패턴과 Service, Domain, Repository는 무슨 관계인 걸까?
    - Model
        
        여러가지 의미로 사용된다. 
        
        스프링에서 Model은 단순히 Controller에서 View 데이터를 전달할 때 데이터를 담는 껍데기에 역할을 하며 다른 곳에서는 dao, dto, service를 모두 합쳐 Model이라고 하기도 한다.
        
    - View
        
        UI를 그리는 역할을 담당한다. (ex. html)
        
    - Controller
        
        외부의 요청과 응답을 처리한다.
        
    - Service
        
        Controller와 Repository의 중간 영역이다.
        
        Repository와 Domain을 사용하여 하나의 기능을 담당한다.(ex 주문취소)
        
        트랜잭션의 출발점이 된다(@Transactional이 사용됨)
        
    - Domain
        
        비즈니스적으로 의미있는 대상들이 도메인이라고 할 수 있다. 
        
        택시앱이라고 한다면 배차, 탑승, 요금 이런 것들이 도메인이다.
        
        @Entitiy가 붙은 것들이 도메인이다.
        
        VO와 같은 값 객체도 도메인이다.
        
        도메인은 자신과 관련된 비즈니스 로직을 처리한다.
        
    - Repository
        
        dao라고도 할 수 있으며 데이터 저장소에 접근하는 영역이다.
        
    
    ---
    
    이동욱, 스프링 부트와 AWS로 혼자 구현하는 웹 서비스, 프리렉, 초판 5쇄, 101-102, 2021
    
### 기본 MVC 패턴의 한계는 무엇이고 스프링 MVC 패턴은 어떻게 해결했을까?
    
    간결한 설명
    
    많은 코드가 중복되는 문제가 있어서 MVC 패턴은 프론트 컨트롤러 패턴을 이용해서 해결했다.
    
    상세한 설명
    
    - 한계
        
        기본 MVC 패턴은 컨트롤러로 서블릿을 사용하고 뷰로 Jsp 등을 사용한다.
        
        뷰는 깔끔하고 문제가 없지만 컨트롤러의 경우 코드가 중복되는 문제가 있다.
        
        ex dispatcher.forward(request, response);
        
        ex String viesPath = “/WEB-INF/views/new-form.jsp”
        
        이외에도 컨트롤러마다 공통적으로 적용하는 코드가 많아질수록 중복이 더 늘어날 것이다.
        
    - 해결
        
        컨트롤러 호출 전에 공통 기능을 처리해주는 프론트 컨트롤러 패턴을 도입하면 문제를 해결할 수 있다. 스프링 MVC에서는 이것을 DispatcherServlet이라는 것이 담당한다.
        
    
    ---
    
    김영한, 스프링 MVC 1편, 인프런, MVC 패턴 - 한계