### 왜 모든 클래스는 Object 클래스를 상속할까?
    
    모든 클래스가 기본적으로 갖고 있어야 할 메소드들이 있는데,
    그런 메소드들을 Object에 정의하여 중복을 피할 수 있기 때문이다.
    
### equals()와 hashCode()는 무엇이고 둘은 무슨 관계인가? (해시맵 구현체 엮어서)
    - equals()
        
        equals는 두 객체가 같은지 비교하는 메소드이다. 
        Object 클래스에는 단순히 == 비교를 하는 방식으로 구현되어 있다.
        
        보통은 객체들이 Override해서 사용하는데, equals를 재정의할 때 따라야하는 5가지 규칙에 맞춰서 재정의해야 한다.
        
        보통은 IDE에서 이러한 규칙에 맞는 템플릿을 제공해준다.
        
    - hashCode()
        
        hashCode()는 네이티브 메소드이며 객체에 따른 특별한 정수를 리턴한다.
        
        객체에 메모리 주소일 수도 있고 아닐 수도 있다.
        (자바 버전에 따라서 다른데 5 버전에서는 메모리 주소를 반환하게끔 장려되었지만 
        이 때도 강제는 아니었다. 최신 버전(17)에는 이러한 부분에 대한 내용이 언급되어 있지 않고,
        11버전에서는 메모리 주소에 대한 연산으로 구현될 수도 있고 아닐수도 있다고 언급되어 있다.)
        
        equals()로 판정한 같은 객체라면 반드시 같은 정수를 갖는다.
        
        하지만 다른 객체도 같은 hashCode()값을 갖을 수 있다.
        
    
    hashCode()는 hashMap에서 자주 쓰이는데 key에 대한 initial hash를 구할 때 hashCode()가 쓰인다.
    
    equals는 hashMap 안에 있는 같은 bucket 안에 있는 노드 중에서 현재 key와 같은 key를 찾기 위한 과정에서 사용된다. 
    
    상세한 부분은 [HashMap](https://www.notion.so/HashMap-f606dab3e77a47e7ac7f74ce4d1d8696) 
    
    ---
    
    [java api document - Object Class 부분](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Object.html#hashCode())
    
    [https://docs.oracle.com/javase/1.5.0/docs/api/](https://docs.oracle.com/javase/1.5.0/docs/api/)
    
    [https://www.baeldung.com/java-object-memory-address](https://www.baeldung.com/java-object-memory-address)
    
### hashCode()는 프로그램 실행 때마다 동일한 객체에 대해서 동일할까?
    
    동일할 수도 있고 그렇지 않을 수도 있다.
    
### 과거에 어떻게 hashCode()가 memory 주소를 반환했을까? GC에 의해서 메모리 주소가 바뀔 텐데, 그렇다면 같은 객체에 대해서 애플리케이션이 종료될 때까지 같은 값을 유지해야 한다는 hashCode()의 규칙을 지킬 수 없지 않을까?
### 왜 equals()와 hashCode()를 같이 Override 해야 할까?
    
    동일한 객체는 동일한 해시코드를 갖아야 한다는 규칙 때문에,
     equals()가 재정의되면 hashCode()도 그에 따라 재정의 되어야 한다.
    
### toString()에 나온 @1b67f74d와 같은 것은 뭘까?
    
    toString()을 하게 되면 객체의 hashCode()의 16진수 값을 @뒤에 붙여서 출력해준다.
    
### toString()은 언제 Overriding하는 것이 좋을까?
    
    객체의 내용(변수 값들)을 쉽게 알고 싶을 때.
    
    특히 DTO를 사용할 때는 꼭 Overriding 하는 것이 좋다.
    그래야 내용 확인이 쉽기 때문이다.
    
### clone()은 shallow copy를 할까 deep copy를 할까?
    
    shallow copy다 
    
    그래서 객체를 복제할 때  mutable한 참조 변수가 존재한다면 이 변수의 클래스  또한 Cloneable 인터페이스를 구현한 후 clone() 메소드를 조금 수정해주어야 깊은 복사가 된다.
    
    예를 들면 이런식으로 해야 한다.
    
    public class Student implements Cloneable { 
    	String name; 
    	int age; 
    	Family family; 
    
    	@Override 
    	public Object clone() throws CloneNotSupportedException { 
    		Student student = (Student)super.clone(); 
    		student.family = (Family)family.clone(); 
    		return student; 
    	} 
    }
    
    public class Family implements Clonealbe { 
    	String name; 
    	int age; 
    	boolean isOfficeWorkers;
     
    	@Override public Object clone() throws CloneNotSupportedException { 
    		return super.clone(); 
    	} 
    }
    
    그렇지 않다면 객체 안에 있는 mutable한 참조 변수는 얕은 복사가 되어 완전한 깊은 복사가 이루어지지 않을 것이다. 
    
    ---
    
    [https://rok93.tistory.com/entry/얕은복사-VS-깊은복사]
    
    - shallow copy는 뭐고 deep copy는 뭘까?
        - shallow copy
            
            A라는 클래스를 B라는 클래스가 shallow copy했다고 하자.
            
            B는 A라는 클래스가 가진 모든 변수와 동일한 참조를 갖는다.
            
        - deep copy
            
            A라는 클래스를 B라는 클래스가 deep copy했다고 하자.
            
            A의 모든 변수들을 새로 만들어져서 메모리에 할당되고
            B는 이 새로 만들어진 값 혹은 객체들을 참조한다.
            
            B는 A라는 클래스가 가진 모든 변수와 다른 참조를 갖는다.
            
        
        ---
        
        [https://en.wikipedia.org/wiki/Object_copying](https://en.wikipedia.org/wiki/Object_copying)

### clone()을 사용 하면 안 좋을까?


    배열의 복사를 위해서라면 clone()을 쓰는 것이 좋지만,객체의 복사를 위해서라면 clone()보다는 복사 생성자나 복사 팩토리 방식을 쓰는 것이 좋다.

    왜냐하면 객체 안에 가변 객체가 포함된다면 가변 객체에 대한 복사작업을 다시 구현해주어야 하고
    CloneNotSupportedException 처리와 같은 부분이 없어서 깔끔하게 복사할 수 있다.

    또한 ArrayList에서 LinkedList로 복사하는 등 더 유연하게 복사 가능하다.

    ---

    [https://www.artima.com/articles/josh-bloch-on-design#part13](https://www.artima.com/articles/josh-bloch-on-design#part13) - Copy Constructor versus Cloning 파트

    [https://it-mesung.tistory.com/190](https://it-mesung.tistory.com/190)

