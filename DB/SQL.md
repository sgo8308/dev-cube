### SQLInjection이란 무엇일까요?★
    
    웹사이트에서 해커가 인풋에 SQL 쿼리와 관련된 것을 집어넣어서 데이터베이스에 마음대로 접근하는 해킹 기술을 말합니다.
    
    예를 들어 해커는 input 값으로 WHERE 조건문을 항상 참으로 만드는 SQL관련 문자를 집어넣을 수 있습니다.
    
    아이디와 비밀번호를 확인해서 유저정보를 가지고 오는 쿼리가 다음과 같다고 하겠습니다.
    
    sql = 'SELECT * FROM Users WHERE Name ="' + uName + '" AND Pass ="' + uPass + '"'
    
    해커는 아이디와 비밀번호에  " or ""=” 라는 값을 넣습니다.
    
    최종 SQL은 다음과 같습니다.
    
    SELECT * FROM Users WHERE Name ="" or ""="" AND Pass ="" or ""=""
    
    “”=””는 항상 참이기 때문에 이 쿼리는 유저테이블의 모든 정보를 반환합니다.
    
    ---
    
    [https://www.w3schools.com/sql/sql_injection.asp](https://www.w3schools.com/sql/sql_injection.asp)
    
### SQL 쿼리의 여러 절(SELECT, FROM, WHERE 등등) 작동 순서에 대해서 아는게 왜 중요할까?
    
    어떤 절이 먼저 실행되는지를 모르면 처리 내용이나 처리 결과를 예측할 수 없기 때문이다. 
    
    예를 들어 다음과 같은 테이블이 있다고 해보자
    
    Table : Users
    
    이름    나이
    홍길동   125
    임창정   42
    윤민수   49
    
    이 때 다음과 같은 쿼리를 날리면 결과는 어떻게 될까?
    
    SELECT * From Users
    ORDER BY 나이
    LIMIT 1;
    
    만약 LIMIT이 먼저 실행된다면 홍길동 레코드가 반환될 것이고,
    ORDER BY가 먼저 실행된다면 임창정 레코드가 반환될 것이다.
    
    이렇듯 실행 순서를 알아야 처리 결과를 예측할 수 있다.
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0 2권, 2쇄, 위키북스, 53p, 2022
    
### SELECT 문장에 올 수 있는 여러 절의 처리 순서를 설명해주세요★
    
    기본적으로 data 전체를 먼저 찾고 필터링하는 방식으로 진행됩니다.
    
    - Data 찾기
        1. FROM과 JOIN
            
            먼저 FROM과 JOIN절로 작업하고자 하는 테이블을 가지고 옵니다.
            
    
    - 필터링하기
        1. WHERE
            
            그 후 WHERE 절을 통해 해당하지 않는 레코드들을 필터링합니다.
            
            이 때 SELECT절에 있는 별칭은 아직 SELECT절이 실행되어 별칭이 적용되지 않았기 때문에 WHERE절에서 사용 불가능합니다.
            
        2. GROUP BY
        3. HAVING
        4. SELECT
            
            이 부분에서 SELECT 절에서 선택한 칼럼들로 값을 추리고, 함수나 연산이 있다면 적용하고, 별칭을 적용합니다.
            
            따라서 이 다음 절부터는 별칭 사용이 가능합니다.
            
        5. DISTINCT
        6. ORDER BY
            
            DISTINCT 이후 ORDER BY를 하는 것이 ORDER BY후 DISTINCT하는 것보다 성능상 좋기 때문에 DISTINCT가 먼저 일어납니다.
            
        7. LIMIT
    
    ---
    
    [https://sqlbolt.com/lesson/select_queries_order_of_execution#:~:text=Because each part of the,what results are accessible where](https://sqlbolt.com/lesson/select_queries_order_of_execution#:~:text=Because%20each%20part%20of%20the,what%20results%20are%20accessible%20where).
    
## LIMIT
### LIMIT의 동작 원리는?
    
    LIMIT은 전체 쿼리를 일종의 short circuit하게 만들어준다고 할 수 있다.
    
    예를 들어 다음과 같은 쿼리가 있다고 하자.
    
    SELECT * FROM salaries ORDER BY salary LIMIT 10;
    
    이 쿼리의 실행 순서는 FROM SELECT ORDER BY LIMIT 순이다.
    
    LIMIT이 마지막에 실행되기 때문에 salary로 정렬된 모든 데이터를 다 탐색한 후에 그 중에서 10개를 반환할 것처럼 느껴진다.
    
    실제로는 ORDER BY를 수행하기 위해 salaries에서 모든 데이터를 다 탐색하긴 한다.
    
    하지만 ORDER BY에 의해서 정렬이 수행되면서 맨 위 10개의 데이터가 확인된 순간 정렬 작업을 멈추고 10개의 데이터를 반환한다.
    
    이번에는 다음과 같은 offset을 이용한 쿼리가 있다고 하자.
    
    SELECT * FROM salaries ORDER BY salary LIMIT 10000, 10;
    
    이 경우에는 10010개의 정렬된 데이터가 생길 때까지 기다린 후 ORDER BY 작업을 멈추고 앞에 10000개의 데이터를 제외하고 남은 10개의 데이터를 반환한다.
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0 2권, 2쇄, 위키북스, 78p, 2022
    
### 왜 LIMIT 200000, 10 일 때 200010개의 데이터를 찾아야 할까? 200000번째 데이터로 빠르게 찾아간 후 10개만 가져오면 안될까?
    
    LIMIT은 테이블이 최종적으로 구성되고 가장 마지막에 실행되기 때문에 불가능하다.
    
    LIMIT가 실행되기 전에 앞 작업들이 먼저 이루어져야하고 따라서 이 작업들을 만족하는 200010개의 데이터가 찾아질 때까지 기다려야 한다.