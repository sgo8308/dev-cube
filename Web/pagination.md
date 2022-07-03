### 

### 공부해야 하는 이유는?
    
    모든 웹사이트나 앱은 페이징이 필요하다. 잘 구현하기 위해서 또 상황에 맞는 적절한 방식을 선택하기 위해서는 잘 알아야 한다.
    
### 페이징이란??
    
    마치 책에 있는 페이지처럼 큰 데이터를 작은 부분으로 나눠 보여주는 것.
    
    영어로는 Pagination이라고 불린다.
    
    웹페이지에서 많은 정보를 보여줘야 할 때 한번에 보여주는 것이 아니라 적정한 크기로 잘라서 보여주는 기법.  페이지 방법, 더보기, 무한 스크롤 등등의 방법이 있다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c3179288-601a-491a-bf23-76ee467d12b0/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25fba6dc-0a8c-4cb8-938d-5b8109da79c1/Untitled.png)
    
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
        
### 어떤 방식으로 동작할까?
    1. offset을 이용한 방식 = 페이지 방식 (1페이지, 2페이지 …)
        
        다음과 같이 OFFSET으로 어느 부분부터 읽어드릴지 정하고 LIMIT으로 읽어들일 데이터의 수를 정한다.
        
        SELECT * FROM table 
        ORDER BY timestamp 
        OFFSET 10
        LIMIT 5
        
        그 후 GET의 쿼리 파라미터에 페이지 번호를 달아서 전달하는 방식으로 구현한다.
        
    2. cursor를 이용한 방식 = 무한 스크롤 또는 더보기 방식