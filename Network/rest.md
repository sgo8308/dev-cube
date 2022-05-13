### REST의 의미

    Representational State Transfer.
  
    여기서 Representational(표현)이란 어떤 자원을 어떻게 표현할 것인지이다.
    http헤더의 content-type이 이 표현의 포맷이다.
  
    State Transfer은 ‘상태 전이’라는 뜻인데 어떤 한 웹페이지를 상태라고 하고 그 웹페이지에서 다른 웹페이지로 링크를 타고 이동하는 행위를 ‘상태 전이’라고 한다.
  
    또한 자원은 실제 문서나 이미지 등을 직접 가르키는 것이 아니라 URI 그 자체를 의미한다.
  
    예를 들면 members/100이라는 URI가 있을 때 자원은 ‘members/100’이라는 URI 자체인 것이다. 이 URI에 대응되는 것은 무엇이든 될 수 있다.
      
    ---

    아직 정확하게 의미가 와닿지 않는다. 좀 더 경험을 쌓고 이해해보자.

### REST의 개념

    분산 하이퍼미디어 시스템(ex 웹)을 위한 제약조건의 집합

### RESTful이란?

    REST의 제약조건을 모두 만족해야 RESTful하다고 부른다.
  
    그러나 RESTful API REST 제약 조건을 완벽하게 만족하기가 상당히 힘들기도 하고, 지금껏 그렇게 불러왔기 때문에 관례상 실무에서는 완벽하게 만족하지 않더라도 RESTful API라고 부른다.

### REST이란 개념은 왜 나왔을까?

    웹이 폭발적으로 성장하고 있었고 아직 웹에 대한 명확한 표준이라는 것이 없던 1994년에 웹 프로토콜 표준에 대한 가이드 프레임워크로써 웹이 어떻게 동작해야하는지에 대한 구조적인 모델을 만들기 위해 나오게 되었다.
  
    로이 필딩은 Rest가 [본인 블로그](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)의 8번째 답글에서 REST의 모든 세부사항이 소프트웨어의 지속성과 독립적인 진화를 위해서 의도되었다고 한다.

### RESTful api란? ★

    Rest의 제약 조건을 모두 지킨 API를 Rest API라고 한다.

    하지만 실제로는 HTTP프로토콜을 사용하는 API를 Rest API라고 부른다.

    그리고 REST와 별로 상관은 없지만 URI로 자원을 표현하고 자원에 대한 행위는 HTTP Method로 표현하는 등의 규칙을 지켜 설계하면 Restful하게 API를 설계했다고 한다.

    하지만 정확히는 Rest 제약 조건을 모두 지켜야 Rest API이기 때문에 위와 같은 API는 HTTP API로 부르는 것이 낫다.
      
    ---

    [https://www.inflearn.com/questions/126743](https://www.inflearn.com/questions/126743)

    [https://velog.io/@lehdqlsl/Spring-boot-HTTP-API-만들기-Hello-World](https://velog.io/@lehdqlsl/Spring-boot-HTTP-API-%EB%A7%8C%EB%93%A4%EA%B8%B0-Hello-World)

    [https://www.infoq.com/news/2016/07/microsoft-rest-api/](https://www.infoq.com/news/2016/07/microsoft-rest-api/)

### RESTful API의 탄생 배경

    REST라는 것과 RESTful API는 같은 시대에 나온 개념이 아니다.
  
    REST라는 것은 이미 HTTP를 잘 설계하기 위한 고민에서 나온 아키텍쳐 스타일이다.
  
    RESTful API는 원격의 서버의 기능을 어떻게 하면 쉽고 간단하게 사용할 수 있을까 고민하는 과정에서 나온 API 스타일이다.
  
    번거로운 과정 없이 마치 HTTP로 html 문서를 주고 받듯이 API를 설계하는 것이다.
  
    HTTP 프로토콜을 최대한 그대로 사용하기도 하고 HTTP가 이미 REST 제약조건에 따라 잘 설계되어 있기 때문에 이런 방식의 API를 REST API라고 부르는 듯하다. HTTP API라고 불러도 상관은 없다.
  
    HTTP API에서 REST 제약을 추가한 것이 REST API라고 하기도 하는데 HTTP는 이미 REST에 바탕 위에서 만들어져 있다. 이를 테면 Stateless나 Cacheable과 같은 것이다.
    좀 더 고민해봐야겠다.

### RESTful하게 api를 설계해야 하는 이유

    레스트풀하게 함으로써 서버와 브라우저가 독립적으로 진화하고 각자가 어떻게 업데이트하든 상관없이 항상 옳바로 동작할 수 있음. 서버 업데이트로 인해 앱이 자주 업데이트 해야하는 앱생태계는 앱과 서버가 레스트풀하게 통신하고 있지 않다고 할 수 있음

    하지만 이것은 엄격한 Rest API를 설계할 경우이고 일반적으로 우리가 말하는 Rest API를 설계하는 이유는 더 명확하고 이해하기 쉬운 API를 만들기 위함이다.
    
    ---

    [https://dzone.com/articles/7-rules-for-rest-api-uri-design-1](https://dzone.com/articles/7-rules-for-rest-api-uri-design-1)

### 단점

    완벽하게 구현하기 어렵다. 웹페이지는 잘 구현됐으나, api는 구현이 어렵다