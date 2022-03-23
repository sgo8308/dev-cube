### Enum 클래스는 무엇일까?
    
    상수의 집합.
    
    어떤 클래스가 상수만으로 이루어졌을 때 이 클래스를 enum 으로 만들면 
    상수의 집합이라는 것을 명시적으로 나타낼 수 있어서 좋다. 
    
### Enum 클래스는 언제 사용할까?
    
    int 상수를 사용할 필요가 있을 때 열거 타입을 사용하면 좋다.
    
    왜냐하면 타입에 안전하고, 이 상수들에 필요한 행위를 enum 클래스 안에 구현하는 방식으로 관련 있는 코드를 한 곳으로 모을 수 있기 때문이다.
    또한 switch 문에서 사용할 때는 어떤 case 인지 상수 이름으로 구분할 수 있기 때문에 가독성에 좋다.
    
    ---
    
    [https://techblog.woowahan.com/2527/](https://techblog.woowahan.com/2527/)
    
### Enum 클래스의 생성자는 왜 public이나 protected를 사용하면 안 될까?
    
    만약 public이나 protected 생성자가 있다면 enum 클래스 안에 있는 상수들을 사용하는게 아니라 enum 클래스를 직접 인스턴스화해서 사용 가능해지는데, 
    이런 식으로 사용되게끔 enum을 만든게 아니라서 그렇지 않을까?
    
### C언어의 Enum과 자바의 Enum의 차이점은?
    
    자바의 Enum은 C언어의 Enum과 달리 타입이 다르면 == 비교시 false를 리턴한다.
    
    반면에 C언어의 Enum은 타입이 달라도 값이 같으면 == 비교시 true를 리턴한다.
    
### Enum은 내부적으로 어떻게 구현이 되어 있을까?
    
    다음과 같은 코드를 컴파일하면
    
    public enum Direction { EAST, SOUTH, WEST, NORTH}
    
    다음과 같이 된다.
    
    public final class Direction extends java.lang.Enum<Direction>{
    	public static final Direction EAST = new Direction("EAST", 0); //name, ordinal
    	public static final Direction SOUTH = new Direction("SOUTH", 1);
    	public static final Direction WEST = new Direction("WEST", 2);
    	public static final Direction NORTH = new Direction("NORTH", 3);
    
    	//생성자나 name 변수, ordinal 변수는 부모 클래스(Enum)에 선언되어 있음
    }
    
### Enum에서 추상 메소드를 구현한 후 상수들에서 추상 메소드를 오버라이딩 할 때 왜 enum 클래스에 변수의 접근자가 priavte인 것은 접근하지 못할까?
    
    추상 메소드가 없을 때는 상수들은 모두 enum 클래스의 타입과 동일하기 때문에 private 변수도 접근 가능하다.
    
    그러나 추상 메소드가 있을 때는 상수들은 enum 클래스를 상속하는 익명클래스의 형태로 만들어지기 때문에 private 변수에 접근 불가능하다.
    
    따라서 이 때는 protected로 만들어주어야 한다.
    

---

남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 691-701, 2016

이상민, 자바의 신, 2판 1쇄, 로드북, 327-335, 2017