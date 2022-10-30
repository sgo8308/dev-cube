### JDBC란 무엇인가? 어떤 장점을 가지고 있을까요?★
    
    Java DataBase Connectivity의 약자로서 자바에서 데이터베이스와 관련된 작업을 처리할 때 사용하는 API입니다.
    
    DBMS 종류에 상관없이 일관된 인터페이스로 모든 DBMS에 대해서 동일한 방식으로 작업을 할 수 있다는 것이 장점입니다.
    
    Mybatis를 사용하던 Jpa를 사용하던 내부적으로 JDBC를 사용합니다.
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 377p,2021
    
### JDBC의 한계는?
    
    데이터베이스를 변경하였을 때 JDBC를 사용하는 코드는 변경할 필요는 없지만 SQL은 변경해주어야 할 때가 많다.
    OCP가 완벽하게 이루어지지는 않는다.
    
    ---
    
    김연한, 스프링 DB 1편, 인프런, JDBC 이해 파트

### Statement 와 PreparedStatement 의 차이는 무엇일까요?★
    1. 코드를 깔끔하게 만들어준다.

       PreparedStatement는 SQL쿼리의 틀을 미리 생성해 놓고 값을 나중에 지정합니다.

       PreparedStatement는 쿼리에 알맞게 값 변환을 자동으로 진행해줍니다.
       (”손’지우” ⇒ “손’’지우” , TIMESTAMP나 DATE, TIME 등도 DBMS마다 표현 방식이 다른데 Statement를 사용하면 다 다르게 작성해야 한다.)

       PreparedStatement는 코드를 깔끔하게 만들어 주어 개발자가 실수한 여지를 줄여 줍니다.

    2. SQL Injection을 예방해준다.

       PrparedStatement는 SQL틀 자체를 미리 DBMS에 보내고 data는 나중에 보내는 방식으로 SQLInjection 코드가 SQL 쿼리의 틀을 바꾸지 못하게 만들어서 SQLInjection을 방지합니다.

       Statement 쿼리와 PreparedStatement가 최종적으로 실행될 쿼리를 비교해보겠습니다.

       예를 들어

        $sql= "SELECT * FROM users where id=$expected_data";

       라는 SQL이 있을 때

        $spoiled_data = "1; DROP TABLE users;"

       해커가 데이터를 이런식으로 집어넣으면 최종 SQL은 이렇게 됩니다.

        SELECT * FROM users where id=1; DROP TABLE users;

       즉 users 테이블이 DROP됩니다.

       하지만 PreparesStatement를 이용해 다음과 같이 사용한다면

        PreparedStatement query = 
        	conn.prepareStatement("SELECT * FROM users where id=?");
        query.setString("1; DROP TABLE users;") 

       여기서 ?를 ‘bind variable’이라고 합니다.

       최종 쿼리는 다음과 같습니다.

        SELECT * FROM users where id="1; DROP TABLE users;"

       따라서 SQLInjection을 예방할 수 있습니다.

    3. 성능이 좋다.

       DB는 내부적으로 SQL을 파싱하는 작업을 다음과 같이 진행합니다.

        1. 구문 오류 체크
        2. 공유 영역에서 해당 구문 검색
        3. 권한 체크
        4. 실행 계획 수립
        5. 실행 계획 공유 영역에 저장

       이 후 쿼리를 실행하게 됩니다.

       이 때 공유 영역은 쿼리를 캐싱하는 공간이며, 만약 이전에 동일한 쿼리가 실행된 적이 있어서 공유 영역에서 동일한 쿼리를 발견하게 되면 3~5번 과정을 건너 뛰게 됩니다. 1~5번을 모두 수행하는 parsing을 Hard Parsing이라고 하며 3~5번을 건너뛰는 parsing을 Soft Parsing이라고 합니다.

       만약 Statement 방식을 사용하게 되면 매번 바뀌는 변수만큼 서로 다른 SQL이 공유 영역에 저장됩니다.

       예를 들어 다음과 같이 비슷한 구조를 가진 쿼리가 캐싱의 혜택을 받지 못하게 됩니다.

       SELECT * FROM product WHERE id = 1

       SELECT * FROM product WHERE id = 2

       SELECT * FROM product WHERE id = 3

       하지만 PreparedStatement를 사용할 경우,

       SELECT * FROM product WHERE id = ?

       이 쿼리가 공유 영역에 저장되고, 따라서 Soft Parsing이 이루어지게 되어 성능 향상이 일어납니다.

    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 407p,2021
    https://pinokio0702.tistory.com/54](https://pinokio0702.tistory.com/54

    