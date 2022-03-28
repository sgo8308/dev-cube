# Gc
### GC란?
    
    더 이상 참조되지 않아 접근이 불가능한 메모리를 Garbage라고 부르는데 이 메모리를 회수하는 것을 Garbage Collection이라고 부른다.
    
    GC는 보통 Garbage Collector에 의해 자동으로 이루어진다.
    
    ---
    
    [https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))

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
## Serial Collector란?
    
    싱글 프로세서 머신에 최적화된 가비지 컬렉터.
    
    최대 100MB정도 되는 작은 데이터셋을 사용한다면 멀티프로세서 머신에서 동작할지라도 유용할 때가 있다.
    
    쓰레드 간 커뮤니케이션 오버헤드가 적기 때문이다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html)
    
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
        
## Concurrent Collectors
### Concurrent Collector란?
    
    자바 애플리케이션과 동시에 진행되는 가비지 컬렉터.
    
    짧은 pause를 위해 고안되었다. 다른 가비지 컬렉터들에 비해 stop the world의 시간이 짧아서 빠른 반응이 필요한 어플리케이션에 적합하다.
    
    medium에서 large size의 데이터 셋을 가지고 반응 속도가 전체적인 처리량보다 중요한 어플리케이션에 사용하면 좋다.
    
    ---
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/collectors.html)
    
    [https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/concurrent.html#mostly_concurrent](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/concurrent.html#mostly_concurrent)
    
### Concurrent Mark Sweep(CMS) Collector란?
    
    어플리케이션에 쓰일 processor resource가 GC에 사용되기 때문에 어플리케이션의 성능이 떨어진다.
    
### G1GC - Garbage-First Garbage Collector란?
            
            
    