### 예외는 어떤 종류가 있을까? ★
    1. error
        
        자바 프로그램 밖에서 발생한 예외를 말한다.예를 들어 서버의 디스크가 고장 났다던지, 메인보드가 맛이 가능 등 자바 프로그램이 제대로 동작하지 못하는 경우이다.
        
        Error는 발생하면 복구가 불가능하다.
        
    2. checked exception
        
        error와 unchecked exception이 아닌 것은 모두 checked exception이다.
        
        Checked Exception 그 코드에서 개발자가 아무리 용을 써도 사용자가 잘못하면 예외가
        
        나올 수 밖에 없을 때 미리 예외를 체크하는 것이다.
        
    3. uncheck exception 또는 runtime exception
        
        컴파일시에 체크하지 않기 때문에 uncheck exception이라고 불리며 런타임 도중에 예외가 발생하기 때문에 runtime exception이라고도 한다. 
        
        UnChecked Exception은 개발자가 잘 신경만 쓰면 절대 예외가 안 나오게 할 수 있기 때문에 굳이 체크를 할 필요가 없다.
        
        UnChecked Exception도 마찬가지로 try catch로 잡아서 처리해 줄 수 있지만,
        
        모든 UnChecked Exception에 이 처리를 하게 되면 코드가 너무 지저분해진다.
        
        try catch를 쓰는 시간에 코드를 째려보고 예외 터질 부분을 수정해주는게 훨씬 낫다.
        
### Error와 Exception을 가르는 가장 큰 차이는?
    
    프로그램 안에서 발생했는지, 밖에서 발생했는지 여부.
    
    하지만 더 큰 차이는 프로그램이 멈추어 버리느냐 계속 실행할 수 있느냐의 차이다.
    
    정확하게 말하면 Error는 프로세스에 영향을 주고 Exception은 쓰레드에만 영향을 준다.
    
    ---
    
    이상민, 자바의 신, 2판 1쇄, 로드북, 357p, 2017
    
    - 쓰레드가 죽으면 다른 쓰레드에 영향이 있을가 없을까?
### try-catch-finally문에서 catch블록에 return 키워드를 사용하면 finally구문을 실행될까요? ★
    
    그래도 finally 구문이 실행된다.
    
    finally는 이외에도 catch안에서 continue나 break가 발생한 경우에도 실행된다.
    
    finally는 System.exit() method와 같이 jvm이 종료될 때나 jvm crash , power failure, software cash 등등의 경우를 제외하면 모두 실행된다.
    
    - 어떤 식으로 이게 가능한걸까?
        
        catch문 안에 return문이 있을 경우 return 문 바로 오른쪽에 명령까지 실행되고
        (ex 만약 catch문이 return getInt(); 로 끝나면 getInt()까지만 실행된 후 이 값을 저장해놓음)
        
        finally구문의 내용을 실행한 후 마지막에 return이 된다. 이 때 저장해놓은 return value를 리턴한다. 
        
        ---
        
        [https://blog.jamesdbloom.com/JavaCodeToByteCode_PartOne.html#while_loop]
        
        직접 바이트코드를 분석
        
    
    ---
    
    [https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html]
    
    [https://www.tutorialspoint.com/will-a-finally-block-execute-after-a-return-statement-in-a-method-in-java]
    
### 어차피 try catch 아래에 코드를 쓰면 되는 finally가 필요한 이유가 뭘까?
    
    앞에서 return이 되더라도 코드가 진행되기 때문에
    예외가 예상되지 않을 때에도 resource를 해제해주는 cleanup메소드 같은 것들을 finally에 위치해주면 좋다.
    
    ---
    
    [https://docs.oracle.com/javase/tutorial/essential/exceptions/finally.html]
    
### finally문은 언제 실행될까?
    1. try문에서 예외가 발생하지 않았을 때
    2. try문에서 예외가 발생하고 catch문으로 처리한 후
    3. try문에서 예외가 발생했지만 catch문에서 핸들링하지 못하는 예외일 경우 finally구문이 모두 실행되고 이 예외를 밖으로 던진다.
    
    ---
    
    [https://blog.jamesdbloom.com/JavaCodeToByteCode_PartOne.html#while_loop]
    
### catch문 안에서 다시 한 번 예외가 발생한 것도 같은 지금 있는 catch문으로 잡힐까?
    
    그렇지 않다. 이 예외는 다시 한 번 try catch로 감싸서 처리해야 한다.
    
    이 때 예외 처리를 하지 못하더라도 finally구문은 실행된다.
    
### 다음과 같은 코드가 있을 때 어떤 예외가 발생할까?
    
    int[] intArray = new int[5];
    intArray = null;
    System.out.println(intArray[5]);
    
    NullPointerException 예외가 발생한다.
    
    왜냐하면 null인 객체를 갖고 작업하면 안 되기 때문에 해당 객체가 null인지 확인하는 작업이 반드시 먼저 선행되어야 하기 때문이다.
    
    ---
    
    이상민, 자바의 신, 2판 1쇄, 로드북, 354p, 2017
    
### 다음과 같은 코드는 컴파일이 될까?
    
    public void multiCatchOrderChange(){
    	int[] intArray = new int[5];
    	try{
    		System.out.println(intArra[5]);
    	} catch(Exception e){
    	
    	} catch(ArrayIndexOutOfBoundsException e){
    		
    	}
    }
    
    컴파일이 되지 않는다.
    
    왜냐하면 첫번째 catch문에서 모든 exception을 다 잡아버리기 때문에 두번째 catch문은 필요가 없기 때문이다.
    
    ---
    
    이상민, 자바의 신, 2판 1쇄, 로드북, 352p, 2017
    
    - 하지만 문제 생기는 것도 아닌데 굳이 컴파일이 안되야하는 이유가 있을까?
### Throwable 생성자에 정의되어 있는 cause는 뭘까?
    
    cause는 이 예외의 원인이 된 예외이다.
    
    예를 들어서 예외를 체이닝하는 경우가 있다.
    
    가장 low한 level의 예외 (ex ArrayOutOfBoundException)를
    좀 더 High level(도메인 특화적인 ex) NonValidMoneyException) 예외로 연결시키는 것이다.
    
    이 때 low한 level의 예외는high level 예외의 원인인 셈이다.
    
    이런 상황에서 이 원인 예외를 지금 발생시키는 예외에 등록시켜주면 StackTrace에서 Caused by 문장과 함께 출력이 가능하다.

### 예외를 처리하는 안 좋은 방법들은 어떤 것들이 있고 어떤 방식이 좋은 방식일까?
    
    #협업 #귀찮음 #의존
    
    예외는 try catch로 잡거나 밖으로 던질 수 있다. 둘 다 **귀찮음**으로 인한 문제가 발생할 수 있다. 
    
    1. try catch로 잡았는데 아무런 처리를 안하고 먹어버리기
        
        이런 식으로 처리할 경우 문제 상황이 해결되지 않은 채 지속되며 결국에는 장애로 이어지는데 더 큰 문제는 무엇이 원인인지 파악하기 힘들다. 
        
    2. throws Exception으로 모두 던져버리기
        
        이렇게 throws Exception이 있는 메소드를 사용하는 다른 개발자는 이 메소드에서 어떤 문제가 발생하는지 알기 어렵고 따라서 마찬가지로 throws Exception을 해줄 수 밖에 없다. 
        만약 처리가능한 예외였다면 적절한 시점에 처리되지 못한 채 밖으로 던져지는 것이다.
        
        throws InvalidKeySpecException과 같이 구체적인 예외를 던지더라도 이 예외가 특정한 구현 방식과 관련 있는 예외라면 문제가 있다.
        이 예외를 잡아서 처리하는 코드는 그 아래 구현 방식이 변경되면 더 이상 쓸 수 없는 코드가 되기 때문이다. 예를 들어 위 Exception은 PBKDF방식의 암호화하는 코드에서 나오는 예외다. 
        만약 다른 방식의 암호화 방식을 쓴다면 InvalidKeySpecException을 잡아서 예외 처리하는 코드는 쓸모없어진다.
        
    
    따라서 위와 같은 방식으로 예외를 처리하는 것은 피해야 한다.
    
    대신에 예외를 처리할 수 있다면 빠르게 처리하고 처리할 수 없다면 예외를 전환하는 방법을 생각해볼 수 있다.
    
    예외 전환 방법
    
    1. 더 적절하고 추상화된 이름을 가진 예외로 만들어서 밖으로 던진다.
        
        밖에 코드에서 이 예외를 잡아서 적절한 처리를 해줄 수 있다.
        
    2. 어디에서도 처리하기 힘든 예외라면 RuntimeException으로 감싸거나 RuntimeException을 상속한 예외로 감싸서 던지기
        
        이렇게 할 경우 바깥 코드들(service, controller)에서 쓸데없이 throws ㅁㅁㅁ와 같은 식으로 예외를 던져줄 필요도 없고,
        이런 예외들에 의존하게 될 필요도 없으므로 깔끔하고 좋은 방법이다. 
        이 후 예외처리를 하는 담당하는 곳에서 자세한 로그를 남기고 관리자에게 통보하고 사용자에게 친절한 안내 메시지로 처리하면 좋다.
        
        ---
        
        토비의 스프링 4장

### 애플리케이션에서 발생한 예외, 예를 들면 계좌 이체를 하는데 잔고 부족 등을 처리하는 방법은 어떤게 있을까?
    
    #협업 #귀찮음
    
    1. 계좌 이체에 성공한 경우 1 실패한 경우 0과 같은 식으로 해서 처리할 수 있다.
        
        이렇게 할 경우 문제는 개발자들 사이에 이러한 규약이 공통적으로 정의되어 있지 않다면 혼동이될 여지가 많고 불필요한 if문이 남발되어 **가독성**이 떨어질 수 있다.
        
    2. InsufficientBalanceException과 같은 적절한 이름의 예외를 만들어 던져준다. 
        
        이렇게 할 경우 무슨 문제가 발생했는지 바로 알 수 있고 코드 또한 정상적인 로직은  try안에서 예외 발생할 경우는 catch 안에서로 나뉘게 되어 **가독성**이 더 좋아진다.
        
    
    ---
    
    토비의 스프링 4장