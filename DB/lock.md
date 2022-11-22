### 락을 왜 공부해야할까?
    
    DB에서 동시적으로 트랜잭션이 진행될 때 문제가 발생하지 않기 위해서는 락에 대해 잘 알아야 한다.
    
### 락을 실무에서도 직접 자주 사용할까?
    
    락을 거는 것은 요즘 같은 실시간 애플리케이션에서는 여러가지 위험이 많습니다.(데드락 문제 등등)
    
    그래서 각 데이터베이스 특성에 맞는 락 사용 방법을 명확하게 이해하고, 또 락이 걸린 로우를 조회했을 때 어떤 예외가 발생하는지 등등 다양한 테스트를 꼭 해보아야 합니다.
    
    저는 가급적이면 락을 거는 방법은 피하고, 낙관적 락이나, CAS 스타일의 방식, 또는 RDB나 REDIS 등을 사용해서 해당 주문번호를 한번에 한명한 변경할 수 있도록 로직의 입구에서 막는 방법을 선호합니다.
    
    ---
    
    [https://www.inflearn.com/questions/92603](https://www.inflearn.com/questions/92603)
    
### 락이란??
    
    락은 크게 2가지 종류가 있는데 둘을 아우르는 정의를 내리기가 어렵다.
    
    일반적으로 생각하는 락은 **전용 락(Exclusive Lock)**이다.
    
    이 락은 일종의 권한이다. 단 하나의 트랜잭션만 이 락을 얻어 락을 얻은 데이터에 대해서 읽기와 쓰기 작업을 할 수 있다. 
    만약 다른 트랜잭션이 이 데이터에 대해 읽거나 쓰고 싶다면 앞 트랜잭션이 락을 반환할 때까지 기다려야 한다.
    
    두번째 락은 **공용 락(Shared Lock)**이다.
    
    이 락은 일종의 선포다. “이 데이터는 공용으로 쓰자! 대신에 쓰기 작업은 하지 말자!”
    
    하나의 트랜잭션이 이 락을 얻게 되면 다른 다른 트랜잭션은 똑같이 공용 락을 사용해서 접근할 수 있지만 전용 락을 사용할 수는 없다.
    
    공용 락을 사용하는 트랜잭션은 읽기 작업만 할 수 있다. 따라서 쓰기 작업을 하려면 이 공용 락이 모두 반환될 때까지 기다려야 한다.
    
    - 삭제와, 생성도 락이 걸릴까??
        - 삭제
            
            삭제할 때도 락이 걸린다. 삭제 명령 날리고 커밋하기 전에 다른 커넥션에서 업데이트 치면 펜딩된다. 그리고 커밋하면 업데이트가 적용됐다고 뜨는데 실제로 보면 삭제 되어 있다. 
            
            - 근데 굳이 왜 삭제할 때 락을 걸까? 어차피 삭제하고 나면 결과는 같은데
                
                삭제하고 나서 다른 트랜잭션이 업데이트 하고 롤백 되면 ?
                
        - 생성
            
            생성할 때도 락이 걸린다.
            
        
        ---
        
        [https://en.wikipedia.org/wiki/Two-phase_locking#:~:text=Write-lock (exclusive lock) is associated with a database object by a transaction (Terminology%3A "the transaction locks the object%2C" or "acquires lock for it") before writing (inserting/modifying/deleting) this object](https://en.wikipedia.org/wiki/Two-phase_locking#:~:text=Write%2Dlock%20(exclusive%20lock)%20is%20associated%20with%20a%20database%20object%20by%20a%20transaction%20(Terminology%3A%20%22the%20transaction%20locks%20the%20object%2C%22%20or%20%22acquires%20lock%20for%20it%22)%20before%20writing%20(inserting/modifying/deleting)%20this%20object).
        
### 락을 사용해야 하는 이유는?
    
    트랜잭션이 동시적으로 수행될 때 문제가 없기 위해서

### 어떤 방식으로 동작할까?
    
    락 매니저란 것이 존재하며 트랜잭션들은 이 락 매니저와 메시지를 주고 받는다.
    
    이를 테면 락을 얻고 싶다 락을 해제하고 싶다 등의 메세지를 보내면 락 매니저는 락 상황을 파악하고 허용하거나 대기시킨다.
    
    이 때 락 매니저는 lock table이란 것을 통해 이 작업을 진행한다.
    
    lock table은 해시테이블 자료구조로써 lock이 요청되는 데이터를 key로 갖는다.
    
    이 해시테이블에는 각 버켓마다 연결리스트가 들어 있고 해시 충돌이 난다면 seperate chaining 방식으로 해결한다.
    
    이 연결리스트에 모든 노드는 다음 정보를 갖고 있다.
    
    1. lock을 요청한 트랜잭션에 대한 정보
    2. lock의 종류 (공용, 전용)
    3. 현재 lock을 얻은 상태인지, 기다리는 상태인지
    
![Untitled](https://user-images.githubusercontent.com/71138398/177580430-49ac6478-85b6-4336-b16a-efa3e4db5436.png)
    
    - 위 그림을 보면 I23은 데이터를 나타낸다.
    - T1과 T8은 현재 락이 허용된 트랜잭션 데이터고 T2는 아직 기다리는 트랜잭션 데이터다.
    
    **동작 방식**
    
    - Transaction이 Lock을 요청할 때
        
        어떤 트랜잭션이 lock을 요청하면 LockManager는 lock을 요청한 데이터의 인덱스에 있는 연결리스트에 추가한다.
        
        만약 요청한 데이터에 lock 걸려있지 않다면 lock을 얻게 해주고 lock이 걸려있더라도 걸려 있는 lock과 compatible한 lock이라면 lock을 얻게 해준다.
        
    - Transaction이 unlock을 요청할 때
        
        어떤 트랜잭션이 unlock을 요청할 경우 그 트랜잭션을 연결리스트에서 삭제하고 연결리스트에 연결된 다음 트랜잭션부터 순서대로 락 획득을 진행시킨다.
        
    - Transaction이 문제가 생겨서 롤백될 때
        
        LockManager는 이 트랜잭션이 잡고 있는 모든 락을 해제시키고 모든 연결리스트에서 이 Transaction을 제거한다.
        
    
    ---
    
    Silberschatz, Database System Concepts, Seventh Edition, 844-846, 2020
    
### 락의 장점과 단점은?
    
    동시적인 트랜잭션에서 발생할 수 있는 문제를 해결해 줄 수 있지만 동시에 성능을 떨어트릴 수 있다.
## 비관적 락 낙관적 락
    
### 비관적 락과 낙관적 락이란? ★
    
    만약 두 트랜잭션이 동시에 1000이라는 데이터를 읽고 그 데이터에 1000을 더하는 연산을 하자.
    
    이 때 정상적인 결과는 1000을 두 번 더했으니 3000이 나와야 할 것이다.
    
    하지만 두 트랜잭션이 동시에 1000이라는 것을 읽고 각자 1000을 더해 2000을 만들어 업데이트 하면 최종적인 결과는 2000이 된다. 
    이렇게 두 트랜잭션이 동시에 데이터를 수정하는 것을 충돌이 일어났다고 표현한다.
    
    만약 Isolation Level이 Serializable인 경우 해결될 수 있으나 MySQL에서는 심지어 Serializable임에도 불구하고 이런 현상이 나타날 수 있다.
    
    이런 경우에 낙관적 락 또는 비관적 락을 사용해야 한다.
    
    - 낙관적 락
        
        낙관적 락이란 상황을 낙관적으로 보는 것이다. 위와 같은 충돌 사태는 잘 일어나지 않는다. 
        그렇기 때문에 굳이 락을 걸 필요는 없고 커밋하기 전에 다른 누군가가 혹시 수정했는지만 확인하자. 라는 마인드다.
        
        이 때 만약 다른 누군가가 수정했다면 지금까지 진행상황을 롤백시키고 다시 시작한다.
        
    - 비관적 락
        
        비관적 락이란 상황을 비관적으로 보는 것이다. 위와 같은 충돌 사태는 자주 일어날 수 밖에 없으니 항상 락을 걸자. 라는 마인드다.
        
        이 때 중요한 것은 읽기 작업부터 전용 락을 걸어야 한다는 것이다. 왜냐하면 이 문제는 두 트랜잭션이 동일한 값을 읽어가는 것이 원인이기 때문이다.
        
        따라서 이 때는 모든 트랜잭션이 데이터 수정을 위한 SELECT를 할 때마다 SELECT … FOR UPDATE 구문을 통해 읽기를 할 때부터 전용 락을 걸어줄 필요가 있다.
        이렇게 할 경우 여러 트래잭션이 순서대로 진행되기 때문에 위와 같은 문제가 일어나지 않는다.
### DB 관련해서 낙관적 락은 어떻게 구현할까?
    
    Select id, balance, version from account where id="1";
    Query results: id=1, balance=1000, version=1
    
    Update the account
    Set the balance = the balance + 100, version = version + 1
    Where id = "1" and version = 1
    
    위와 같이 version 정보를 사용하는데, select할 때와 동일한 버전일 때만 update하게 되고 그렇지 않을 경우 실패하게 된다. 실패하는 경우 보통 아예 요청을 실패시키며 될 때까지 시도할 수도 있다.
    
    또한 Java의 Atomic 객체에서 CAS 알고리즘에 낙관적 락이 사용되며 이 때는 될 때가지 시도한다.[[2]](https://accessun.github.io/2017/03/12/Optimistic-locking-and-CAS-algorithm/#:~:text=Optimistic%20locking%20in%20Java%20is%20implemented%20using%20an%20algorithm%20called%20CAS%2C%20which%20is%20%E2%80%9CCompare%2DAnd%2DSet%E2%80%9D%20or%20%E2%80%9CCompare%2DAnd%2DSwap%E2%80%9D.%20By%20using%20this%20algorithm%2C%20programs%20no%20longer%20have%20to%20lock%20up%20the%20whole%20critical%20section.%20It%20just%20attempts%20to%20update%20the%20shared%20resource.)
    
    ---
    
    코드 참고 : [https://topic.alibabacloud.com/a/mybatis-how-to-use-optimistic-locks_8_8_31118982.html](https://topic.alibabacloud.com/a/mybatis-how-to-use-optimistic-locks_8_8_31118982.html)

### 각각의 장단점은 무엇이고 언제 어떤 것을 쓰는 것이 좋을까?
    - 낙관적 락
        - 장점
            1. 낙관적 락은 락을 걸지 않기 때문에 병목이 발생하지 않아 성능에 좋다.
        - 단점
            1. conflict가 발생하면 요청이 실패되거나 또는 재시도를 위한 로직을 새로 짜야한다.
            2. conflict가 발생할 때 마다 앞서 진행된 수정사항이 있다면 롤백이 일어나야 하고 새로 트랜잭션을 실행해야 한다. conflict가 자주 발생하게 되면 이는 심각한 성능 저하를 초래한다.
        
        ⇒ 동시성 문제가 발생하긴 하지만 정말 가끔 발생할 때 좋다. 왜냐하면 자주 발생하면 자주 실패가 발생한다.
            만약 요청이 아예 실패되게끔 구현했으면 사용자 경험이 안 좋아지고, 재시도하게끔 구현했으면 계속해서 재요청하게 되므로 mysql서버에 부하가 많이 갈 수가 있고 네트워크를 많이 사용하게 된다.
        
    - 비관적 락
        - 단점
            1. 데드락의 문제가 있을 수 있다.
            2. 경합이 발생하지 않아도 매번 락을 획득하고 릴리즈해주어야 해서 성능상 좋지 않다.
            3. 다른 트랜잭션들이 기다려야 하기 때문에 병목이 생길 수 있다. 낙관적 락을 경합시 실패시키게 하면 병목이 안생기게 할 수 있다.
            
    
    ⇒ 낙관적 락은 동시적인 트랜잭션이 적게 일어나는 곳에서 사용하고 비관적 락은 많이 일어나는 곳에서 사용한다. 왜냐하면 동시적인 트랜잭션이 적을 때는 낙관적 락이 성능상 유리하고 많을 때는 비관적 락이 유리하기 때문이다. 하지만 비관적 락은 데드락의 위험이 있기 때문에 주의해서 사용해야 한다.
    
    ---
    
    [https://ko.wikipedia.org/wiki/낙관적_병행_수행_제어](https://ko.wikipedia.org/wiki/%EB%82%99%EA%B4%80%EC%A0%81_%EB%B3%91%ED%96%89_%EC%88%98%ED%96%89_%EC%A0%9C%EC%96%B4)
---
## 분산 락
### 정의는?
    
    하나의 머신이 아니라 여러 머신에서 하나의 자원을 상호 배타적으로 사용해야 할 때 이용하는 락. 
    
### 어떻게 구현할까?
    
    여러 서버에서 MySQL에 있느 어떤 공유 자원을 업데이트 한다고 해보자.
    
    - MySQL에 Lock용 테이블을 따로 만들기.
        
        A라는 작업을 한다고 해보자. A 작업을 하려면 이 Lock용 테이블에 접근해서 work라는 필드에 A라는 값을 가진 레코드를 생성해야 한다. 이 때 이 work라는 필드에 unique제약조건을 걸어준다.
        
        처음 record를 집어넣은 트랜잭션만 작업을 수행할 수 있다. 그렇지 않은 트랜잭션은 insert하기 위해 기다리거나 아니면 락을 얻는 트랜잭션과 작업 트랜잭션이 분리되어 있을 경우에는 unique제약 조건에 걸려서 insert에 실패하게 됨으로써 요청이 실패하게 된다. 
        
        lock을 해제할 때는 insert했떤 A레코드를 삭제해준다.
        
        ---
        
        [https://programmer.ink/think/principle-and-implementation-of-distributed-lock.html#:~:text=To create a database table%3A](https://programmer.ink/think/principle-and-implementation-of-distributed-lock.html#:~:text=To%20create%20a%20database%20table%3A)
        
    - MySQL에 네임드락 사용하기
        
        A라는 작업을 한다고 해보자. A라는 이름을 가진 네임드락을 얻어야 이 작업을 할 수 있게 구성한다. 
        
        ---
        
        [https://techblog.woowahan.com/2631/](https://techblog.woowahan.com/2631/)
        
    - Redis 사용하기
        
        A라는 작업을 수행하려면 Redis에 먼저 접근해서 A라는 Key을 가진 데이터를 생성한다. 누군가 이미 생성했다면 계속해서 다시 요청할 수도 있으며(스핀락) 기다렸다가 Redis가 락을 얻을 수 있다고 통지해주면 그 때 다시 요청할 수도 있다.(pub/sub)
        
        Redis를 이용한 방식은 lock에 사용된 key가 일정시간이 지나면 사라지게끔 할 수 있어서 데드락을 회피하는데 도움을 주고 락을 기다리는 다른 트랜잭션들의 Timeout시간을 정해주기가 쉬워서 마찬가지로 데드락을 피하는데 도움을 준다.
        
        ---
        
        [https://hyperconnect.github.io/2019/11/15/redis-distributed-lock-1.html](https://hyperconnect.github.io/2019/11/15/redis-distributed-lock-1.html)
---
### InnoDB는 인덱스 잠금을 한다는데 무슨 말일까?
    
    **간결** 
    
    레코드가 아닌 인덱스를 잠그기 때문에 어떤 실제 update가 일어나는 레코드와 상관없는 레코드를 수정하는 쿼리도 만약 같은 인덱스 데이터를 스캔하려한다면 기다려야 한다.
    
    **상세**
    
    다음과 같은 테이블이 있다고 해보자
    
    TABLE : Employee  Index first_name
    
    emp_no   first_name  last_name
    1        jay         shin
    2        jiwoo       shon
    3        kan         tom
    4        kang        utang
    5        kim         vertex
    6        lang        won
    
    이 때 다음과 같은 쿼리를 날린다고 해보자.
    
    update Employee set first_name= 'baby' where first_name='kan' and last_name = 'tom';
    
    이 때 innoDB는 인덱스를 통해 first_name이 ‘kan’인 데이터를 찾고 last_name이 ‘tom’인 값을 찾기 위해 아래에 모든 인덱스를 스캔할 것이다.
    
    이 때 emp_no 3, 4, 5, 6 을 가진 인덱스가 잠긴다.
    
    이 때 다음과 같은 쿼리들은 앞 쿼리가 잠금을 반환할 때까지 기다려야 한다.
    
    update Employee set first_name='big' where id = 4;
    
    update Employee set first_name='zoo' where id <= 6 and name = 'jiwoo';
    
    왜냐하면 이 두 쿼리는 모두 잠긴 인덱스를 스캔해야 하기 때문이다.
    
    첫 쿼리는 emp_no 4번을 스캔해야 하고, 두번째 쿼리는 6,5,4,3,2,1번을 스캔해야 한다.
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0 1권, 2쇄, 위키북스, 170-172p, 2022
    
    직접 2개의 커넥션을 이용해 테스트
    
    - 왜 인덱스 잠금을 할까? 그리고 다른 DB도 update를 위한 대상을 찾기 위해 스캔하는 모든 대상을 잠금하나?
        
        인덱스 잠금을 하는 이유는 MySQL이 REAPEATABLE READ부터 뭔가 
        
    - 인덱스를 타지 않는 쿼리는?
        
        인덱스를 타지 않는
        
### MySQL에서 REPEATABLE READ isolation level과 READ COMMITED isolation level의 차이는 무엇이 있을까?
    
    일반적인 차이는 제외하고 중요한 차이점 2가지가 있다.
    
    1. 갭 락이 사용되지 않는다. 
    2. 스캔된 인덱스 중 매칭되지 않은 row의 인덱스 잠금이 해제된다. 
### 락은 자동으로 얻어질까 수동으로 얻어야 할까?
    
    MySQL InnoDB는 삭제 또는 업데이트를 할 때 자동으로 락을 얻고 트랜잭션이 커밋되거나 롤백될 때 락을 해제한다.
    
    하지만 수동으로 락을 얻을 수도 있다. 
    
### 공용 락은 왜 필요할까? 그냥 읽으면 되는 거 아닌가?
    
    REAPEATABLE READ Isolation Level을 지원하려면 이 기능이 필요하다. 만약 한 트랜잭션 안에서 데이터를 반복적으로 읽을 때 다른 트랜잭션이 데이터를 변경한다면 처음 읽은 값과 나중에 읽은 값이 달라질 수 있다. 
    
    이 대 공용 락을 이용해서 읽기는 가능하지만 쓰기는 불가능하게 만들면 REAPEATABLE READ를 달성할 수 있다.
    
    한 편 이 방법 말고도 MVCC를 이용해서 달성할 수도 있다.
    
### 왜 MySQL은 update나 delete 할 때 자동으로 lock을 얻도록 했을까?
    
    만약 한 트랜잭션이 어떤 데이터를 수정하고 아직 커밋을 안한 상황이다.
    
    이 때 다른 트랜잭션이 이 데이터를 다시 수정했다.
    
    이 상황에서 만약 첫 트랜잭션이 문제가 생격서 롤백을 해버리면 어떻게 할까?
    
    처음 트랜잭션은 롤백을 했고 그 다음 트랜잭션은 수정 중이다.
    
    이런 식으로 많은 케이스가 꼬이게 되기 때문에 자동을 lock 을 얻도록 한 것이다.
    
    ---
    
    김영한, 스프링 DB 1편 - 데이터 접근 핵심 원리, 인프런, DB 락 - 개념 이해 파트
    
### 만약 두 트랜잭션이 동시에 어떤 데이터를 읽고 그 데이터에 1000을 더하는 연산을 한다면 어떻게 이 동시성 문제를 해결할 수 있을까?
    
    이 문제는 두 트랜잭션이 같은 데이터를 가져가지 못하게 해야한다.
    
    따라서 두 트랜잭션 모두 SELECT를 SELECT … FOR UPDATE 방식으로 진행해야 한다.
    
    이렇게 하면 두번째로 들어온 트랜잭션은 처음 들어간 트랜잭션이 락을 반납할 때까지 기다린 후에 데이터를 읽을 수 있다.
    
    만약 하나의 트랜잭션은 SELECT … FOR UPDATE를 하고 다른 하나는 그냥 SELECT를 한다면 이 방법도 무용지물이 된다.
    
    더 안전하게 하려면 한 세션이 자신의 isolation level을 Serializable로 만들고 SELECT FOR UPDATE를 진행하면 다른 트랜잭션이 어떤 방법을 사용하던지 상관없이 문제를 해결할 수 있다.
    
    다른 트랜잭션이 이 데이터에 접근할 때는 SELECT … FOR SHARE로 자동 전환되기 때문에 첫 트랜잭션이 전용 락을 반납하기 전까지 기다려야 하기 때문이다.
    
### InnoDB는 기본적으로 테이블 잠금을 사용할까 레코드 잠금을 사용할까?
    
    DDL 쿼리에 대해서는 테이블 락이 걸리고 DML 쿼리에 대해서는 레코드 락이 걸린다. 
    
    예를 들어 한 트랜잭션이 Fruit이라는 테이블에 id=20을 가진 레코드에 데이터 변경 작업을 시작한다고 해보자. 
    
    이 때 Fruit 테이블에 DDL 쿼리 작업을 하면 이 변경 작업이 커밋될 때까지 기다려야 한다.
    
    하지만 Fruilt 테이블에 id=20이외에는 DML 쿼리 작업을 하면 기다릴 필요 없이 바로 작업이 가능하다. 
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0 1권, 2쇄, 위키북스, 163p, 2022
    
### 락을 데이터베이스 전체에 잡거나 칼럼 하나에만 잡듯 크게 잡거나 작게 잡는것의 장단점은?
    
    간결
    
    크게 잡을수록 제어는 간단해지지만 성능이 떨어지고 작게 잡을수록 성능은 좋아지지 만 제어가 어렵다.
    
    상세
    
    lock 연산은 크게는 전체 데이터베이스부터 작게는 데이터베이스를 구성하는 [속성](https://terms.naver.com/entry.nhn?docId=3431117&ref=y)에 이르기까지 다양한 크기의 데이터를 대상으로 실행할 수 있다. 릴레이션이나 [투플](https://terms.naver.com/entry.nhn?docId=3431137&ref=y)도 lock 연산의 대상이 될 수 있다. 만약 전체 데이터베이스에 lock 연산을 실행하면 제어가 간단하다는 장점이 있지만 데이터베이스에 하나의 트랜잭션만 수행되므로 병행 수행이라 할 수 없다.
    
    반면, 가장 작은 단위인 속성에 lock 연산을 하면 독점하는 범위가 좁아 많은 수의 트랜잭션이 병행 수행할 수 있다는 장점이 있지만, 제어가 복잡하다는 단점이 있다. 즉, 로킹 단위가 커질수록 병행성은 낮아지지만 제어가 쉽고, 로킹 단위가 작아질수록 제어가 어렵지만 병행성은 높아진다. 그러므로 시스템에 따라 적절한 로킹 단위를 선택하는 것이 중요하다.
    
    ---
    
    **[네이버 지식백과]** [로킹 기법의 개념](https://terms.naver.com/entry.naver?docId=3431286) (데이터베이스 개론, 2013. 6. 30., 김연희)

<details>
<summary>MVCC란 무엇이고 왜 사용할까?</summary>
<br>
Multi Version Concurrency Control의 약자로 하나의 row에 대해 여러 버전을 저장해서, 잠금없는 읽기는 제공한다.

Read Committed 이상의 Isolaton Level에서 하나의 트랜잭션이 특정 row에 업데이트를 하고 아직 commit하지 않은 상황이라고 해보자.

이 때 MVCC를 사용하지 않는 DBMS는 커밋되지 않은 데이터를 읽지 않기 위해서 공용락을 걸고 SELECT해야 하는 규칙을 걸 것이다. 

그래야 쓰기락에 막혀서 업데이트 중인 데이터를 읽지 않게 된다. 

하지만 MVCC를 쓰게 된다면 이전 버전의 데이터가 다른 공간에 저장되어 있기 때문에 이것을 읽으면 된다.

즉 잠금없는 읽기가 가능해진 것이다.

MVCC를 통해 쓰기와 읽기가 공존할 수 있게 되고, 성능도 좋아지며, 데드락의 가능성도 줄어든다.
</details>