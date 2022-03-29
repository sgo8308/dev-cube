# JVM 구조
### JVM 구조는 어떻게 이루어져 있을까?
    JVM은 세 파트로 이루어져 있다.
    
    Class Loader Subsystem, Runtime Data Areas, Execution Engine
    
    Runtime Data Areas는 다시 Method Area, Heap, Java Threads, Program Counter Register, Native Internal Threads로 이루어져 있고

    Execution Engine은 JIT Compiler와 Garbage Collector 그리고 interpretor로 이루어져 있다.
---

참조 문헌

- 웹 문서
    
    [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
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
### Method Area란?
    일반적인 프로세스 구조에서 컴파일된 코드가 저장되는 test 영역과 비슷한 곳이다.
    
    각 클래스별로 runtime costant pool, field, method data, method code, constructor, special methods(ex <init>) 등이 저장된다.
    
    논리적으로 힙 영역에 속하지만 간단한 구현으로 가비지 콜렉팅과 압축의 대상에서 제외 시킬 수 있다. 하지만 메소드 영역의 위치는 강제되지는 않는다.

---

참조 자료

[https://docs.oracle.com/javase/specs/jvms/se17/html/jvms-2.html#jvms-2.5.4](https://docs.oracle.com/javase/specs/jvms/se17/html/jvms-2.html#jvms-2.5.4)

### PermGen이란?
    
    힙 영역은 아니지만 Full GC의 대상이 된다.
    
    Class의 메타 정보, Method의 메타 정보, Static 변수와 상수 정보들이 저장된다.
    
    이 영역은 Java8부터 Native영역으로 이동하여 Metaspace 영역으로 변경되었다.
    
    너무 많은 수의 Class Object가 로딩될 때 주로 꽉차게 되고 Full GC가 발생하게 된다.
    
    - Class의 메타 정보란 무엇인가?
        
        모든 클래스는 Object 클래스를 상속한다. 
        
        Object 클래스에는 getClass()라는 메소드가 정의되어 있는데,
        이 메소드를 통하여 Class라는 클래스를 얻을 수 있다.(다른 방식도 있다)
        
        예를 들면 이런식이다.
        
        A a = new A();
        Class aClass = a.getClass();
        
        이 Class 클래스를 통해서 클래스에 정의되어 있는 변수나 메소드 어노테이션 등의 메타 정보를 얻을 수 있다.
        
        이러한 Class 클래스는 클래스가 로드될 때 할당되며, 위와 같이 Class 객체를 만들 때도 할당된다.
      ---
        
      참고자료
        
      [https://wiki.openjdk.java.net/display/HotSpot/Metaspace]

      이상민, 자바 성능 튜닝 이야기, 초판 4쇄, 인사이트, 132p, 2019
---
    
참조 자료
    
류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 21p,2017
    
류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 125p ,2017
 ### PermGen은 왜 없어졌을까? ★
    
    JRockit JVM과 Hotspot JVM에 합치는 노력의 일환이다. JRockit은 PermGen이 존재하지 않아 관리가 필요없기 때문이다.
    
    또한 PermGen은 JVM에 의해 크기가 강제되던 영역으로 Perm 영역 크기로 인한 OutOfMemoryError가 자주 일어났다.
    
    이러한 이유로 JDK 8부터는 PermGen이 삭제되고 대신 Metaspace 영역이 추가되었다.
    
    Metaspace는 Native memory 영역으로 , OS가 자동으로 크기를 조절하고 기존 Perm영역의 최대 사이즈보다 훨씬 큰 사이즈를 갖는다.
    
    ---
    
    [https://johngrib.github.io/wiki/java8-why-permgen-removed/]
    
    [https://openjdk.java.net/jeps/122]
    
    - JRockit이란?
        
        WebLogic이라는 WAS의 성능 최적화를 위해서 만들어진 JDK이다.
        
        이 JDK를 만든 회사가 Oracle에 넘어갔기 때문에 Oracle은 Oracle JDK와 JRockit JDK 2개를 제공하는 셈이다.
        
        현재는 Oracle JDK에 합쳐진 듯 하다.
        
        ---
        
        이상민, 자바의 신, 2판 1쇄, 로드북, 537p, 2017
        
        [https://stackoverflow.com/questions/23578099/where-to-download-jrockit-for-java-7]
### MetaSpace란?
    
    자바 8부터 Permanent Generation을 대체한 영역이다.
    
    Permanent Generation과 같이 클래스의 메타 정보와 Method의 메타 정보를 저장하는 것은 동일하다. 또 Full GC의 수행 대상이 되는 것도 동일하다.
    
    반면에 기존에 Permgen영역에 저장되던 Static Object 변수와 상수가 heap 영역으로 이동하여 Metaspace에는 존재하지 않게 되었다.
    
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
### 왜 heap은 Generation 구조로 만들었을까?
    
    모든 객체가 쓰레기인지 검사하는 무식한 방식의 가비지 컬렉션은 규모가 큰 프로그램에서 심각한 문제가 생길 수 있다.
    
    JVM GC 설계자들은 경험적으로 대부분의 객체가 생겨나자마자 쓰레기가 된다는 것을 알고 있었다. (이것을 '약한 세대 가설(weak generational hypothesis)'이라 부른다.)
    
    따라서 매번 전체를 검사하지 않고 일부만 검사할 수 있도록 generational한 구조를 고안해 낸 것이다.
    
    이러한 방식 속에서 대부분의 객체는 Young Generation에서 죽게 된다.
    
    ---
    
    [https://johngrib.github.io/wiki/jvm-memory/]
    
### 왜 jvm spec은 Metaspace나 PermGen에 대한 설명이 전혀 없을까?
    
    Metaspace나 PermGen은 hotspot jvm의 세부 구현 사항이기 때문이다.
    
    ---
    (https://stackoverflow.com/questions/49371219/why-oracle-specification-does-not-tell-anything-about-metaspace)
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
### invokevirtual은 무슨뜻일까?
    
    c++에서 어떤 메소드가 오버라이드되려면 virtual로 선언되어야 한다.
    
    자바는 이 부분에서 영감을 얻은 것 같다.
    
    자바는 기본적으로 static과 final 메소드를 제외한 모든 메소드가 오버라이딩 가능하니 기본적으로 virtual 메소드인 셈이다.
    
    ---
    
    [https://dzone.com/articles/how-does-jvm-handle-polymorphism-method-overloadin](https://dzone.com/articles/how-does-jvm-handle-polymorphism-method-overloadin)

# 성능 튜닝
### JVM의 성능 튜닝은 주로 어떤 부분에서 일어날까?
    
    대부분의 성능 튜닝은 힙의 사이즈를 결정하거나 상황에 맞는 적절한 가비지 컬렉터를 선택하는데 있다. 
    
    JIT Compiler도 성능에 중요한 영향을 끼치지만 최신의 JVM에서는 거의 튜닝을 필요로 하지 않는다.

--- 
참조 문헌
    
- 웹 문서 [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)
### 성능에 대한 주요 척도는 Throughput과 Responsiveness인데 둘을 무엇을 의미할까?
    - Throughput
        
        Throughput은 처리량이라는 의미로 얼마나 많은 양의 일을 특정한 기간동안 하느냐이다.
        
        예를 들어 이런 것이다.
        
        주어진 시간동안 완료된 트랜잭션 갯수
        
        배치프로그램이 한시간안에 완요할 수 있는 일의 갯수
        
        한시간안에 완료될 수 잇는 쿼리의 갯수
        
        긴 시간의 pause time은 throughput에 집중하는 앱에는 받아들여질만 하다.
        왜냐하면 이런 어플리케이션은 긴 기간 동안의 벤치마크에 집중하기 때문이다.
        빠른 반응 속도는 고려 대상이 아니다.
        
        이런 앱들은 Serial GC나 Parellel GC를 이용하는 것이 좋다.
        
    - Responsiveness
        
        얼마나 빨리 애플리케이션이나 시스템이 특정한 요청에 반응하는가이다.
        
        예를 들면 이런 것이다.
        
        얼마나 빨리 데스크탑 UI가 이벤트에 반응하는가
        
        얼마나 빨리 웹사이트가 페이지를 리턴하는가
        
        얼마나 빨리 쿼리가 리턴되는가
        
        Responsiveness에 집중하는 애플리케이션에게 긴 시간의 pause타임은 용납이 안된다.
        
        이런 앱들은 Concurrent GC들을 이용하는게 좋다.
        
        
        ---
        
        [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html]