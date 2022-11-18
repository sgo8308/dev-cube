# Collection
### 컬렉션 프레임워크란?
    
    빈번하게 재사용되는 자료구조들을 구현하는 클래스와 인터페이스의 집합이다.
    
    ---
    
    [https://en.wikipedia.org/wiki/Java_collections_framework](https://en.wikipedia.org/wiki/Java_collections_framework)
    
### 컬렉션 프레임워크는 왜 나왔을까?
    
    JDK1.2 이전까지는 Vector, Hashtable, Properties와 같은 다수의 데이터를 저장할 수 있는 클래스들을 서로 다른 각자의 방식으로 처리해야 했다.
    
    데이터를 다루는 방식에 있어서 표준화가 필요해졌으며 이를 위해 컬렉션 프레임워크가 나오게 된 것 같다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 578p, 2016

### Iterator가 필요한 이유는?
    
    컬렉션에 저장된 요소들을 읽어오는 방법을 표준화함으로써 재사용성을 높히기 위해서.
    
    저장된 요소들을 읽어올 때는 iterator를 사용하는게 for문을 돌면서 get을 사용하는 것보다 재사용성이 높다.
    
    왜냐하면 set에는 get이 없다.
    
    list set 모두에서 같은 코드를 통해 저장된 요소를 읽으려면 iterator를 사용해야 한다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 615p, 2016

### Comparator, Comparable 2개가 존재하는 이유는?

# List
### ArrayList vs LinkedList ★
    
    둘은 구현한 방식이 다르다.
    
    이름에서 볼 수 있듯이 ArrayList는 내부적으로 배열을 갖고 있으며 LinkedList는 저장하는 값들을 Link하는 방식으로 구현되어 있다.
    
    ArrayList는 데이터를 자주 읽을일이 많을 때 사용하면 좋고, LinkedList는 데이터를 삭제하거나 리스트 중간에 추가하는 일이 많을 때 사용하면 좋다. 
    왜냐하면 ArrayList는 get에서, LinkedList는 add와 remove에서 시간 복잡도가 더 낮기 때문이다.
    
    ArrayList는 데이터들이 메모리에 연속적으로 저장되지만 LinkedList는 불연속적으로 저장된다.
    
    ArrayList가 다음 노드나 이전 노드를 가르키는 포인터를 저장할 필요가 없기 때문에 같은 크기에 LinkedList보다 메모리를 덜 차지한다.
    
    ---
    
    [https://www.javatpoint.com/difference-between-arraylist-and-linkedlist](https://www.javatpoint.com/difference-between-arraylist-and-linkedlist)
    
### ArrayList가 Vector보다 선호되는 이유는?
    
    더 빠르기 때문이다.
    
    빠른 이유는 ArrayList는 선택적으로 동기화를 할 수 있는 반면에
    
    Vector는 무조건 동기화 방식이기 때문이다.
    
    동기화에서 lock을 유지하는 과정 때문에 Vector는 multi thread에서 뿐만이 아니라 
    
    single thread에서도 살짝 더 느리다.
    
    그러나 single thread에서 그 차이는 미미하다. 
    
### 반복문에서 Arraylist의 remove 메소드를 쓸 떄 어떤 점을 고려해야할까?
    
    ArrayList의 remove메소드는 원소를 삭제하고 빈 공간을 채우기 위해 삭제된 원소의 뒤에 원소들을 앞으로 한 칸 씩 땡겨준다.
    
    만약 반복문에 i를 증가시키면서  i index의 원소들을 remove하면 원치 않는 결과가 나올 수 있다. 
    예를 들면 0번 index의 원소를 삭제하기 전에 1번 인덱스에 있던 값이 삭제하고 난 후에는 0번 인덱스가 되기 때문이다.
    
    따라서 이럴 때는 뒤에서부터 i를 감소시키면서 remove 해주는게 좋다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판,587p, 2016
    
### ArrayList를 생성할 때 성능 관점에서 어떤 점을 고려하면 좋을까?
    
    크기를 예상할 수 있다면 초기 크기를 정해주는 생성자를 사용할 것을 고려해볼 수 있다.
    
    왜냐하면 ArrayList는 내부 배열의 크기를 초과하면 더 큰 배열을 만들어 기존의 값들을 옮기는데 , 이 때 시간이 많이 들기 때문이다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 590p, 2016

# Map
### 해시맵의 최악의 케이스는 무엇이고 자바는 어떻게 이것을 개선했을까? ★
    
    해시맵의 최악의 케이스는 하나의 bucket에 모든 값이 들어가는 경우입니다.
    
    기본적으로는 이 상황이 발생하지 않기 위해 해시맵 내부의 배열의 크기를 크게 만들거나 해시함수를 최대한 잘 만들어야 합니다.
    
    해시 함수 같은 경우 JDK 1.4 이후부터는 보조 해시 함수라는 것을 사용해서 기존보다 해시 충돌이 적게 일어날 수 있도록 개선되었습니다.
    
    자바 8부터는 bucket에 들어가는 자료구조를 변경하여 성능을 더 개선하였는데요.
    
    자바 8 이전까지는 bucket에 LinkedList를 담는 방식으로 해시 충돌을 처리했기 때문에 O(N)만큼의 시간복잡도를 가지고 있었습니다.
    
    그러나 자바 8 이후부터는 bucket이 특정 threshold를 넘을 경우 Linked List가 아닌 Red Black Tree를 사용하여 시간복잡도가 O(logN)이 되도록 개선하였습니다.
    
    모든 경우에 대해서 Red Black Tree를 쓰지 않는 이유는 데이터가 작을 때는 두 방식의 성능 차이가 유의미하지가 않은 반면 Red Black Tree는 Linked List보다 메모리 사용량이 많기 때문이다.
    
    ---
    
    [https://d2.naver.com/helloworld/831311](https://d2.naver.com/helloworld/831311)
    
### 해시맵의 동작 원리는?
    
    해시맵의 자료구조는 배열과 링크드 리스트의 조합으로 되어 있다.
    
    key에 해싱함수를 적용하여 배열에 대한 인덱스를 알아낸다.
    
    그리고 value를 배열에 있는 링크드 리스트에 연결한다
    
    [HashMap](https://www.notion.so/HashMap-f606dab3e77a47e7ac7f74ce4d1d8696)
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 651-652, 2016
    
### 해시맵과 해시테이블의 차이점은?
    1. 해시테이블은 thread-safe하나 해시맵은 그렇지 않다. 
        
        대신 Collections.synchronizedMap()을 이용하거나 직접 lock code를 작성하여 thread-safe한 HashMap을 만들 수 있다.
        
        해시맵은 동기화하지 않기 때문에 더 빠르고 메모리도 덜 차지한다.
        
        일반적으로 싱글 쓰레드에서 unsynchronized object가 synchronized one보다 더 빠르다.
        
    2. 해시맵은 key나 value로 null을 허용하는 반면 해시테이블은 허용하지 않는다.
    
### 언제 해시맵을 쓰고 언제 해시테이블을 써야할까?
    
    싱글 쓰레드와 같이 동기화가 필요한 상황이 아니라면 해시맵을 쓰는 것이 낫다.
    
    해시테이블은 JDK1.8부터 deprecated 됐기 때문에 쓰지 않는 것이 낫고, 
    동기화가 필요한 상황이라면 ConcurrentHashMap을 쓰는 것이 낫다.
    
    ---
    
    [https://www.baeldung.com/hashmap-hashtable-differences#:~:text=Firstly%2C Hashtable is thread-safe,safe version of a HashMap](https://www.baeldung.com/hashmap-hashtable-differences#:~:text=Firstly%2C%20Hashtable%20is%20thread-safe,safe%20version%20of%20a%20HashMap).
    
### 해시충돌은 배열을 크게 늘릴수록 개선이 되는데 그럼 무작정 늘리면 좋을까?
    
    해시맵에서 iterator를 쓸 때는 배열의 크기에 비례해서 시간이 걸리므로 무작정 늘리는 것은 좋지 않다.
    
    해시맵에 어느정도의 원소들이 들어갈지 예상하고 load factor 고려해서 초기에 정해주는 게 제일 좋다.
    
    ---
    
    Java 11 API document - HashMap 설멍부분

### ConcurrentHashMap은 무엇이고 어떤 문제를 해결해줄까?
    
    일반적인 HashMap은 Thread Safe하지 않다. 따라서 다음과 같은 상황에서 문제가 발생할 수 있다.
    
    1. Null 확인 후 집어넣기, read - modify - write
        
        // Null 확인 후 집어넣기
        HashMap<String, Object> something = new HashMap<>();
        
        if(something.get("a") == null){
        	Object object = new Object();
        	something.put("a", object);
        	object.doSomething();
        	...
        }
        
        // read - modify - write
        HashMap<String, Integer> something = new HashMap<>();
        
        int a = something.get("a");
        something.put("a", a + 1);
        
        일반적인 HashMap이라면 멀티 쓰레드 상황에서는 위 코드는 race condition으로 인해 문제가 발생할 확률이 높다. 
        
        하지만 ConcurrentHashMap을 사용할 경우 putIfAbsent()나 computeIfPresent()와 같이 자주 쓰이는 복합 
        연산을 단일 연산화한 메소드를 동기화 된 방식으로 사용할 수 있기 때문에 race condition으로부터 자유롭다.
        
    2. 전체 Map을 순회하면서 값을 다루는 상황
        
        HashMap<String, Object> something;
        
        for(Map.Entry<String, String> entry : something.keySet()){
        ...
        }
        
        만일 위 상황에서 다른 쓰레드가 Map에 있는 값을 수정할 경우, ConcurrentModificationException이 일어난다. 
        
        그 이유는 값 수정으로 인해 잘못된 결과가 나올 수 있기 때문이다.
        
        하지만 ConcurrentHashMap을 사용하면 Iterator을 사용한 시점에 Map 상태의 스냅샷을 찍어놓고 진행하기 때문에
        위 예외로부터 자유롭다.
        
### ConcurrentHashMap은 내부적으로 어떻게 동기화를 할까?
    
    Lock Striping 기법을 사용한다. 여러개의 자원을 두고 하나의 락을 걸던 것을 각각의 자원에 대해 각각의 락을 걸게끔
    하는 것을 락 분할이라고 한다.
    
    Lock Striping 기법은 락 분할을 더 쪼갠 것으로, 하나의 자원을 여러개의 락으로 분할하여 접근하는 것을 말한다.
    
    예를 들어 ConcurrentHashMap에서는 각 버킷에 락을 건다. 
    따라서 서로 다른 버킷에 접근하는 쓰레드들은 자유롭게 맵에 접근 할 수 있어 
    HashTable이나 SynchronizedHashMap보다 성능이 뛰어나다.

# Set
### TreeSet이란? 필요한 이유?
    
    TreeSet이란 이진 검색 트리라는 자료구조로 데이터를 저장하는 컬랙션 클래스다.
    
    이진 검색 트리를 사용하기 때문에 HashSet보다 성능은 떨어지지만 정렬된 형태를 유지할 수 있다.
    
    자바에서는 이진 검색 트리의 최악의 케이스를 막도록 도와주는 Red Black Tree를 사용한다.
    
### TreeSet이 정렬을 유지하는 법은?
    
    TreeSet은 기본적으로 이진 검색 트리를 내부적으로 사용한다.
    
    이진 검색 트리를 왼쪽부터 중위 순회하게 되면 오름차순으로 읽을 수 있게 된다.
    
    반대로 하면 내림차순이다.

# Stack
### Stack은 좋지않은 구현이라고 하는데 이유가 뭘까?
    일단 Stack은 Vector의 한 분류가 아님에도 불구하고 상속 관계에 있으므로 상속을 잘못 사용했다.

    또한 Stack은 Vector를 상속했기 때문에 메소드의 모든 내부 구현에서 부모인 Vector의 메소드를 사용한다.
    
    Vector는 모든 메서드에 synchronized가 걸려있다. 
    
    따라서 메서드를 사용할 때 마다 lock을 획득하고 반납하는 오버헤드가 걸린다.
    
    멀티 쓰레드 프로그램이 아닐 경우 이러한 오버헤드는 불필요할 뿐더러 멀티 쓰레드 프로그램이라 하더라도 특정한 상황에서 비효율적이다.
    
    예를 들면 for문에서 Vector의 get을 여러번 호출하는 경우 매 반복마다 lock을 획득하고 반납해야 한다.
    lock을 한번만 획득하고 for문이 끝나면 반납하면 되는데 Vector는 메소드에 synchronized가 붙어 있어서 그게 불가능하다.

    따라서 Stack은 좋지 않은 구현인 Vector를 상속했기 때문에 좋지 않은 구현이다.
    
    ---
    
    [https://aahc.tistory.com/8](https://aahc.tistory.com/8)
# Stream
### Stream이란?
    
    컬렉션과 배열의 데이터를 선언적인 방식으로 처리할 수 있게 도와주는 API이다.
    
    또한 멀티코어를 활용한 멀티쓰레드 방식의 처리를 쉽게 할 수 있게 도와준다.
    
    ---
    
    [https://www.oracle.com/technical-resources/articles/java/ma14-java-se-8-streams.html](https://www.oracle.com/technical-resources/articles/java/ma14-java-se-8-streams.html)

