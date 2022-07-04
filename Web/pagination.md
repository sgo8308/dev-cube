### 공부해야 하는 이유는?
    
    모든 웹사이트나 앱은 페이징이 필요하다. 잘 구현하기 위해서 또 상황에 맞는 적절한 방식을 선택하기 위해서는 잘 알아야 한다.
    
### 페이징이란??
    
    마치 책에 있는 페이지처럼 큰 데이터를 작은 부분으로 나눠 보여주는 것.
    
    영어로는 Pagination이라고 불린다.
    
    웹페이지에서 많은 정보를 보여줘야 할 때 한번에 보여주는 것이 아니라 적정한 크기로 잘라서 보여주는 기법.  페이지 방법, 더보기, 무한 스크롤 등등의 방법이 있다.
    
    ---
    
    [https://www.seoptimer.com/blog/what-is-pagination/#:~:text=Here are examples of sites where pagination is used%3A](https://www.seoptimer.com/blog/what-is-pagination/#:~:text=Here%20are%20examples%20of%20sites%20where%20pagination%20is%20used%3A)
    
### 사용해야 하는 이유는?
    
    하나의 페이지에 보여주기 힘들 정도로 많은 데이터가 있을 때가 있다.
    
    이 때 페이징 기법을 쓰면 다음과 같은 장점이 있다.
    
    1. 사용자 경험 향상
        
        아마존의 모든 상품이 한 페이지에 있으면 사용자 경험이 어떨지 상상해보자.
        
        특정 상품의 위치를 기억하기도 힘들다.
        
    2. 성능 향상
        
        한번에 모든 데이터를 다 가져와서 보여주려면 꽤 오래 걸릴 것이다. 페이지로 잘라서 가져오면 그만큼 빠르게 페이지를 로딩할 수 있다.

### 어떤 방식으로 구현해야할까?
    1. offset을 이용한 방식 = 페이지 방식 (1페이지, 2페이지 …)

       다음과 같이 OFFSET으로 어느 부분부터 읽어드릴지 정하고 LIMIT으로 읽어들일 데이터의 수를 정한다.

        SELECT * FROM table 
        ORDER BY timestamp 
        OFFSET 10
        LIMIT 5

       그 후 GET의 쿼리파라미터에 페이지 번호를 달아서 전달하는 방식으로 구현한다.

       만약 1페이지라면 offset 0부터 10개 2페이지라면 offset 10부터 10개 이런 식이다.

    2. offset을 이용하지 않은 방식 = 무한 스크롤 또는 더보기 방식

       No Offset 방식은 조회 시작 부분을 인덱스로 빠르게 찾아 매번 첫 페이지만 읽도록 하는 방식이다.

        1. 첫 페이지를 요청한 후 첫 페이지의 마지막 레코드를 기억한다.
        2. 두번째 페이지를 요청할 때는 마지막 레코드의 정보를 WHERE 조건문에 넣고 두번째 페이지의 시작 부분을 바로 찾아낸다. (인덱스를 이용해서)
        3. limit을 이용해 페이지 크기만큼 읽는다.

       예를 들어 첫페이지 쿼리를 다음과 같이 했다고 가정하자.

        테이블 = salaries  ,  인덱스 = salary
        
        SELECT * FROM salaries ORDER BY salary LIMIT 10;

       이 때 나온 마지막 레코드가 다음과 같다고 하면

        emp_no   salary   from_date   to_date
        27       39974    1996-09-01  1997-09-01

       이 마지막 레코드의 정보를 두번째 페이지를 요청할 때 같이 넘긴다.

       두번째 쿼리는 다음과 같다.

        SELECT * FROM salaries WHERE salary > 39974 ORDER BY salary LIMIT 10;

       이렇게 구현할 경우 다음 페이지의 시작 위치를 인덱스를 이용해 빠르게 찾아내고  매번 offset 0부터 시작하는 효과가 있다.

        하지만 위 두번째 쿼리는 39974원의 salary를 가진 레코드가 많을 경우 누락되는 레코드가 있을 수 있다. 
        예를 들어 39974원의 레코드는 5개 있는데 이 중 2개만 첫페이지에 포함되어 있다면
        두번째 페이지에는 나머지 3개가 들어가야 하지만 위 쿼리는 이 3개를 누락시킨다.

       따라서 정확히 모든 레코드를 누락없이 첫 페이지와 중복 없이 얻으려면 다음과 같이 쿼리해야 한다.

        SELECT * FROM salaries
        WHERE salary >= 39974 AND NOT (salary=38864 AND emp_no <= 27)
        ORDER BY salary LIMIT 0, 10;

       이런 식으로 3번째부터 마지막 페이지까지 모두 동일하게 페이징할 수 있다.
        
       ---

       백은빈, 이성욱, Real MySQL 8.0 2권, 2쇄, 위키북스, 80p, 2022

       [https://jojoldu.tistory.com/528](https://jojoldu.tistory.com/528#:~:text=%EC%9D%B4%EC%99%80%20%EA%B0%99%EC%9D%80%20%ED%98%95%ED%83%9C%EC%9D%98%20%ED%8E%98%EC%9D%B4%EC%A7%95,%EB%8B%A4%EC%8B%9C%20%EC%9D%BD%EC%96%B4%EC%95%BC%20%ED%95%98%EA%B8%B0%20%EB%95%8C%EB%AC%B8%EC%9D%B8%EB%8D%B0%EC%9A%94.&text=%EA%B7%B8%EB%A6%AC%EA%B3%A0%20%EC%9D%B4%20%EC%A4%91%20%EC%95%9E%EC%9D%98%2010%2C000%20%EA%B0%9C%20%ED%96%89%EC%9D%84%20%EB%B2%84%EB%A6%AC%EA%B2%8C%20%EB%90%A9%EB%8B%88%EB%8B%A4.&text=%EB%92%A4%EB%A1%9C%20%EA%B0%88%EC%88%98%EB%A1%9D%20%EB%B2%84%EB%A6%AC%EC%A7%80%EB%A7%8C%20%EC%9D%BD%EC%96%B4%EC%95%BC,%EA%B0%88%EC%88%98%EB%A1%9D%20%EB%8A%90%EB%A0%A4%EC%A7%80%EB%8A%94%20%EA%B2%83%EC%9E%85%EB%8B%88%EB%8B%A4)

### 페이징 할 때 뒷페이지로 갈수록 느려지는 현상은 왜 나타나고 어떻게 해결할까?
    
    이 현상은 offset 방식을 사용할 때 나타난다. 만약 offset 10000 limit 20이라고 먼저 테이블의 처음부터 10020개의 행을 읽은 후 앞에 10000개를 버리고 20개를 남긴다.
    따라서 뒤로 갈수록 버리지만 읽어야 할 행의 개수가 많아지기 때문에 느려진다.
    
    해결 방식
    
    1. No Offset 방식을 이용하여 Infinite Scroll로 바꾼다면이 문제를 해결할 수 있다.
    2. 여전히 Pagination(1페이지, 2페이지) 방식을 사용해야 한다면 커버드 인덱스 방식을 사용하여 성능을 개선할 수 있다.
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0 2권, 2쇄, 위키북스, 80p, 2022
    
    [https://jojoldu.tistory.com/528](https://jojoldu.tistory.com/528#:~:text=%EC%9D%B4%EC%99%80%20%EA%B0%99%EC%9D%80%20%ED%98%95%ED%83%9C%EC%9D%98%20%ED%8E%98%EC%9D%B4%EC%A7%95,%EB%8B%A4%EC%8B%9C%20%EC%9D%BD%EC%96%B4%EC%95%BC%20%ED%95%98%EA%B8%B0%20%EB%95%8C%EB%AC%B8%EC%9D%B8%EB%8D%B0%EC%9A%94.&text=%EA%B7%B8%EB%A6%AC%EA%B3%A0%20%EC%9D%B4%20%EC%A4%91%20%EC%95%9E%EC%9D%98%2010%2C000%20%EA%B0%9C%20%ED%96%89%EC%9D%84%20%EB%B2%84%EB%A6%AC%EA%B2%8C%20%EB%90%A9%EB%8B%88%EB%8B%A4.&text=%EB%92%A4%EB%A1%9C%20%EA%B0%88%EC%88%98%EB%A1%9D%20%EB%B2%84%EB%A6%AC%EC%A7%80%EB%A7%8C%20%EC%9D%BD%EC%96%B4%EC%95%BC,%EA%B0%88%EC%88%98%EB%A1%9D%20%EB%8A%90%EB%A0%A4%EC%A7%80%EB%8A%94%20%EA%B2%83%EC%9E%85%EB%8B%88%EB%8B%A4)
    
### 커버드 인덱스 방식을 이용해서 어떻게 Pagination(offset 방식)의 성능을 개선할까?
    
    LIMIT은 가장 마지막에 실행된다.
    
    LIMIT 10000, 10이라면 기본적으로 LIMIT 앞에 작업들(WHERE, ORDER BY 등)이 적용된 10010개의 데이터가 찾아질 때까지 기다려야 한다.
    
    이 때 앞에 작업들을 인덱스만을 이용해서 처리할 수 있다면, 즉 커버드 인덱스를 사용한다면 상당히 빠른 시간 안에 10010개의 데이터를 찾아낼 수 있다. 
    
    따라서 offset이 증가할 때 느려지는 성능을 어느정도 완화시켜줄 수 있다.
    
    단점
    
    1. 너무 많은 인덱스가 필요하다.
    2. 인덱스의 크기가 커진다.
    3. 기본적을 10010개의 데이터를 찾는 것은 같기 때문에 offset이 커질수록 느려진다.
    
    ---
    
    [참고](https://jojoldu.tistory.com/529?category=637935)
    
### Pagination vs Infinite Scroll 언제 어떤 방식의 페이징 기법을 써야할까?
    
    두 방식은 사용성에 있어서 큰 차이를 보이기 때문에 사용성을 기준으로 선택해야 한다.
    
    Pagination 방식은 1페이지에서 다른 페이지로 쉽게 넘어갈 수 있고 내가 읽던 페이지로 돌아오기가 쉽다. 
    
    Infinite Scroll 방식은 다음 페이지로만 넘어갈 수 있다. 한참 읽다가 중간에 다른데 갔다가 돌아오면 원래 읽던 부분을 찾을 수가 없고 다시 한참 아래로 내려야 한다. 
    
    Pagination 방식은 계속 다음 페이지를 보기 위해 링크로 마우스를 올리고 클릭해야 한다. 이 방식이 불편하다.
    
    Infinit Scroll 방식은 스크롤을 이용해서 계속 새로운 컨텐츠를 볼 수 있기 때문에 흐름이 끊기지 않고 사용자 경험이 향상된다.
    
    따라서 Pagination 방식은 사용자들이 특정한 목적을 갖고 사이트를 탐색하는 이커머스나 검색 사이트에 좋고, 
    Infinite Scroll 방식은 매우 많은 데이터가 있고 사람들이 특정한 목적을 갖고 탐색하는게 아닌 경우에 좋다. 
    예를 들어 페이스북, 핀터레스트, 인스타그램 등이 있다.
    
    ---
    
    [https://uxplanet.org/ux-infinite-scrolling-vs-pagination-1030d29376f1](https://uxplanet.org/ux-infinite-scrolling-vs-pagination-1030d29376f1)