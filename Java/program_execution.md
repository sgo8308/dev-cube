### cmd란 무엇일까?
    
    Command Prompt(명령 프롬프트)의 줄임말.
    
    텍스트를 이용해서 컴퓨터와 상호작용(프로그램 실행 등등)할 때 쓰는 프로그램이다.
    
### 자바 프로그램을 실행하려면 어떻게 해야 할까?
    
    cmd에서 다음 명령어를 입력하면 된다.
    
    java [options] <mainclass> [args...]
    (to execute a class)
    or  java [options] -jar <jarfile> [args...]
    (to execute a jar file)
    or  java [options] -m <module>[/<mainclass>] [args...]
    java [options] --module <module>[/<mainclass>] [args...]
    (to execute the main class in a module)
    or  java [options] <sourcefile> [args]
    (to execute a single source-file program)
    
    - jar 파일
        - 무엇일까?
            
            JAR(Java Archive, 자바 아카이브)는 여러개의 자바 클래스 파일과, 클래스들이 이용하는 관련 리소스(텍스트, 그림 등) 및 메타데이터를 하나의 파일로 압축한 파일이다.
            
        - 왜 나왔을까?
            
            자바의 애플릿과 그것에 필요한 요소들을 브라우저에서 한번의 HTTP 트랜잭션으로 보내기 위해 만들어졌다.
            
            - 애플릿이란?
                
                자바로 만들어진 리치 인터넷 애플리케이션(한 페이지로 구현된 웹 응용 프로그램)
                
        - 왜 사용할까?
            
            오디오, 이미지, 클래스 파일을 한번에 처리할 수 있는 유일한 포맷이다.
            
            압축하기 때문에 용량을 줄여준다.
            
### 왜 main 메소드는 public static void main(String[] args)이어야 할까?
    - public
        
        main 함수는 jvm이 실행시키는데 그러기 위해서는 public이어야 한다.
        
    - static
        
        main 메소드가 static이 아니라면 jvm은 main 메소드를 갖고 있는 클래스의 인스턴스를 만들어야 한다. 만약 생성자를 가지고 있다면 어떤 생성자를 호출해야 될지도 모호하며 이 클래스는 abstract이어서도 안된다.
        
        이러한 여러 불편한 점 때문에 main method는 static이다.
        
    - void
        
        main 메소드가 종료되자마자 자바 프로그램은 끝나기 때문에 무언가를 return할 필요가 없다.
        
        C나 C++같은 경우 main함수가 int를 리턴하는데, 이것은 시스템이 정상적으로 종료되었는지 아닌지를 운영체제에 알려 메모리리를 해제하는 등 적절한 처리를 하기 위함이다.
        
        그러나 자바 프로그램은 운영체제와 직접적으로 상호작용하지 않고,
        jvm 위에서 돌아가기 때문에 위와 같은 처리가 필요 없다.
        
    - String[] args
        
        커맨드라인 인터페이스에서 프로그램 시행시 인자를 전달할 때 사용하라고 만들어 놓았다. 그러나 인자를 전달하지 않는 프로그램을 위해 String[] args가 없는 main()을 허용하면, 두 타입의 메소드가 같이 있을 경우 무엇을 실행할지 모호하기도 하고, 여러 편의성을 위해 String[] args를 강제한다. args는 다른 이름이어도 상관 없다.
        
        - 어떻게 java program 실행시 아무 인자를 넘기지 않아도 실행이 잘 될 수 있나?
            
            만약 아무 인자도 없다면 jvm이 크기가 0인 String 배열을 넘겨서 실행한다.
            
    
### 컴파일이라는 것은 무엇이며 내부적으로 어떤 과정을 거칠까?
    
    컴파일은 내가 만든 프로그램 코드를 컴퓨터가 이해할 수 있도록 엮어주는 작업이다.
    
    Source code를 기계어 파일로 번역하는 과정이다.
    
    컴파일 과정
    
    1. Pre-processing
        
        컴파일을 진행하기 전에 전처리를 진행한다.
        
        ex) 주석 제거
        
    2. Tokenizing
        
        언어에 정의된 각각의 토큰을 체크하는 과정
        
        if(a >= 1) continue;
        
        라는 코드가 있을 때 토큰은 IF, IP, STR, GTR, NUM, RP, CONTINUE, SEMICOL 등이 있다.
        
    3. Parsing
        
        각 토큰들이 문법에 맞게 제대로 배치되었는지 확인하는 과정
        
        ex) if 다음에는 반드시 왼쪽 괄호가 오므로 LP(left paranthesis)가 아닌 경우 에러
        
    4. Optimization
        
        코드를 최적화하는 과정
        
        ex) For문을 100만번 도는데 안에 a=1밖에 없다면 for문을 삭제한다.
              꼬리재귀 함수는 일반적인 for문으로 만들어 준다.
        
    5. Generation Code
        
        분석된 소스 코드를 목표 기계에 맞는 어셈블리어나 기계어로 변환한다.
        
        기계어로 변환됐을 경우 오브젝트 파일이 생성된다.
        
    6. Linking
        
        목적 코드가 기계어일 경우 여러 라이브러리의 목적 코드를 묶어 하나의 실행 파일을 생성한다.
        
    
    출처 
    
    [https://www.slideshare.net/jongyoungpark2/java-class-file-format](https://www.slideshare.net/jongyoungpark2/java-class-file-format)
    위키피디아 : 컴파일러
    [https://bradbury.tistory.com/226](https://bradbury.tistory.com/226)