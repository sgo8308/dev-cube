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

### 사용해야 하는 이유는?
    
    예를 들어 SHOP이라는 테이블은 가게명, 업주명, 사업자번호, 가게 연락처 등등 수많은 필드를 필요로 한다. 삽입, 수정, 삭제와 같은 Command는 이런 모든 필드들을 필요로 한다. 하지만 조회하는 입장에서 필요한 정보는 가게명, 업소명, 사업자번호 등으로 소수일 수 있다.
    
    이런 상황에서 Query를 위한 DB와 Command를 위한 DB를 분리한다면 Query용 DB는 최소한의 데이터로 관리가 될 것이고 이로 인해 조회 성능 또한 올라간다. 또한 Command용 DB에 문제가 생긴다고 하더라도 조회용 DB는 정상적이라면 정삭적으로 서비스를 운영할 수 있다. 즉 장애 격리가 가능하다.
    
    --- 

    www.youtube.com/watch?v=BnS6343GTkY

### 어떻게 구현할 수 있을까?

    두 DB의 싱크를 맞추는 것이 중요하다.
    
    가게노출 DB는 Query용, 가게/업주 DB는 Command용으로 나뉘어져 있다고 하자.
    
    가게/업주 DB에 변경이 발생하면 가게노출 DB로 이벤트가 전송된다.

    이벤트를 받은 가게/업주 DB는 변경된 데이터로 가지고 있는 데이터를 변경해준다.
    
    ---
    
    www.youtube.com/watch?v=BnS6343GTkY