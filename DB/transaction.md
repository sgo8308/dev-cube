### 공부해야 하는 이유는?
    
    트랜잭션과 관련된 문제 상황이 있을 때 알아차리기 위해서
    
    실행하는 SQL 문장이 어떤 결과를 가져오게 되는지를 정확히 예측하려면 
    각 트랜잭션 격리 수준이 어떻게 작동하는지 알아야 한다.
    
### 정의는?
    
    ACID 특성을 만족하는 연속적인 데이터베이스 작업을 트랜잭션이라 한다.
    
    두 개 이상의 쿼리를 모두 성공적으로 실행해야 데이터가 정상적으로 처리되는 경우
    DBMS가 두 개 이상의 쿼리를 마치 한 개의 쿼리처럼 처리할 수 있도록 해주는 것.
    
    트랜잭션이 시작되면 트랜잭션 범위 내에 모든 쿼리 결과는 DB에 모두 반영되거나 모두 반영되지 않는다.
    
    innoDB를 사용하는 MySQL의 모든 SQL은 각각 하나의 트랜잭션이라 할 수 있다. 기본적으로 autocommit이 켜져있기 때문에 명시적으로 commit하지 않아도 이러한 트랜잭션들이 커밋되는 것이다.
    
    ---
    
    [https://en.wikipedia.org/wiki/ACID](https://en.wikipedia.org/wiki/ACID)
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 411p,2021
    
    [https://inpa.tistory.com/entry/MYSQL-📚-트랜잭션Transaction-이란-💯-정리#top](https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98Transaction-%EC%9D%B4%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC#top)
    
### 어떤 방식으로 동작할까?
    
    
    - 데이터 여러개일 때는?
### 잘 사용하려면 어떻게 해야할까?
    
    커넥션과 마찬가지로 트랜잭션이 활성화돼 있는 프로그램의 범위를 최소화해야 된다.

    또한 프로그램의 코드에서 라인 수는 한두 줄이라고 하더라도 메일 전송, FTP 파일 전송, 네트워크를 통해 원격 서버와 통신하는 등의 작업은 반드시 트랜잭션에서 제거하는 것이 좋다.

    왜냐하면 트랜잭션 안에서 위와 같은 작업이 문제가 생길수 있다.

    그렇게 되면 일단 커넥션을 계속 들고 있기 때문에 문제가 되고,

    또 트랜잭션 과정에서 MVCC를 위해 이전 버전이 저정되는데 이것이 제거되지 않는 상태로 계속 남아있게 되면서
    DBMS 서버가 높은 부하 상태에 빠지거나 위험한 상태에 빠질 수 있기 때문이다. 

    ---

    백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 159-160, 2022

    백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 104, 2022
    
### 단점은?
    
    
### ACID에 대해서 설명해주세요 ★
    
    트랜잭션이 가져야하는 특성이다.
    
    일련의 데이터베이스 작업이 ACID 특성을 만족하면 트랜잭션이라고 불리며 DBMS에서 제공하는 트랜잭션 기능은 ACID를 만족해야 한다.
    
    - A = Atomicity
        
        전체 트랜잭션은 한번에 일어나거나 일어나지 않아야 한다.
        
    - C = Constistency
        
        트랜잭션 이전과 이후에도 데이터들이 비즈니스 규칙에 맞게 일관성을 유지해야 한다.
        
        예를 들어 A + B = 100이어야 한다는 규칙이 있을 때 A에서 10을 빼는 작업을 할 경우 B는 10이 더해져야 일관성 있게 된다.
        
        혹은 A와 B칼럼에 정수를 넣기로 되어 있는데 소수가 들어오면 규칙에 위배되므로 커밋에 실패하게 된다.
        
        혹은 A라는 데이터베이스에 있는 지우의 계좌에서  B라는 계좌에 있는 영현의 계좌에 돈을 보낼 경우 두 계좌 금액의 총합은 트랜잭션 전과 후에 일치해야 한다. 그렇지 않으면 트랜잭션은 커밋되지 않는다.
        
        즉 Consistency는 하나의 테이블을 넘어 하나의 데이터베이스 여러 데이터베이스의 일관성을 아우르는 개념이다.
        
    - I = Isolation
        
        여러개의 트랜잭션이 서로에 대한 침범 없이 독립적으로 일어나야 한다.
        
        중간에 어떤 트랜잭션이 사용하고 있는 테이블이 다른 트랜잭션에 의해서 바뀌거나 하지 않는 것을 의미한다.
        
        이 특성은 성능 문제 때문에 isolation level에 따라 유연하게 적용될 수 있다.
        
    - D = Durability
        
        일단 트랜잭션이 커밋되면 이 후 시스템에 문제가 생기더라도 롤백되거나 바뀌는 일없이 커밋된 상태로 남아 있어야 한다. 트랜잭션이 커밋되고 디스크에 쓰이기 전에 문제가 생기더라도 시스템이 다시 복구되면 커밋된 데이터는 업데이트 되어야 한다.
        
    
    트랜잭션의 4가지 특성을 보장하기 위해서 DBMS는 다음과 같은 기능이 필요하다.
    
    - 회복 기능 : 장애가 발생했을 때 데이터베이스를 장애가 발생하기 전의 일관된 상태로 복구시키는 것.
    - 병행 제어 기능 : 여러 트랜잭션이 동시에 수행될 때 마치 트랜잭션이 순차적으로 실행되는 것처럼 제어하는 기능.
        
    DBMS는 락을 이용해서 병행 제어 기능을 수행할 수 있다.
        
    
    ---
    
    [https://www.geeksforgeeks.org/acid-properties-in-dbms/](https://www.geeksforgeeks.org/acid-properties-in-dbms/)
    
    [https://en.wikipedia.org/wiki/ACID](https://en.wikipedia.org/wiki/ACID)
    
    [https://www.tutorialspoint.com/dbms/dbms_transaction.htm](https://www.tutorialspoint.com/dbms/dbms_transaction.htm)
    
    [http://www.kocw.net/home/cview.do?cid=485196f2ed6fa411](http://www.kocw.net/home/cview.do?cid=485196f2ed6fa411)
### 트랜잭션 isolation level에 대해 생각나는대로 설명해주세요.★
    
    트랜잭션 isolation level이란 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지를 결정하는 것.
    
    - READ UNCOMMITTED
        
        Dirty read가 가능한 격리 수준이다.
        
        데이터 정합성에 문제가 많아 트랜잭션 격리 수준으로 인정하지 않을 정도다.
        
        - Dirty Read
            
            아직 commit 되지 않은 정보를 읽는 것
            
    - READ COMMITTED
        
        COMMIT이 완료된 데이터만 다른 트랜잭션이 볼 수 있고 COMMIT 전에는 기존 데이터를 보게 되는 격리 수준.
        
        오라클 DBMS에서 기본 격리 수준이며 온라인 서비스에서 가장 많이 선택되는 격리 수준이다. Dirty Read가 발생하지 않지만 NON-REPEATABLE READ와 PHANTOM-READ 문제가 있다.
        
        일반 웹 프로그램에서는 크게 문제가 되지 않기 때문에 이 격리 수준이 사용된다.
        
        - NON-REPEATABLE READ
            
            하나의 트랜잭션에서 같은 데이터를 2번 읽었을 때 값이 다르게 나오는 것
            
        - PHANTOM READ
            
            범위 검색 시 처음에는 없던 데이터가 중간에 다른 트랜잭션의 삽입으로 인해 보이게 되는 것
            범위 락을 사용하지 않는다면 이 문제가 나타난다. 
            
    - REPEATABLE READ
        
        말 그대로 REPEATABLE READ가 가능한 격리 수준이다.

        MVCC를 사용한다면 이전 버전의 데이터를 저장하고 자신보다 먼저 진행된 트랜잭션이 변경한 데이터만 SELECT하게함으로써 달성할 수 있다.

        MVCC를 사용하지 않는다면 SELECT할 때 공용 락을 걸어서 데이터의 수정이 일어나지 않게 함으로써 달성할 수 있다.

        PHANTOM READ의 문제가 있다.

        - 왜 PHANTOM READ의 문제가 생길까?
            - MVCC 방식에서
    
                MVCC 방식을 사용했을 때는 PHANTOM READ 현상이 SELECT를 할 때는 나타나지 않지만 SELECT … FOR UPDATE나 SELECT … LOCK IN SHARE MODE로 할 때 나타난다.
                
                왜냐하면 위 SELECT는 SELECT할 때 레코드에 쓰기 잠금을 걸어야 한다. 그런데 이전 버전의 데이터(언두 레코드)에는 쓰기 잠금을 걸을 수 없기 때문에 현재 데이터를 SELECT할 수밖에 없다. 따라서 PHANTOM READ 현상이 일어난다.
    
            - MVCC를 사용하지 않는 방식에서
            
                이 때는 앞에서도 말했듯이 공용 락을 사용하게 된다. 하지만 공용 락은 각각의 레코드에 걸리는 것이지 범위적으로 걸리는 것이 아니기 때문에 데이터가 추가되는 것은 막을 수 없다. 따라서 PHANTOM READ 현상이 일어난다.
        
            ---
        
        백은빈, 이성욱, Real MySQL 8.0 1권, 2쇄, 위키북스, 183p, 2022
        
    - SERIALIZABLE

    **간결한 설명**
    
    InnoDB에서는 한 트랜잭션이 읽고 있는 데이터에 대해서는 다른 트랜잭션도 읽기만 가능하고 쓰고 있는 데이터는 읽기도 불가능해진다.
    
    **상세한 설명**
    
    innoDB에서 모든 SELECT문은 SELECT … FOR SHARE로 변경된다.
    
    즉 공용 락을 걸게 된다. 따라서 한 트랜잭션이 데이터를 읽고 있을 때 다른 트랜잭션이 그 데이터를 읽을 수만 있고 다른 트랜잭션이 그 데이터를 변경할 수 없게 된다.
    
    또한 하나의 트랜잭션이 데이터를 쓰고 있다면 전용 락이 걸리게 되고 모든 SELECT는 공용락을 획득해야하기 때문에 읽으려고 해도 전용 락이 반환될 때까지 기다려야 한다.
    
    위에 있는 모든 문제가 없지만 가장 동시 처리 성능이 떨어진다.
    
    하지만 innoDB 스토리지 엔진에서는 갭 락과 넥스트 키 락 덕분에 REAPEATABLE READ 수준에서도 이미 팬텀 리드가 발생하지 않기 때문에 굳이 사용할 필요는 없다.

    - InnoDB는 어떻게 갭 락과 넥스트 키 락으로 팬텀 리드 문제를 없앴을까?

     넥스트 키 락은 레코드 락과 갭 락을 합친 것이다.

     다음과 같은 쿼리가 있다고 하자.
    
       SELECT * FROM child WHERE id > 100 FOR UPDATE;
    
     이 때 갭 락은 id는 100을 넘는 모든 레코드들에 대해 잠금을 획득하며, id가 100이 넘는 레코드가 새로 생성되는 것을 막는다.
    
     따라서 팬텀 리드 현상이 사라진다.
      
     ---
    
     [https://dev.mysql.com/doc/refman/8.0/en/innodb-next-key-locking.html#:~:text=15.7.4-,Phantom Rows,-The so-called](https://dev.mysql.com/doc/refman/8.0/en/innodb-next-key-locking.html#:~:text=15.7.4-,Phantom%20Rows,-The%20so%2Dcalled)
### 트랜잭션 Isolation Level이 생성, 수정, 삭제 등과도 관련이 있을까?
    
    관련 없다. 트랜잭션 Isolation Level은 트랜잭션끼리 변경하는 데이터를 볼 수 있냐 없냐에 관한 것이다.
    
    어떤 Isolation level에서도 하나의 트랜잭션에서 수정이 일어나면 lock이 걸리고 다른 트랜잭션은 만약 이 데이터에 수정 또는 삭제를 하고 싶다면 lock이 풀릴 때까지 기다려야 한다.
    
### 트랜잭션 isolation level과 lock은 무슨 관계가 있을까?
    
    각 Level의 특성을 달성하기 위해서 여러 종류의 Lock이 사용된다.
    
    예를 들어 REPEATABLE READ를 달성하기 위해서는 공용 락을 사용할 수 있으며,
    SERIALIZABLE을 달성하기 위해서 배타적 락을 사용하게 된다.
### MVCC란?
    Multi Version Concurrency Control의 로 잠금을 사용하지 않는 일관된 읽기를 제공하기 위해서 DBMS가 제공하는 기능이다.

    만약 트랜잭션 격리 수준이 READ COMMITTED 이상일 경우에 업데이트된 레코드를 읽으면 기존 버전의 값을 조회하게끔해서 Dirty Read를 방지하는 것이 MVCC이다.

    만약 MVCC를 제공하지 않는다면 READ COMMITTED isolation level을 달성하기 위해 읽기 할 때도 잠금(공용 락)을 사용해야 할 것 이다.

    이 기존 버전의 값은 InnoDB 스토리지 엔진의 경우 언두 로그라는 공간에 보관되는데 트랜잭션이 완료되고 InnoDB 스토리지 엔진이 불필요하다고 판단하는 시점에 주기적으로 삭제한다.

    ---

    백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 100-103, 2022

### MySQL에 세션에서 Isolation Level을 설정할 때는 어떻게 쿼리들이 작용할까?
    
    하나의 세션에서 Isolation Level을 바꾸면 그 세션이 작업하는 데이터들은 모두 이 Isolation Level 아래에서 이루어진다.
    
    즉 한 세션이 자신의 Isolation Level을 Serializable이라고 정하고 특정 데이터를 읽는다면 다른 세션은 자신의 Isolation Level이 Repeatable Read임에도 불구하고 이 데이터를 수정하지 못한다.