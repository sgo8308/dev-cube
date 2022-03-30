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

