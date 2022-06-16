### 스프링 부트는 예외 또는 response.sendError() 발생시 일관된 예외 화면을 어떻게 보여줄까?
    
    - ErrorPage 자동 등록, Controller 미리 생성
    
    스프링 부트를 사용하지 않을 때는 직접 톰캣을 상태 코드에 따라 ErrorPage를 요청하도록 등록하고 이 ErrorPage 요청을 처리할 Controller를 따로 만들어주어야 한다.
    
    그러나 스프링 부트에서는 자동으로 ErrorPage를 등록해주고 이 ErrorPage를 처리해 줄 컨트롤러까지 BasicErrorController라는 것을 자동으로 등록해준다.
    
    이제 오류가 발생하면 오류페이지로 /error를 기본으로 요청한다. 또한 위 컨트롤러는 기본적으로 resources/error/로 오는 Request를 모두 받도록 되어 있으며 상태코드에 걸맞는 html을 찾아서 반환해준다.
    
    따라서 개발자는 원하는 원하는 에러 html을 reources/error/하위에 404.html 이런 식으로 만들어 주면 된다. 이 때 4xx.html 이렇게 만들면 400번대 오류코드는 모두 이 html을 반환한다.
    
    - 에러에 관한 상태 model에 담아 뷰에 전달
    
    BasicErrorController는 model에 다음과 같은 정보를 담아 뷰에 전달한다.
    
    예시
    * timestamp: Fri Feb 05 00:00:00 KST 2021
    * status: 400
    * error: Bad Request
    * exception: org.springframework.validation.BindException
    * trace: 예외 trace
    * message: Validation failed for object='data'. Error count: 1
    * errors: Errors(BindingResult)
    * path: 클라이언트 요청 경로 (`/hello`)
    
    하지만 고객이 몰라도 될 정보(exception, message, stacktrace, binding error) 등등은 기본적으로 포함하지 않지만 설정을 통해 바꿀 수 있다. 하지만 그대로 두는 것이 낫다.
    
    ---
    
    김영한, 스프링 MVC 2편 , 인프런, 스프링 부트 - 오류 페이지1

### API 예외처리와 html 오류 페이지가 다른 점은 무엇일까?
    
    html 오류 페이지는 거의 일관되게 나갈 수 있기 때문에 BasicErrorController를 이용해서 커버가 가능하다. 단순히 5xx, 4xx 관련된 오류 화면을 보여주면 된다.
    
    그러나 API 같은 각각의 컨트롤러나 예외마다 서로 다른 응답 결과를 출력해야 할 수도 있다. 예를 들어 같은 예외라도 회원과 관련된 API 발생할 때와, 상품과 관련된 API에서 발생할 때 그 결과가 달라야 될 수 있다. 또 API는 앱이 요청할 수도 있고 다른 서버가 요청할 수도 있으며 그 상대방에 따라 API 규약 또한 다르다.
    
    따라서 API 예외처리는 더 세밀한 처리가 필요하다.
    
    따라서 BasicErrorController는 html 예외 페이지에서는 정말 편하고 좋지만 API 예외처리에는 한계가 있다.(기본적인 Json형태로 오류 내용에 대해서 반환은 가능하지만)
    
    ---
    
    김영한, 스프링 MVC 2편, 인프런, 스프링 부트 기본 오류 처리
    
### 스프링 부트가 기본으로 제공하는 ExceptionResolver는 뭐가 있을까?
    1. ExceptionHandlerExceptionResolver
    2. ResponseStatusExceptionResolver
    3. DefalutHandlerExceptionResolver
    
    번호가 커질수록 우선 순위가 떨어진다.


## ExceptionResolver

### 공부해야 하는 이유는?
    
    @ExceptionHandler를 공부하기 전까지 이해를 돕는 과정
    
### 정의는?
    
    컨트롤러에서 예외가 발생했을 때 이 예외를 WAS로 바로 내보내지 않고 처리(Resolve)해서 보내주는 것.
    
### 사용해야 하는 이유는?
    
    예외가 발생할 경우 WAS는 모든 예외를 상태 코드 500으로 내보낸다.
    
    이 때 예외에 따라서 상태 코드를 변경하려면 예외를 중간에서 잡아서 먹은 후 response.sendError()를 통해 원하는 상태 코드를 넣어줄 수 있다.
    
    혹은 WAS까지 예외를 전파하지 않고 아예 예외에 따라 원하는 뷰를 등록하거나 Json 응답을 직접 넣어 전달해줄 수도 있다.
    
    즉 서블릿 컨테이너까지 예외가 올라갈 때 발생하는 복잡하고 지저분한 추가 프로세스(다시 error페이지를 요청하는) 과정 없이 깔끔하게 예외가 처리된다.
    
### 어떤 방식으로 동작할까?
    
    DispatcherServlet에서 컨트롤러(핸들러)가 처리를 다 한 후 예외가 발생되었는지 살펴서 예외가 발생되었다면
    등록되어 있는 ExceptionReslover들을 순환하면서 resolveException()을 호출한다.
    이 때 resolveException은 ModelAndView를 반환하는데, 이 값이 null이라면 다음 ExceptionResolver의 resolverException()을 호출하고
    null이 아니라면 이 ModelAndView를 갖고 정상적으로 다음으로 진행한다.
    
    ---
    
    DispatcherServlet.doDispatch(), DispatcherServlet.processHandlerException() 
    
### 단점은?
    
    서블릿 컨테이너까지 예외를 올리지 않고 깔끔하게 처리한다는 점에서는 좋지만 이것을 구현하는 것이 상당히 복잡하다. 
    이를테면 다음과 같은 코드가 필요하다.
    
    @Override
     public ModelAndView resolveException(HttpServletRequest request,
    		HttpServletResponse response, Object handler, Exception ex) {
    	 try {
    		 if (ex instanceof UserException) {
    			 String acceptHeader = request.getHeader("accept");
    			 response.setStatus(HttpServletResponse.SC_BAD_REQUEST);
    
    			 if ("application/json".equals(acceptHeader)) {
    				 Map<String, Object> errorResult = new HashMap<>();
    				 errorResult.put("ex", ex.getClass());
    				 errorResult.put("message", ex.getMessage());
    				 String result = objectMapper.writeValueAsString(errorResult);
    				 
    				 response.setContentType("application/json");
    				 response.setCharacterEncoding("utf-8");
    				 response.getWriter().write(result);
    				 return new ModelAndView();
    			 } else {
    				 //TEXT/HTML
    			 return new ModelAndView("error/500");
    			 }
    		 }
    	 } catch (IOException e) {
    		 log.error("resolver ex", e);
    	 }
    	 return null;
     }
    
    API 입장에서는 View가 필요없는데 무조건 ModelAndView를 반환해야 하는 점이 번거롭다.
    

### ADVANCED

### 잘 사용하려면 어떻게 해야할까?
    
    Configuration 파일에 등록할 때 extendHandlerExceptionResolvers()를 사용하도록 한다. 
    configuredHandlerExceptionResolvers()를 사용할 수도 있는데 그렇게 되면 스프링이 기본으로 등록하는 ExceptionResolver가 제거된다.

---

김영한, 스프링 MVC 2편, 인프런, HandlerExceptionResolver 파트


## ExceptionHandlerExceptionResolver

### BASIC

### 공부해야 하는 이유는?
    
    ExceptionHandler를 사용하여 일관된 예외처리를 구현하기 위해서
    
### ExceptionHandlerExceptionResolver란?
    
    스프링 부트가 기본으로 제공하는 ExceptionResolver 중 가장 우선순위가 높은 Resolver.
    
### 사용해야 하는 이유는?
    
    BasicErrorController로 API의 예외까지 처리하기에는 세밀함이 떨어지기에 부족함이 많다.
    
    또한 기본적으로 Was → 컨트롤러 → WAS → error controller → WAS로 이어지는 불필요한 과정이 있다. ExceptionResolver를 사용하면 이를 해결할 수 있지만 구현하기가 복잡하고 번거롭다.
    
    ExceptionHandlerExceptionResolver는 깔끔하게 이 문제를 해결할 수 있다.
    
    또한 @ControllerAdvice를 같이 사용하면 예외 처리 코드들을 한 곳으로 모을 수 있따.
    
### 어떤 방식으로 동작할까?
    
    A라는 컨트롤러에서 B라는 예외가 발생하면 ExceptionHandlerExceptionResolver는 A라는 컨트롤러 안에 존재하는 B라는 예외를 처리하는 @ExceptionHandler가 붙은 메서드를 호출한다.
    
    개발자는 일반적인 Rest API 컨트롤러 메서드를 만들듯이 이 메서드를 만들어 에러를 처리하면 된다. (에러 정보를 담는 객체를 만들어서 리턴한다던지)
    
### 단점은?
    
    boilerplate 코드가 많을 수 있다?
    
    ---
    
    [https://auth0.com/blog/get-started-with-custom-error-handling-in-spring-boot-java/#:~:text=The first one involves boilerplate code%2C but it is clean and best-practice based](https://auth0.com/blog/get-started-with-custom-error-handling-in-spring-boot-java/#:~:text=The%20first%20one%20involves%20boilerplate%20code%2C%20but%20it%20is%20clean%20and%20best%2Dpractice%20based).
    

### ADVANCED

### 잘 사용하려면 어떻게 해야할까?
    
    컨트롤러 내부에서 사용하면 비즈니스 로직과 예외 처리 로직이 한 곳에 존재하게 되고, 여러 컨트롤러에서 비슷한 ExceptionHandler가 중복될 수 있다.
    
    이 때 @ControllerAdvice가 붙은 클래스를 만들고 이 안에 ExceptionHandler를 넣어주면 된다. @ControllerAdvice에는 범위를 지정해줄 수 있다.
    
    ExceptionHandlerExceptionResolver는 컨트롤러에서 예외가 발생하면 알맞은 @ControllerAdvice를 찾아서 그 안에 @ExceptionHandler가 붙은 메서드를 호출한다.
    
    RestControllerAdvice는 ControllerAdvice에 @ResponseBody가 붙은 것이다.


### 필터나 인터셉터에서 예외가 터지면 ExceptionHandler가 처리할까?
    
    필터는 DispatcherServlet으로 들어오기 전에 작동한다.
    
    ExceptionHandler는 DispatcherServlet이 호출시켜주는 ExceptionResolver가 처리하기 때문에 필터의 예외는 처리하지 못한다.
    
    인터셉터의 preHandle()과 postHandle()은 ExceptionHandler가 잡아줄 수 있다. 하지만 afterCompletion()은 잡아줄 수 없다.
    
    잡히지 않은 예외는 WAS로 갔다가 /error로 요청을 할 것이고 이는 BasicErrorController가 처리를 할 것이다.
    
    ---
    
    DispatcherServlet.doDispatch(), DispatcherServlet.processDispatchResult() 코드

---

김영한, 스프링 MVC 2편, 인프런, ExceptionHandler 파트


### ResponseStatusExceptionResolver란?
    
    @ResponseStatus(value = HttpStatus.Not_Found)와 같은 애너테이션이 붙어 있는 예외 클래스나 스프링이 기본으로 만들어 놓은 ResponseStatusException이라는 클래스를 잡는다.
    
    그 후 동봉되어 있는 상태코드와 메시지를 response.sendError()를 통해 붙여준다.
    
    그 후 빈 ModelAndView를 반환한다.
    
    직접 ExceptionResolver를 구현하고 예외를 먹은 후 상태코드에 맞게 바꺼주는 그런 작업을 이미 만들어 놓아 개발자가 편리하게 이용할 수 있게 해준다.
    
### DefalutHandlerExceptionResolver란?
    
    스프링 내부에서 발생하는 스프링 예외를 해결한다.
    
    원래 모든 예외는 상태 코드 500 에러를 내보내지만 이 것이 각각의 스프링 예외에 맞는 적절한 상태코드를 가진 에러로 변환시킨다.
    
    예를 들어 TypeMismatchException같은 경우 클라이언트 쪽에서 데이터를 잘못 입력했기 때문에 발생하는 예외다. 이런 예외는 상태 코드 400과 Bad Resquest를 내보내는게 맞는 방식이다.