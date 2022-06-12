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
    
### Statement와 PreparedStatement의 차이는 무엇일까요?★
    
    PreparedStatement는 SQL쿼리의 틀을 미리 생성해 놓고 값을 나중에 지정합니다.
    
    PreparedStatement는 쿼리에 알맞게 값 변환을 자동으로 진행해줍니다.
    (”손’지우” ⇒ “손’’지우” , TIMESTAMP나 DATE, TIME 등도 DBMS마다 표현 방식이 다른데 Statement를 사용하면 다 다르게 작성해야 한다.)
    
    PreparedStatement는 코드를 깔끔하게 만들어 주어 개발자가 실수한 여지를 줄여 줍니다.
    
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
    
    최종 쿼리는 다음과 같습니다.
    
    SELECT * FROM users where id="1; DROP TABLE users;"
    
    따라서 SQLInjection을 예방할 수 있습니다.
    
    ---
    
    최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 407p,2021
    