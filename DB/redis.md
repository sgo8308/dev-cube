### In-memory DB란?
    
    RDBMS와 같이 데이터를 디스크에 저장하는 것이 아니라 메모리에 저장하는 DB를 말한다. **성능** 문제를 해결하기 위해 나왔다.
    
### Redis란 무엇일까?
    
    #성능 
    
    Redis는 In-memory 데이터 저장소이다. 메모리 기반으로 동작하기 때문에 매우 빠르게 데이터를 검색하거나 저장할 수 있다. 메모리 기반이기 때문에 영구적으로 저장되지 않는다. 따라서 영구적인 데이터 저장용으로는 적합하지 않다.
    
    Redis는 **Re**mot **Di**ctionary **S**erver의 약자로  key-value 형식의 저장소이다. key를 통해서 저장된 값을 찾아올 수 있다.
    
    Redis는 **확장성**과 관련된 문제를 해결하기 위해 나왔다.
    
### Redis는 Key Value 형식으로 저장하는데 List나 Set 자료 구조는 뭘까?
    
    Redis는 기본적으로 거대한 해시 테이블이라 생각하면 된다. 그리고 
    
    List나 Set은 value가 list도 될 수 있고 set도 될 수 있다는 의미다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dede1e16-584d-41c8-ae95-cf868568f870/Untitled.png)
    
    위와 같이 이 해시 테이블에는 여러 타입의 value를 담을 수 있는데 그 value는 string이 될 수도 있고 List가 될 수도 있고 Set이 될 수도 있다.
    
    만약 SET str “test” 라고 한다면 str이라는 key의 value는 String으로 정해진다.
    
    만약 LPUSH list “test” 라고 한다면 list라는 key의 value는 List으로 정해진다.
    
    이런 식으로 그 key에 처음 작업한 Operation에 따라 그 안에 담기는 Value의 자료구조가 정해지며 이후에 이 key는 value의 자료구조에 맞는Operation만 수행 가능하다.
    
    위 예제에서는 LPUSH str “hello”라는 Operation은 오류를 일으킨다. 왜냐하면 str이라는 key의 value의 데이터타입은 Strings로 처음에 정해졌기 때문이다.
    
    ---
    
    패스트 캠퍼스 The Red 백엔드 에센셜 with 강대명 - redis 자료구조 파트
    
### Redis를 사용할 때 주의할 점은 무엇이 있을까?
    - TTL 관련 문제
        
        하나의 엔티티를 저장한다고 할 때 Expire 시간을 두고 따로 따로 저장할 경우 데이터가 서로동일한 시간에 생겼다 동일한 시간에 사라지지 않을 수 있다. 
        
        예를 들어 MySQL로 치면 다음과 같은 스키마가 있다고 가정하자.
        
        Create table Users(
        	name varchar(100),
        	email varchar(100)
        );
        
        Redis에 이 데이터를 저장한다면 다음과 같이 저장할 수도 있다.
        
        set name:jiwoo "jiwoo"
        set email:jiwoo "abc123@naver.com"
        
        이 때 각 데이터에 만료 시간(TTL)이 있을 경우 어느 시점에 두 데이터 중 한 쪽만 존재할 수도 있다는 의미이다.
        
        따라서 이 경우는 strings로 저장하는게 아니라 hash로 저장한다면 위와 같은 상황을 막을 수 있다.
        
        hash로 저장한다면 다음과 같이 저장할 수 있을 것이다.
        
        hset jiwoo "name" "jiwoo"
        hset jiwoo "email" "abac123@naver.com"
        
        이렇게 되면 하나의 key에 담기게 되므로 TTL에 의해 둘 중 하나만 존재하는 상황은 벌어지지 않는다. 마치 key 하나가 객체 하나에 대응되는 셈이라 할 수 있다.
        
        ---
        
        패스트 캠퍼스 The Red 백엔드 에센셜 with 강대명 - redis 자료구조 파트
        
    - 싱글 쓰레드 관련 문제
        1. mset을 통해 너무 많은(일반적으로 50개 이상)의 데이터를 한 번에 저장한다면 이 작업을 하는 동안 다른 작업들은 너무 오래 진행이 안되기 때문에 나눠서 처리하는 것이 좋다. 그래야 갑작스럽게 시스템에 반응성이 느려지거나 하지 않고 일정한 **성능**을 기대할 수 있다.
        
        ---
        
        패스트 캠퍼스 The Red 백엔드 에센셜 with 강대명 - redis 자료구조 파트
        
    - 성능 관련 문제
        
        그냥 레디스를 사용하다 보면 뭔가 레디스가 느리다고 생각될 때가 있다. 레디스로 명령을 보내고 명령완료에 대한 응답을 받는 그 과정 사이에 시간 때문이다.
        
        이 때는 명령완료에 대한 응답을 받고 다음 명령을 보내는 것이 아니라 한꺼번에 명령을 보내고 한꺼번에 응답을 받는 방식을 사용할 수 있는데 이것을 Pipeline이라고 부른다.
        
        Redis가 제공하는 기능은 아니고 Library이다.
        
        실제로 100만개의 set을 수행하는 시간이 10배 이상 차이가 났다.
        
        ---
        
        패스트 캠퍼스 The Red 백엔드 에센셜 with 강대명 - redis 자료구조 파트
        
### Redis는 Copy On Write 방식으로 작동한다는게 무슨 말일까?
    
    Redis는 기본적으로 메모리와 디스크 방식을 동시에 가져간다. 디스크는 데이터를 백업해놓고 복구하는 용도이다. 
    
    디스크에 데이터를 백업하기 위해 Redis는 fork()를 통해 Redis에 메인 프로세스를 복제한다. 이 때 Copy On Write 방식이 사용되기 때문에 Child Process가 디스크에 데이터를 저장하는 동안 Parent Process에 특별히 변경사항이 발생하지 않으면 서로 동일한 페이지를 바라본다.
    
    때문에 fork()로 인해서 Process가 2개 생긴다고 해서 Memory가 더 많이 차지되지 않지만 만약 변경사항이 발생하면 Memory는 더 많이 차지될 것이다.
    
    즉 Redis는 실제로 필요한 메모리 용량보다 Disk에 백업하는 작업을 위해 더 많은 메모리를 필요로 할 수 있다.
    
    ---
    
    [https://redis.io/docs/getting-started/faq/#:~:text=the long one%3A-,The Redis background,-saving schema relies](https://redis.io/docs/getting-started/faq/#:~:text=the%20long%20one%3A-,The%20Redis%20background,-saving%20schema%20relies)
    
    [https://deveric.tistory.com/65?category=346637#:~:text=Memcached가 유리합니다.-,Redis는,-Copy%26Write 방식을](https://deveric.tistory.com/65?category=346637#:~:text=Memcached%EA%B0%80%20%EC%9C%A0%EB%A6%AC%ED%95%A9%EB%8B%88%EB%8B%A4.-,Redis%EB%8A%94,-Copy%26Write%20%EB%B0%A9%EC%8B%9D%EC%9D%84)
    
### vs Memcached

    Redis는 Memcached가 제공하는 모든 기능을 제공하고 있으나 Memcahced는 Redis가 할 수 없는 것들이 있다. 
    대표적으로 다양한 자료구조와 장애가 생겼을시 복구하는 기능이다. 
    Memcached는 String 자료구조만 지원하며 Consistent Hashing 기능을 사용하여 여러 Memcached 서버에 자료를 분산 저장이 가능하지만
    서버에 장애가 생겼을 경우 그 Memcached 서버에서 저장되고 있는 데이터는 잃어버릴 수 밖에 없다.

    Redis는 대신에 성능에서 약간 손해를 본다. Copy on Write 때문에 메모리를 좀 더 필요할 수 있으며 메모리 파편화 현상으로 대규모 트래픽이 발생할 시 안정적인 응답 속도가 나오지 않을 수 있다.
    또 싱글쓰레드이기 때문에 Scale Up시 여러 코어들을 많이 활용하기 힘들다.

    사실상 Redis와 Memcahced의 성능차이는 미미하다. Redis는 스프링 부트에서도 지원해주고 있어 개발과 유지보수가 편리하고, 
    장애 관련 기능으로 인해 Memcached보다 가용성이 좋다. 따라서 Redis를 사용하는 것이 좋다고 생각된다.
    