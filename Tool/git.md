### BASIC

### 정의는?
    
    base를 바꿔준 행위
    
    base란 두 개의 브랜치가 있을 때 두 브랜치가 갈리기 시작한 커밋을 말한다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a6f7a04-a06d-4d94-b4d0-e0570e560f96/Untitled.png)
    
    위와 같은 브랜치가 있을 때 C가 바로 base이다.
    
    M2 브랜치를 T2 브랜치로 rebase한다는 의미는 M2의 브랜치인 C를 T2로 바꾸어준다는 의미이다.
    
    rebase를 하고나면 다음과 같이 된다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/24c5940b-83dd-4883-a0cc-bf5d0f8b5f01/Untitled.png)
    
    ---
    
    [https://opentutorials.org/course/3843/24444](https://opentutorials.org/course/3843/24444)
    
### 사용해야 하는 이유는?
    
    여러가지 브랜치로 나뉘어서 작업된 것이 마치 하나의 브랜치에서 작업된 것 처럼 만들어주기 때문에 Git 커밋 히스토리를 깔끔하게 만들 수 있다.
    
    ---
    
    [https://opentutorials.org/course/3843/24444](https://opentutorials.org/course/3843/24444)
    
### 어떤 방식으로 동작할까?
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a6f7a04-a06d-4d94-b4d0-e0570e560f96/Untitled.png)
    
    다음과 같은 두 브랜치가 있고 M 브랜치 T 브랜치라고 하자.
    
    M에서 T로 Rebase를 진행한다.
    
    먼저 T2 커밋과 M1 커밋을 합친 L1 커밋이 새로 생성되고 다시 L1 커밋과 M2 커밋이 합쳐서 L2 커밋이 생성된다.
    
    최종 log는 다음과 같이 된다.
    
    A → B → C → T1 → T2 → L1 → L2 (L1의 변경사항은 M1과 같고 , L2의 변경사항은 M2와 같다.)
    
    ---
    
    [https://opentutorials.org/course/3843/24444](https://opentutorials.org/course/3843/24444)
    
### 잘 사용하려면 어떻게 해야할까?
    
    아직 push 하지 않은 커밋들에 대해서 만 rebase를 적용한다. 그렇지 않고 push한 커밋들에 대해서 rebase를 적용하면 엉망진창이 될 수 있다. 
    
    ---
    
    [https://opentutorials.org/course/3843/24444](https://opentutorials.org/course/3843/24444)
    
### 단점은?
    
    위험하다..