### 왜 HTTP를 공부해야 할까?
    
    월드 와이드 웹을 지탱하는 가장 중요한 기술 두 가지는 HTML과 HTTP이다.
    
    따라서 HTTP를 이해한다는 것은 웹이 어떻게 동작하는지 이해한다는 것이며, 이를 깊이 이해하면 웹 프로그래밍을 하면서, 웹 서버를 조작하면서, 그리고 네트워크를 관리하면서 정확한 근거에 기반한 올바른 기술적 판단을 내려야 할 때 큰 도움이 된다.
    
    HTTP는 지난 30년간 사용되어 왔으며 앞으로도 계속 사용될 것이다.
    
    ---
    
    데이빗 고울리 외 4명, HTTP 완벽 가이드, 이응준 외 1명, 도서출판인사이트, 초판 6쇄, 옮긴이 서문,2021
    
### stateless한 특성이란?
    
    초기 HTTP 프로토콜을 사용하는 서버는 클라이언트의 상태를 저장할 방법이 없었기때문에 HTTP 프로토콜은 stateless하다고 불렸다.
    
    하지만 이후 stateful한 기능을 위해 쿠키와 같은 여러가지 기술이 추가되었다.
    
    따라서 마음만 먹으면 stateful하게 서버를 설계할 수 있다.
    
    하지만 그럼에도 불구하고 서버를 최대한 stateless하게 설계하는 것이 중요한데, 그 이유는 서버의 scale out에 유리하기 때문이다.
    
### connectionless한 특성이란?
    
    HTTP는 기본적으로 연결을 유지하지 않는 모델이기 떄문에 connectionless하다고 한다. 
    
    HTTP가 처음 나왔을 때는 단순 텍스트를 주고받는 형태였기 때문에 클라이언트에게 요청을 받고 응답을 준 후 더 이상 연결을 유지할 필요가 없었다.
    
    또한 connection을 유지하는데는 많은 자원이 소모되기 때문에 connectionless하게 설계되었다.
    
    이를 테면 하나의 connection에는 하나의 소켓이 필요한데 소켓이 생성될 때마다 소켓으로부터의 입력과 출력을 도와주는 입출력 버퍼가 같이 생성된다. 이 버퍼의 크기를 약 128KB바이트(일반적인 리눅스 디폴트 값)라 가정했을 때 10만명의 클라이언트와 동시 접속을 유지하게 될 경우 약 10Gb의 메모리를 차지한다.
    
    그러나 웹 페이지가 단순 텍스트에서 멀티미디어로 변함에 따라 이런 connectionless한 특성이 문제가 되었고, HTTP/1.1에서는 지속 연결이라 하여 클라이언트가 필요한 자원을 다 받을 때까지 TCP 연결이 유지되도록 만들어졌다.
    
    ---
    
    Charles M. Kozierok, TCP/IP 완벽 가이드, 강유 외 3명, 에이콘출판주식회사, 1336p,2007
    
    김영한, 모든 개발자를 위한 HTTP 웹 기본 지식, 인프런
    
    윤성우, 윤성우의 열혈 TCP/IP 소켓 프로그래밍, 오렌지미디어, 6쇄, 134-135, 2020
    
    [https://meetup.toast.com/posts/53](https://meetup.toast.com/posts/53)


### Request, Response에는 각각 어떤 정보가 포함될까요?
    
    둘 다 시작 라인, 헤더, 공백 라인, 메시지 본문으로 구성이 됩니다.
    
    이 때 본문은 있어도 되고 없어도 되며 나머지 부분은 반드시 있어야 합니다.
    
    - 시작 라인은 이것이 어떤 메시지인지 서술하며, 헤더는 속성을, 메시지 본문은 데이터를 담고 있습니다.
        - 시작 라인
            
            구체적으로 시작 라인은 request line과 status line으로 구분되며 request line은 요청 메시지에 status line은 응답 메시지에서 사용됩니다.
            
            request line은 HTTP 메소드, 요청대상인 PATH, HTTP 버전순으로 구성이 되고,
            status line은 HTTP 버전, 상태 코드, 이유 문구순으로 구성이 됩니다.
            
        - 헤더
            
            헤더 같은 경우는 메시지 전체에 적용되는 정보인 General 헤더, 요청 정보인Request 헤더, 응답 정보인 Response 헤더, Representation(표현)에 대한 정보인 Representation 헤더로 나누어볼 수 있습니다. 이 때 Representation은 실제 전송되는 데이터를 의미합니다.
            
            - 왜 전송하는 데이터를 ‘표현’이라고 할까?
                
                회원이라는 리소스를 전송한다고 할 때 우리는 이 리소스를 html로 ‘표현’할 수도 있고 json으로 ‘표현’할 수도 있다. 그렇기 때문에 ‘표현’이라는 용어를 사용한다.
                
        - 본문
            
            본문 같은 경우는 실제 전송할 데이터가 담겨 있으며 HTML, 이미지, 영상, JSON 등 byte로 표현할 수 있는 모든 데이터가 전송 가능합니다.
            
    
    ---
    
    김영한, 모든 개발자를 위한 HTTP 웹 기본 지식, 인프런
    
### content negotiation에 대해서 설명해주세요 
    
    콘텐트 협상이라는 뜻으로 클라이언트가 선호하는 표현을 요청하는 것을 의미합니다. 
    
    요청 메시지 헤더에 있는 Accept, Accept-Charset, Accept-Encoding, Accept-Language와 같은 필드를 통해 클라이언트가 선호하는 미디어 타입, 문자 인코딩, 압축 인코딩, 자연 언어 등을 명시하면 서버는 최대한 클라이언트의 요구에 맞춰서 데이터를 전송해주는 식으로 협상이 이루어집니다. 
    
    ---
    
    김영한, 모든 개발자를 위한 HTTP 웹 기본 지식, 인프런

### Rest API란 무엇인가요? ★
    
    Rest의 제약 조건을 모두 지킨 API를 Rest API라고 한다.
    
    하지만 실제로는 HTTP프로토콜을 사용하는 API를 Rest API라고 부른다.
    
    그리고 REST와 별로 상관은 없지만 Microsoft의 Rest API 가이드라인을 따라 URI로 자원을 표현하고 자원에 대한 행위는 HTTP Method로 표현하는 정도로 설계하면 Restful하게 API를 설계했다고 한다.
    
    하지만 정확히는 Rest 제약 조건을 모두 지켜야 Rest API이기 때문에 위와 같은 API는 HTTP API로 부르는 것이 낫다.
    
    ---
    
    [https://www.inflearn.com/questions/126743](https://www.inflearn.com/questions/126743)
    
    [https://velog.io/@lehdqlsl/Spring-boot-HTTP-API-만들기-Hello-World](https://velog.io/@lehdqlsl/Spring-boot-HTTP-API-%EB%A7%8C%EB%93%A4%EA%B8%B0-Hello-World)
    
    [https://www.infoq.com/news/2016/07/microsoft-rest-api/](https://www.infoq.com/news/2016/07/microsoft-rest-api/)

### 멱등성이란?
    
    HTTP 메서드의 특성 중 하나다. 
    
    나만 요청하는 경우에 한해서 한 번 요청하든 여러번 요청하든 첫 요청시와 서버의 상태가 같을 때 이 요청에 사용된 http 메소드는 멱등하다고 한다.
    
    예를 들어 get을 요러번 요청했을 때 서버의 상태는 처음 요청과 다를바 없다.
    
    delete로 a라는 파일을 없앴다고 하면 이 delete를 여러번 했을 때 서버에 a라는 파일이 없다는 상태는 처음과 동일하다.
    
    반면 post로 결제를 여러번 했을 때는 서버의 상태는 여러번 결제된 상태가 된다. 첫 post로 결제했을 때와 두번째 post로 결제했을 때 서버의 상태가 다르다. 
    
    멱등이 가능해야 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가에 대한 판단 근거가 되기 때문에 중요하다. 
    그래서 POST같은 경우는 함부로 재요청을 할 수 없다.