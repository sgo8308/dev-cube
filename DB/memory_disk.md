### 데이터를 디스크에 저장하는 것과 메모리에 저장하는 것의 차이는? ★
    
    메모리는 전자식 장치이며 하드디스크는 기계식 장치이다. 하지만 최근에 나오는 SSD는 전자식이다.
    
    메모리는 컴퓨터가 꺼지면 데이터가 사라지고 디스크는 남아있다.
    
    메모리는 용량이 낮고 하드디스크는 높다.
    
    메모리는 데이터 처리 속도가 하드디스크나 SSD보다 훨씬 빠르다.
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 214-, 2022
    
    [https://www.geeksforgeeks.org/difference-between-memory-and-hard-disk/](https://www.geeksforgeeks.org/difference-between-memory-and-hard-disk/)

### 요즘 DB 서버는 HDD일까 SDD일까?
    
    대부분 요즘은 SSD를 채택하고 있다.
    
    ---
    
    백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 215p, 2022
    
    - 왜 SSD를 선택할까?
        
        사실 순차 I/O는 HDD나 SDD나 속도가 비슷하다.
        
        그러나 랜덤 I/O에서 속도가 10배까지 차이난다.
        
        데이터베이스 서버에는 순차 I/O작업은 비중이 크지 않고 랜덤 I/O를 통한 작은 데이터를 읽고 쓰는 작업이 대부분이기 때문에 SDD를 사용한다.
        
        ---
        
        백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 215-216p, 2022