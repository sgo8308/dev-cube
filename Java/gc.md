# Gc
### GC란?
    
    더 이상 참조되지 않아 접근이 불가능한 메모리를 Garbage라고 부르는데 이 메모리를 회수하는 것을 Garbage Collection이라고 부른다.

    GC는 보통 Garbage Collector에 의해 자동으로 이루어진다.
    
    가장 최신 버전인 java 18 기준으로 자바에서 사용되는 GC의 종류로는 Serial GC, Parallel GC, G1GC, Z GC가 있고 가장 많이 쓰이는 java 8 기준으로는 Serial GC, Parallel GC, CMS GC,  G1GC가 있다.
    
    ---
    
    [https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))
    
    [https://docs.oracle.com/en/java/javase/18/gctuning/available-collectors.html#GUID-F215A508-9E58-40B4-90A5-74E29BF3BD3C](https://docs.oracle.com/en/java/javase/18/gctuning/available-collectors.html#GUID-F215A508-9E58-40B4-90A5-74E29BF3BD3C)
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html#sthref27](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html#sthref27)
### Reachable Object는 어떻게 판별할까?
    
    Root로 부터 어떤 식으로든 Reference 관계가 있다면 Reachable Object라고 한다.
    
    구체적으로 말하면,
    
    Loacl variable Section, Operand Stack에 Object의 Reference 정보가 있다면 Reachable Object이다.
    
    Method Area에 로딩된 클래스 중 constant pool에 있는 Reference 정보를 토대로
    Thread에서 직접 참조하진 않지만 constant pool을 통해 간접 link를 하고 있는 Object는 Reachable Object이다.
    
    아직 Memory에 남아 있으며 Native Method Area로 넘겨진 Object의 Reference가 JNI 형태로 참조 관계가 있는 Object는 Reachable Object이다.
    
    ---
    
    류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 26p,2017
    
    - Root란?
        
        가비지 컬렉터를 위한 특별한 오브젝트이다. 
        가비지 컬렉터는 Root로 부터 방문되어진 모든 객체들을 살아있는 객체로 마킹하고 그렇지 않은 객체들을 가비지로 판정한다.
        
        Root는 예를 들어 static 변수에 대한 참조를 가지고 있는 로딩된 클래스, 지역 변수나, Active한 모든 자바 쓰레드들 등이 있다.
        
        ---
        
        [https://www.baeldung.com/java-gc-roots](https://www.baeldung.com/java-gc-roots)
### GC 과정에서 발생하는 stop the world라는 것은 무엇일까?


    GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것이다.
    
    stop the world가 발생하면 GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다.
    
    ---
    
    [https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)

### 왜 GC는 stop the world가 필요할까?
### GC 성능에 대한 주요 척도는 Throughput과 Latency인데 둘을 무엇을 의미할까?
    - Throughput
        
        Throughput은 처리량이라는 의미로 
        
    - Latency
        
        지연 시간, 즉 애플리케이션에 반응성에 대한 것으로, 가비지 컬렉션에 pause가 영향을 미친다.
        
         
        
        좀 더 공부
        
        ---
        
        [https://docs.oracle.com/en/java/javase/17/gctuning/garbage-collector-implementation.html#GUID-C2CA24AD-DC01-4B31-A868-F7DAC7E3BF4D](https://docs.oracle.com/en/java/javase/17/gctuning/garbage-collector-implementation.html#GUID-C2CA24AD-DC01-4B31-A868-F7DAC7E3BF4D)

### 각 버전별 Default GC는 어떻게 될까?

    java 8 - Parellel Collector
    
    java 9 ~12 - G1GC 
    
    ---
    
    [https://docs.oracle.com/en/java/javase/12/gctuning/garbage-first-garbage-collector.html#GUID-ED3AB6D3-FD9B-4447-9EDF-983ED2F7A573](https://docs.oracle.com/en/java/javase/12/gctuning/garbage-first-garbage-collector.html#GUID-ED3AB6D3-FD9B-4447-9EDF-983ED2F7A573)
# GC의 종류
## Serial Collector
### Serial Collector란?   
    싱글 프로세서 머신에 최적화된 가비지 컬렉터.
    
    최대 100MB정도 되는 작은 데이터셋을 사용한다면 멀티프로세서 머신에서 동작할지라도 유용할 때가 있다.
    
    쓰레드 간 커뮤니케이션 오버헤드가 적기 때문이다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html)
 ### Serial Collector의 동작 방식은?
    
    객체는 Eden 영역에 처음 생성된고 Eden 영역이 꽉 차거나 Survivor 영역이 일정 퍼센트를 넘게 될 때 Minor GC가 일어난다.
    
    - Minor GC 동작 방식
        1. Root 객체로부터 출발하여 객체 그래프를 탐색하며 살아 있는 객체들을 Marking한다.
        2. Marking되지 않은 객체들은 Garbage이므로 삭제를 한다.
        3. 가비지 컬렉터는 Eden 영역에서 살아 남은 객체를 Survivor 0과 Survivor1 중 비어 있는 영역으로 이동시킨다.
        4. Survivor 영역 중 비어 있지 않은 영역에서 살아 남은 객체들도 같은 영역으로 이동된다. 이 때 나이를 한 살 먹게 된다.
        5. 위 과정이 반복되다가 일정 나이가 지난 객체는 Old Generation으로 Promotion된다.
    
    Old Generation이 꽉 차게 되면 Major GC가 발생한다.
    
    - Major GC 동작 방식
        
        Minor GC와 마찬가지로 살아 있는 객체들을 Mark하고 나머지 객체들을 Sweep한다.
        
        이 때 메모리가 단편화 된 상태가 되는데 남아 있는 객체들을 한곳으로 모아주는 Compaction 작업이 일어난다. 
        
    
    ---
    
    [https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)   
## Parallel Collector
### Parallel Collector란?
    
    throughput collector라고도 불리는데, 이 컬렉터는 minor gc 시 병렬적으로 가비지 콜렉팅을 진행한다. major gc는 싱글 스레드로 작동한다.
    
    멀티프로세서 머신이나 멀티쓰레딩이 가능한 하드웨어에서 동작하는 medium-sized에서 large-sized data set을 가진 어플리케이션에 적절하다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html)
    
### Parallel Compaction은 뭘까?
    
    이 옵션을 사용하면 major gc 또한 병렬적으로 처리된다.
    
    large-sized data set을 가진 어플리케이션에 적절하다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/par-compaction-6.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/par-compaction-6.html)
    
### 왜 Parellel Compaction은 Parallel Compaction이라고 불릴까?
    
    major gc에 compaction 과정이 있기 때문이 아닐까..?
### Parallel Collector의 동작 방식은?
    
    Minor GC시 Parallel Copy라는 알고리즘을 사용한다.
    
    아 알고리즘은 Eden, Survivor Area의 Live Object Copy 작업을 여러 Thread가 동시 수행한다.
    
    Major GC는 Parallel Compaction 옵션을 키지 않을 경우 Serial GC와 동일한 Serial Mark-Sweep-Compact 알고리즘을 사용한다.
    
    Parallel Compaction 옵션을 사용할 경우 Parallel Mark-Sweep-Compact 알고리즘을 사용한다.
    
    ---
    
    류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 38p,2017       
## Concurrent Collectors
### Concurrent Collector란?
    
    자바 애플리케이션과 동시에 진행되는 가비지 컬렉터.
    
    짧은 pause를 위해 고안되었다. 다른 가비지 컬렉터들에 비해 stop the world의 시간이 짧아서 빠른 반응이 필요한 어플리케이션에 적합하다.
    
    medium에서 large size의 데이터 셋을 가지고 반응 속도가 전체적인 처리량보다 중요한 어플리케이션에 사용하면 좋다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html)
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/concurrent.html#mostly_concurrent](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/concurrent.html#mostly_concurrent)
### Concurrent Collector는 Stop-the-world 현상이 없을까?
    
    Serial이나 Parallel보다는 시간이 적지만 그래도 stop-the-world 현상이 일어난다.
    
    ---
    
    [https://www.oracle.com/technetwork/java/javase/memorymanagement-whitepaper-150215.pdf](https://www.oracle.com/technetwork/java/javase/memorymanagement-whitepaper-150215.pdf) - Design Choices에 Concurrent versus Stop-the-world 파트
    
    [https://docs.oracle.com/en/java/javase/18/gctuning/garbage-first-g1-garbage-collector1.html#GUID-CE6F94B6-71AF-45D5-829E-DEADD9BA929D](https://docs.oracle.com/en/java/javase/18/gctuning/garbage-first-g1-garbage-collector1.html#GUID-CE6F94B6-71AF-45D5-829E-DEADD9BA929D)   
### Concurrent Mark Sweep(CMS) Collector
     - CMS Collector의 동작 방식은?
        
        Young Area는 Parallel Copy 알고리즘이 사용되며 Old Area는 Concurreant Mark-Sweep 알고리즘이 사용된다.
        
         Concurreant Mark-Sweep 알고리즘도 마찬가지로 Mark와 Sweep 단계를 거치지만 Compaction단계는 없다. 왜냐하면 Compaction 작업은 stop the world가 필요하여 이는 pause time 증가로 이어지기 때문이다.따라서 반복된 Sweep은 메모리 단편화를 유발한다.
        
        한 편 Mark 단계와 Sweep 단계의 작업들은 쪼개서 단계적으로 하고, stop the world 하지 않은 상태에서도 이 작업이 진행되기 때문에 한번에 생기는 pause time이 짧아진다.
        
        ---
        
        류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 41p,2017
        
    - Floating Garbage문제란 무엇일까?
        
        Old Generation을 청소할 때 Concurrent Mark 단계에서 새롭게 Promotion되는 객체가 Promotion되자마자 죽는 경우가 있을 수 있다.
        (Major GC가 일어나는 중에 Minor GC가 끼어들 수 있다. 그럴 경우 이런 식으로 Major GC 진행 도중 새로운 객체가 Promotion되기도 한다.)
        
        Concurrent Mark 단계에서는 Initial Mark 단계에서 Old Area에 존재하던 객체에 대해서만 Marking을 수행하기 때문에 위와 같은 객체는 Major GC가 최종적으로 끝난 후에도 Garbage인 상태로 Old Area에 남아 있게 된다.
        
        또한 Live Object로 판명된 객체가 Collection 작업이 끝날 때쯤 dead될 수도 있다. 이러한 객체도 Major GC가 끝난 후에도 Garbage인 상태로 Old Area에 남아 있게 된다.
        
        물론 다음 번 Major GC 때 수집되지만 잠재적으로 Old Area를 확장시킬 수 있어서 좋지 않다.
        
        ---
        
        류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 43p,2017
        
        [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html#concurrent_mark_sweep_cms_collector](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/cms.html#concurrent_mark_sweep_cms_collector)
        
    - CMS는 Compacting 작업을 하지 않아 단편화가 생기는데 이를 어떻게 해결할까?
        
        Free List란 것을 사용한다.
        
        Promotion된 Object와 크기가 비슷한 Old Area의 Free Space를 Free List에서 탐색한 후 할당한다.
        
        이러한 작업은 다른 Collector들이 Old Area에 올라온 Object를 그냥 순서대로 할당하는 것에 비해 시간이 오래걸린다.
        
        ---
        
         류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 42p,2017   
        어플리케이션에 쓰일 processor resource가 GC에 사용되기 때문에 어플리케이션의 성능이 떨어진다.
    
### G1GC - Garbage-First Garbage Collector란?
 ### G1GC란?
    
    서버스타일의 가비지 컬렉터로 큰 메모리를 가진 멀티프로세서 머신에 특화되어 있다. 
    
    이전 컬렉터들이 낮은 pause time과 높은 throughput 둘 중 하나를 포기할 수 밖에 없었다. G1GC는 pause time을 줄임과 동시에 high throughput을 내면서 CMS GC를 대체할 목적으로 만들어졌다.
    
    다른 GC들과 달리 Heap의 물리적 Generation 구분을 없애고 전체 Heap을 1Mbytes 단위 Region으로 재편한다.
    
    각각은 논리적으로 Eden 영역, Survivor 영역, Old 영역과 Humongous 영역이라고 하는 거대한 오브젝트들을 담는 영역으로 나뉜다.
    
    Garbage First라고 불리는 이유는 이렇게 나누어진 Rigion 중 Garbage 비율이 많은 Region부터 Collection을 시작하기 때문이다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/g1_gc.html#garbage_first_garbage_collection](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/g1_gc.html#garbage_first_garbage_collection)
    
    류길현 외 2명 , JVM Performance Optimizing 및 성능분석사례, 초판 1쇄, 엑셈, 49p,2017
    
### G1GC의 동작 방식은?
    - Young GC - stop the world , pararell
        
        이전 컬렉터들과 마찬가지로 Eden 영역에 객체가 꽉차거나 Survivor 영역이 일정 크기 이상 차게 되면 Young GC가 일어난다.
        
        살아남은 객체들은 다른 Survivor 구역으로 이동하거나 Survivor 구역이 꽉 차면 새로운 구역으로 이동하는데 이렇게 이동된 구역은 새로운 Survivor 영역이 된다.
        
        객체들은 이동하면서 age를 먹게 되고 age가 threshold를 넘은 객체는 Old 영역으로 승격된다.
        
    - 
