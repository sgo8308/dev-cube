### String은 왜 기본 자료형이 아닐까?
    
    변수를 선언하면 그 변수는 비어 있는 메모리에 있는 특정 주소에 공간을 만든다.
    
    int형으로 선언할 경우 그 공간은 4byte의 크기를 갖는다.
    
    만약 String으로 선언한다면 몇 byte를 가져야 할까?
    
    String으로 올 수 있는 값은 크기가 변동적이기 때문에 이 값을 특정할 수 없다.
    
    예를 들어 String이 기본 자료형이고 String a = “hello”와 같이 선언했다고 해보자.
    
    a라는 값이 메모리 주소 0x11을 가르킨다고 하면, 다음과 같이 저장될 것이다.
    
    [  h  ][  e  ][  l  ][ l  ][  o  ]
    0x11 ... 
    
    이 때 a = “school과 같이 hello보다 더 긴 문자열을 재할당하면,
    a주소가 만들어 놓은 5바이트의 공간이 초과될 것이고 5바이트 공간 이후에 다른 값이 저장되어 있을 수 있는데
    그런 값을 덮어씌우던지 등 여러모로 문제가 생길 것이다.
    
    대신에 String을 참조자료형으로 만든다면 언제나 실제 값의 주소만을 갖기 때문에,
    아무리 실제 값이 커지더라도 다시 그 데이터에 주소만 저장하면 되므로 문제가 없다.
    
### String은 왜 immutable할까? ★
    1. 동시성 문제 해결
        
        불변하기 때문에 멀티쓰레드 프로그래밍시 발생할 수 있는 동시성 문제를 해결할 수 있다.
        
    2. 보안
        
        String은 주로 중요한 정보에 많이 쓰인다.
        
        예를 들면 유저 이름, 패스워드, url 등등
        
        만약 어떤 유저가 회원가입할 때 우리가 여러가지 체크를 통해서 제출된 이름이 규칙에 걸맞다는 것을 확인했다고 하자.
        
        만약 String이 mutable하다면, 이렇게 삼엄한 체크가 이루어진 후에도 안심할 수 없다.
        
        이 String의 레퍼런스를 갖고 있는 쪽이 database에 집어넣기 전에 String에 어떠한 변화를 줄 수도 있기 때문이다.
        (프로그램 바깥에서 어떻게 이 String을 변화시키는지는 잘 몰겠다.)
        
        하지만 String이 immutable하다면, 우리는 securiy check가 끝나면 안심하고 이 String을 다룰 수 있다.
        
    3. 성능
        
        자바에서 String은 String pool에 저장된다. 동일한 String을 사용하는 변수들이 String pool에 있는 하나의 데이터를 가르키게 하여 메모리를 절약한다.
        
        이 때 String이 mutable하다면 String이 변경됐을 때 이 String을 가르키는 모든 변수가 원치않는 변화로 피해를 입을 것이다.
        
        (여기서 말하는 변화는 String a = "a"한 후 a = "b"와 같이 단순히 a가 다른 값으로 할당되는 것을 말하는 것은 아니다.
        이 경우에는 어차피 a가 다른 주소를 가르키게 되는 것이니 String이 mutable하더라도 상관없다.
        여기서 말하는 변화는 a.toUpperCase()와 같이 이 값에 직접적으로 변화를 주는 것을 말한다.)
        
        즉 String pool이라는 기능이 가능한 것은 String이 immutable하기 때문이다.
        
        또한 String은 HashMap이나 HashSet의 자료 구조에서 key로 많이 쓰이게 된다.
        
        String이 immutable하기 때문에 String constant pool에 있는 String들은 한 번 hashCode()가 호출되면 그 값을 캐싱하고 있다가
        다시 한 번 호출될 때는 caching한 값을 반환하기 때문에 자료구조에서 데이터를 찾을 때마다 haschCode()의 연산 과정이 필요없어서 성능상 이점을 준다.
        
### String a = “hello” 와 String a = new String(”hello”) 의 차이는 뭘까? ★
    
    String a = “hello”와 같이 스트링을 만들어 줄 경우,
    이 “hello”라는 데이터는 heap안에 있는 “String constant pool”이라는 곳에 저장된다.
    
    이렇게 선언해 놓으면, 다음 번에 String b = “hello”와 같이 동일한 문자열을 선언할 경우,
    새롭게 메모리 영역에 데이터를 할당하는게 아니라 String constant pool”에서 동일한 데이터가 이미 존재하는지 찾고, 있다면 그 데이터의 주소를 b에 할당해준다.
    
    따라서 다음이 성립한다.
    
    String a = "hello";
    String b = "hello";
    
    a == b // true
    
    반면에 new 키워드로 스트링을 생성할 경우 이 데이터는 heap 영역 안에 “String constant pool” 밖에 저장된다.
    
    동일한 문자열을 다시 new로 생성하면 동일한 데이터를 heap 안에 새로운 영역에 또 할당한다.
    
    따라서 다음이 성립한다.
    
    String a = new String("hello");
    String b = new String("hello");
    
    a == b // false
    
    - String constant pool도 가비지 컬렉팅이 작동할까?
        
        String constatnt pool도 heap 영역 안에 있기 때문에 더 이상 접근 불가한 String들은 가비지 컬렉팅된다.
        
        Java 7 이전에는 특별한 heap 영역인 PermGen이라는 곳에 String constant pool이 위치했다. Java 7 이후에는 메인 heap 영역으로 옮겨졌다.
        
        참고로 Java 8 이후에는 PermGen을 Metaspace라는 녀석이 대체했다.
        
### StringBuffer와 StringBuilder는 왜 나왔을까?
    
    String의 immutable하기 때문에 String에 변화를 줄 때마다 jvm에 heap영역에 객체가 계속 생성되어 성능에 문제가 생기는 경우를 막기 위하여.
    
### 컴파일러가 최적화가 없다는 가정 하에, String 변수에 +를 반복하는 로직이 나쁜 이유는?
    
    String에 +를 반복하면 새로운 객체가 계속해서 heap에 쌓이게 된다.
    
    그 만큼 GC가 많이 발생하게 된다.
    
    GC를 하면 할수록 시스템의 CPU를 사용하게 되고 시간도 많이 소요된다.
    
    String에 +하는 로직은 jdk 5 이후에는 StringBuilder로 컴파일러가 알아서 변환해주지만
    
    +하는 로직이 반복문 안에 있을 경우에는 역시 계속해서 heap에 객체가 생성되므로
    이런 경우에는 StringBuilder나 StringBuffer를 사용하는 것이 좋다.
    
    ---
    
    이상민, 자바 성능 튜닝 이야기, 초판 4쇄, 인사이트, 41-56, 2019
    
### 왜 intern()메소드는 사용하면 안될까?
    
    새로운 문자열을 쉴새 없이 만드는 프로그램에서 intern() 메소드를 사용하여 억지로 문자열 풀에 값을 할당하도록 만들면, 저장되는 영역은 한계까 있기 때문에 그 영역에 대해서 별도로 메모리를 청소하는 단계를 거치게 된다. 따라서 전체 자바 시스템의 악영향을 주게 된다.
    
    ---
    
    이상민, 자바의 신, 2판 1쇄, 로드북, 408p, 2017