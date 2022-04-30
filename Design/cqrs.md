### 공부해야 하는 이유는?
    
    상황에 따라 CQRS를 적용했을 때 좋은 상황이 있다면 적용할 수 있기 위해서

### 정의는?
    
    Command Query Responsibility Segregation의 약자로 명령과 조회의 책임을 분리하는 것을 의미한다.
    
    명령은 시스템의 상태를 변경하는 작업을 의미하며 조회는 시스템의 상태를 반환하는 작업을 의미한다.
    
    CQS 원리에 기반하며 CQS가 명령과 조회를 메소드 수준에서 분리하는 반면 CQRS는 개체(Object)나 시스템(혹은 하위 시스템) 수준에서 분리한다.
    
    예를 들면 우리는 데이터베이스와 상호작용할 때 CRUD 모델을 바탕으로 한다.
    
    ---
    
    [https://justhackem.wordpress.com/2016/09/17/what-is-cqrs/](https://justhackem.wordpress.com/2016/09/17/what-is-cqrs/)
    
    - CQS란?
        
        Command and Query Separation의 약자로 메소드는 어떤 상태를 바꾸는 작업을 하거나(명령) 값을 얻거나(조회)하는 둘 중의 하나만을 해야한다는 원칙이다.
        
        ---
        
        [https://en.wikipedia.org/wiki/Command–query_separation](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation)