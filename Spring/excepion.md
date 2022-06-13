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