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

### 요청이 스프링에 도착했을 때 일어나는 일련의 과정은? ★
    
    모든 경로의 요청은 DispatcherServlet으로 들어간다. DispatcherServlet의 Service()메소드가 호출되고 이 안에서 doDispatch()메소드가 호출된다.
    
    DoDispatch() 메소드 세부내용
    
    1. 핸들러 조회: 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
    2. 핸들러 어댑터 조회: 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
    3. 핸들러 어댑터 실행: 핸들러 어댑터를 실행한다.
    4. 핸들러 실행: 핸들러 어댑터가 실제 핸들러를 실행한다.
    5. ModelAndView 반환: 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서
    반환한다.
    6. viewResolver 호출: 뷰 리졸버를 찾고 실행한다.
    JSP의 경우: InternalResourceViewResolver 가 자동 등록되고, 사용된다.
    7. View 반환: 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
    JSP의 경우 InternalResourceView(JstlView) 를 반환하는데, 내부에 forward() 로직이 있다.
    8. 뷰 렌더링: 뷰를 통해서 뷰를 렌더링 한다
    
    ---
    
    김영한, 스프링 웹 MVC, 인프런,  스프링 MVC 전체 구조
    
### 스프링 MVC에서 확장 가능한 포인트는 어떤 부분이 있고 어떻게 작동할까?
    
    요청을 처리할 수 있는 Handler를 찾아내는 HandlerMapping
    
    Handler를 일관된 인터페이스로 만들어주는 HandlerAdapter
    
    뷰 이름을 통해 뷰를 만들어주는 ViewResolver

    Argument를 Resolve 해주는 ArgumentResolver

    Return Value를 Resolve해주는 ReturnValueHandler

    Http Message를 변환해주는 HttpMessageConverter
    
    등을 확장할 수 있다.
    
    이들은 이미 스프링이 여러 방식의 클래스를 등록해놓는다.
    
    예를 들어 HandlerMapping 같은 경우는
    
    1. RequestMappingHandlerMapping - 애노테이션 기반 컨트롤러의 사용
    2. BeanNameUrlHandlerMapping - 스프링 빈의 이름으로 핸들러를 찾음
    
    등이 등록되어 있는데 RequestMappingHandlerMapping을 통해 매칭되는 핸들러가 있는지 찾고 없으면 다음으로 BeanNameUrlHandlerMapping을 통해 매칭되는지 찾는다.
    
    다른 요소들도 이런 방식으로 등록되어 있는 것들을 순서대로 이용해서 찾는다.
    
    ---
    
    김영한, 스프링 웹 MVC, 인프런,  스프링 MVC - 구조 이해 파트

### HttpMessageConverter는 무엇이고 어떤 방식으로 동작할까?
    - HttpMessageConverter란?
        
        Argument Resolver가 HttpMessage의 body를 객체나 String으로 변환할 때 사용하는 클래스
        
    - 어떤 방식으로 동작할까?
        
        **간결한 설명**
        
        요청이 들어올 때는 핸들러가 호출되기 전에 핸들러의 붙은 Parameter의 타입과 요청의 Content-type을 고려해서 HttpMessageConverter가 선택되고 이 Converter가 변환해서 HandlerMethod에 인자로 넣어준다.
        
        요청이 나갈 때는 요청 header의 Accept와 컨트롤러의 반환 타입을 고려해서 HttpMessageConverter가 선택되고 이 Converter가 변환해서 요청을 내보낸다.
        
        **상세한 설명**
        
        - HTTP 요청 데이터 읽기
            - HTTP 요청이 오고, 컨트롤러에서 @RequestBody , HttpEntity 파라미터를 사용할 때
            - 메시지 컨버터가 메시지를 읽을 수 있는지 확인하기 위해 canRead() 를 호출한다.
                - 대상 클래스 타입을 지원하는가.
                예) @RequestBody 의 대상 클래스 ( byte[] , String , HelloData )
                - HTTP 요청의 Content-Type 미디어 타입을 지원하는가.
                예) text/plain , application/json , */ *
            - canRead() 조건을 만족하면 read() 를 호출해서 객체 생성하고, 반환한다.
        - HTTP 응답 데이터 생성
            - 컨트롤러에서 @ResponseBody , HttpEntity 로 값이 반환될 때
            - 메시지 컨버터가 메시지를 쓸 수 있는지 확인하기 위해 canWrite() 를 호출한다.
                - 대상 클래스 타입을 지원하는가.
                    - 예) return의 대상 클래스 ( byte[] , String , HelloData )
                - HTTP 요청의 Accept 미디어 타입을 지원하는가.(더 정확히는 @RequestMapping 의 produces )
                    - 예) text/plain , application/json , **/ **
            - canWrite() 조건을 만족하면 write() 를 호출해서 HTTP 응답 메시지 바디에 데이터를 생성한다.
        
        ---
        
        김영한, 스프링 MVC 1편, 인프런, HTTP 메시지 컨버터 파트
        
### 어떻게 핸들러 메소드들은 여러가지 파라미터를 사용해서 원하는 값을 변환해서 받고 여러가지 리턴 값을 변환해서 내보낼 수 있을까?
    
![Untitled](https://user-images.githubusercontent.com/71138398/176466300-7769e77b-d673-4aa5-9f81-515124c5088f.png)

    
    **요청시**
    
    핸들러 메소드로 전달되는 값들은 핸들러 메소드가 호출되기 전에 Argument Resolver를 통해서 핸들러 메소드의 파라미터 타입과 어노테이션 등에 따라 변환된다.
    
    이 때 만약 파라미터 가 @RequestBody를 어노테이션으로 갖고 있거나 HttpEntity를 타입으로 갖고 있다면 HttpMessageConverter를 이용해서 변환할 것이다.
    
    **응답시**
    
    리턴되는 값은 ReturnValueHandler에 의해서 ReturnType에 따라 변환된다.
    
    이 때 만약 @ResponseBody라는 어노테이션이 핸들러에 붙어 있거나 HttpEntity를 리턴한다면 HttpMessageConverter를 이용해서 변환할 것이다.
    
    ---
    
    김영한, 스프링 MVC 1편, 인프런, 요청 매핑 헨들러 어뎁터 구조 파트
    
### RedirectAttributes란 무엇이고 왜 쓸까?
    
    리다이렉트를 할 때 사용되는 값들을 key value 형태로 저장하는 자료구조다.
    
    "redirect:/basic/items/" + item.getId() 이렇게 사용하면 URL 인코딩이 안되서 위험하다.
    
    다음과 같이 사용할 수 있다.
    
    @PostMapping("/add")
    public String addItemV6(Item item, RedirectAttributes redirectAttributes) {
     Item savedItem = itemRepository.save(item);
     redirectAttributes.addAttribute("itemId", savedItem.getId());
     redirectAttributes.addAttribute("status", true);
     return "redirect:/basic/items/{itemId}"; // "redirect:/basic/items/" + item.getId() 이렇게 사용하면 URL 인코딩이 안되서 위험하다.
    }
    
    //itemId는 url에 사용되었고 사용되지 않은 status는 쿼리파라미터의 형태로 붙는다.
    // ex.  http://localhost:8080/basic/items/5/?status=true


### 세션을 만들고 쿠키를 HttpServletResponse에 포함하지 않았는데 어떻게 쿠키가 자동으로 내려갈까?
    
    서블릿을 통해 세션이 생성되면 톰캣이 자동으로 ‘JSESSIONID’라는 이름의 쿠키에 세션아이디를 담고 응답 헤더에 같이 보낸다.
    
    ---
    
    인프런, 김영한, 스프링 MVC 2편, 로그인 처리하기 -  서블릿 HTTP 세션 1
    
### 스프링에서 쿠키와 세션은 어떻게 사용하고 어떤 식으로 동작할까?
    
    cookie는 그냥 new Cookie()로 생성해서 response 객체에 응답 헤더에 추가할 수 있다.
    
    세션은 HttpServletRequest로 부터 getSession()을 통해 얻을 수 있고 controller에 파라미터로 @SessionAttribute를 통해 세션 안에 값을 바로 얻어올 수도 있다.
    
### 사용자의 브라우저가 닫아진 것을 어떻게 알고 세션을 만료해야할까?
    
    기본적으로 서버는 stateless이기 때문에 브라우저가 닫아졌는지 안 닫아졌는지 확인할 수 있는 길이 없다. 따라서 서버에서는 세션의 만료시간을 잡고 일정 만료시간이 지나면 삭제하게끔 한다. 
    
    ---
    
    인프런, 김영한, 스프링 MVC 2편, 세션 정보와 타임아웃 설정
    
### 세션의 만료 시간과 보안은 어떤 관련이 있을까?
    
    세션 만료 시간이 길수록 해커가 쿠키를 탈취하여 로그인할 수 있는 시간이 길어지기 때문에 세션 만료 시간과 보안 강도는 반비례한다. 따라서 세션의 만료 시간을 짧게 잡는 것이 중요하다.
    
    하지만 세션 만료 시간을 짧게 잡는 경우 사용자는 서비스를 이용하다가 로그인이 풀릴 수가 있다. 따라서 세션은 ‘마지막 접근 시간으로부터 몇 분간 유효’와 같은 방식으로 구현하는 것이 좋다. 기본적으로 서블릿에서 제공하는 HttpSession은 이 방식으로 동작한다.
    
    ---
    
    인프런, 김영한, 스프링 MVC 2편, 세션 정보와 타임아웃 설정