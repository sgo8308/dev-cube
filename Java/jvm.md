# JVM 구조
### JVM 구조는 어떻게 이루어져 있을까?
### JVM Stack이란?
    
    JVM Stack은 쓰레드마다 할당된 Stack 자료 구조를 갖는 공간이다.
    
    쓰레드에서 어떤 메소드를 호출하며 Stack에 하나의 프레임이 생긴다.
    
    그 프레임 안에서 다른 메소드 호출시 이 프레임 위로 또 하나의 프레임이 쌓인다.
    
    - 프레임은 어떻게 구성되어 있을까?

        - Local Variable Array
            
            말 그대로 지역 변수가 저장되는 배열이다.
            
            호출된 메소드에서 사용하는 지역변수들이 저장된다.
            
            인스턴스 메소드의 경우 클래스 자신을 가르키는 this가 변수에 포함되는데,
            이 this는 지역 변수 배열의 가장 첫번째 인자로 등록된다.
            
            JVM의 Word Size인 4byte를 넘는 long과 double을 제외하면,
            모두 이 지역 변수 배열에 한 칸씩을 차지한다.
            
        - Operand Stack
            
            피연산자가 저장되는 스택이다.
            
            보통 어떠한 연산을 하기 전에 이 스택에다가 저장을 먼저하고
            
            연산을 수행하면서 피연산자가 필요할 때 pop을 하여 사용한다.
            
        - Constant Pool Reference
            
            클래스마다 할당되는 상수 풀에 대한 참조다.
            
            어떤 명령어는 상수 풀 안에 값에 대해서 수행되는데 이 때 이 참조를 이용한다.
### MetaSpace란?
        
    자바 8부터 Permanent Generation을 대체한 영역이며 클래스의 메타 데이터가 저장된다.
        
    자바의 힙과 인접하지 않으며 네이티브 메모리에 할당된다.
---
        
참조 문헌
        
- 웹 문서 [https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf), 27p
### code cache란 무엇인가?
    
    JIT 컴파일러에 의해 컴파일된 코드를 보관하는 영역이며 네이티브 메모리에 할당된다.
---
    
참조 문헌
    
- 웹 문서 [https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf), 31p
### 네이티브 메모리란?
    
    이용가능한 시스템 메모리. 
    
    JVM의 메모리 관리를 받지 않는다. 
---
    
참조 문헌
    
- 웹 문서 [https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf), 32p

# JVM Instruction Set

### JVM Instruction Set은 뭘까?
    
    Instruction Set이란 CPU가 인식해서 기능을 이해하고 실행할 수 있는 기계어 명령어들의 집합을 말한다.
    
    JVM은 가상의 컴퓨터이고 JVM Instruction Set이란
    JVM이 알아듣는 명령어어들의 집합이라고 할 수 있다.
    
    자바 프로그램은 자바 컴파일러가 컴파일 한 후 바이트코드로 변환되고,
    이 바이트코드는 JVM Instruction Seㅌㅌㅌt의 명령어들과 그 명령어에 전달하는 피연산자 등으로 이루어져 있다.
    
    자바 프로그램의 어셈블리 언어라고도 할 수 있다.
    
### aload, iload, lload, fload, dload는 무슨 의미일까?
    
    앞에 붙는 첫 글자는 자료형을 의미하고 뒤에 붙는 load는 Local Variable Array로부터
    Operand Stack에 쌓는다는 의미다.
    
    a는 오브젝트의 레퍼런스, i는 int, l은 long, f는 float, d는 double이다.
    
    즉 iload는 int값을 Operand Stack에 쌓는다는 의미이다. 

# 성능 튜닝
### JVM의 성능 튜닝은 주로 어떤 부분에서 일어날까?
    
    대부분의 성능 튜닝은 힙의 사이즈를 결정하거나 상황에 맞는 적절한 가비지 컬렉터를 선택하는데 있다. 
    
    JIT Compiler도 성능에 중요한 영향을 끼치지만 최신의 JVM에서는 거의 튜닝을 필요로 하지 않는다.

--- 
참조 문헌
    
- 웹 문서 [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
