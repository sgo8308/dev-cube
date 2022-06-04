### 패키지 구조는 Layer 우선 방식과 모듈 우선 방식 중 어떤 방식을 사용할까?
    - Layer 우선 방식
        - 정의
            
            Service, Domain, Repository와 같은 계층(Layer)에 따른 분류를 먼저한다. 
            
            예를 들면 다음과 같은 방식이다.
            
            - com.jiwoo.domain.modulename
            - com.jiwoo.service.modulename
            - com.jiwoo.repository.modulename
        - 장점
            - ?
        - 단점
            - MSA와 같이 모듈 단위로 분리할 때 어려움이 있다.
            - 하나의 기능에 포함되는 클래스들이 여러 폴더에 분산되어 코드를 탐색하는데 시간이 많이 들어 생산성이 떨어진다.
    - 모듈 우선 방식
        - 정의
            
            order, member와 같이 모듈에 따른 분류를 먼저 한다.
            
            예를 들면 다음과 같은 방식이다.
            
            - com.jiwoo.order.domain
            - com.jiwoo.order.service
            - com.jiwoo.member.domain
            - com.jiwoo.member.service
        - 장점
            - MSA와 같이 모듈 단위로 분리할 때 유리하다.
            - 변경의 주기가 같은 것들이 묶여서 변경이 일어났을 때, 이 폴더 저 폴더로 옮겨 다니면서 작업할 필요가 없어 생산성을 올려준다.
        - 단점
            ?

    장점이 많은 모듈 우선 방식을 사용하는 것이 좋을 것 같다.
    
    대용량 서비스는 MSA로 나아갈 확률이 크고 그 때 모듈 우선 방식이 훨씬 MSA를 적용하기 쉽기 때문이다. 
    
    ---
    
    [http://www.javapractices.com/topic/TopicAction.do?Id=205](http://www.javapractices.com/topic/TopicAction.do?Id=205)