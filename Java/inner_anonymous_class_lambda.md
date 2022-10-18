# 내부 클래스
### 내부 클래스는 언제 사용해야 할까?
    어떤 클래스가 하나의 클래스에서만 사용되고 다른 곳에서는 사용될 일이 없을 때 사용하면 좋다. 

    특히 자바 기반의 UI 처리나 사용자의 입력, 외부의 이벤트에 대한 처리를 하는 곳에서 많이 사용된다. 
    그러나 이런 부분 외에는 잘 사용되지 않는다.
### 내부 클래스를 사용하면 어떤 점이 좋을까?
    
    클래스 파일 갯수가 줄기 때문에 관리가 용이하다고 생각한다.

### 내부 클래스에서 this는 외부 클래스를 가르킬까?
    
    내부 클래스에 this는 내부 클래스를 가르킨다.
    
    외부 클래스를 가르키려면 OuterClass.this 이런식으로 사용해야 한다.

### 클래스를 몇개까지 중첩할 수 있을까?
    
    자바 언어 명세에서는 찾을 수 없기 때문에 확실히 알 수는 없지만
    stackoverflow의 누군가의 실험에 의하면 30개까지는 가능하나 300개에서는 에러가 났다고 한다.
    
    ---
    
    참조 문헌
    
    [https://stackoverflow.com/questions/1785686/how-many-times-can-classes-be-nested-within-a-class]
    
### 내부 클래스는 컴파일 했을 때 어떻게 될까?
    
    다음과 같은 클래스를 만들고
    
    public class HelloWorld {
    	class InnerClass{}
    	static class StaticInnerClass{}
    }
    
    다음과 같은 클래스를 만들어서 컴파일 했을 때
    
    public class SwitchTest {
        public static void main(String[] args) {
            HelloWorld h = new HelloWorld();
        }
    }
    
    다음과 같은 .class 파일이 생성되었다.
    
    SwitchTest.class
    HelloWorld.class
    HelloWorld$InnerClass.class
    HelloWorld$StaticInnerClass.class
    
### 한 파일 안에 2개에 클래스가 있다면 컴파일 했을 때 어떻게 될까?
    
    다른 하나의 클래스를 사용하지 않더라도 두 개의 .class 파일로 분리되어 컴파일된다.
    
    ---
    
    직접 실험

### 왜 내부 클래스는 가능한 static으로 만드는 것이 좋을까?
    
    #제대로 작동 안함
    
    내부 클래스를 static으로 만들지 않을 경우 내부 클래스는 바깥 클래스의 참조를 갖게 된다.
    
    참조를 갖고 있기 때문에 바깥클래스의 변수나 메소드를 사용할 수 있다.
    
    하지만 참조를 갖고 있다는 점은 이러한 장점보다는 참조로 인해 GC 대상에서 제외되고 메모리 릭을 일으킬 수 있다는 점에서 치명적이다.
    
    이렇게 내부적으로 참조되고 있는 것은 찾아내기도 힘들기 때문에 되도록 static으로 사용함으로써 이런 일을 사전에 예방하는 것이 좋다.
    
    ---
    
    https://johngrib.github.io/wiki/java-inner-class-may-be-static/
# 익명 클래스
### 익명 클래스가 뭘까?
    
    말 그대로 이름이 없는 클래스다.
    
    클래스 내부에서 생성되기 때문에 내부 클래스이다.

### 익명 클래스는 왜 사용할까?
    
    어떤 내부 클래스가 여러 곳에서 사용되지 않고 한 부분에서만 사용될 때
    이런 클래스를 이름을 주어 만드는 것보다 간단하게 익명 클래스로 만드는 것이
    코드 작성도 편하고 코드 관리에도 용이하다.
    
    하지만 익명 클래스를 쓸 일이 있을 때 람다를 쓸 수 있다면 람다를 쓰는 것이 낫다.
    
    익명 클래스가 주는 장점을 람다로 더 극대화 할 수 있기 때문이다.
    
### 익명 클래스는 어떻게 생성할 수 있을까?
    
    new 조상클래스생성자(매개변수){
    	//멤버 선언
    }
    
    new 구현인터페이스이름(){
    	//멤버 선언
    }
    
### 익명 클래스의 제약은 무엇이 있을까?
    - 이름이 없기 때문에 생성자를 가질 수 없다.
    - 여러 인터페이스를 상속할 수 없다.

### 익명 클래스는 왜 사용할까?
    익명 클래스는 일회용으로 쓰는 느낌이 강한데,
    이런 클래스를 이름을 주어 만드는 것보다 간단하게 익명 클래스로 만드는 것이 코드 작성도 편하고 코드 관리에도 용이하다고 생각한다.

### 익명 클래스가 들어 있는 클래스를 컴파일하면 어떻게 될까?

    class OuterClass{
    	Object o = new Object(){void method()}; //익명 클래스
    	static Object cd = new Object(){void method()}; //익명 클래스
      void myMethod(){
    		Object iv = new Object(){ void method()} // 익명 클래스
    	}
    }

    OuterClass.class

    OuterClass$1.class

    OuterClass$2.class

    OuterClass$3.class

    모두 ‘외부 클래스$숫자.class’ 형식으로 따로 클래스 파일이 생성된다.

# 람다 표현식
### 람다 표현식이란?
    
    함수형 인터페이스를 구현한 익명 클래스를 간결하게 표현한 것.
    
### 람다 표현식은 왜 사용할까?
    
    코드가 간결해지기 때문에 사용한다.
    
    람다식은 메소드의 매개변수로 전달되어지는 것과 리턴되는 것이 가능하다.
    
    람다식은 함수처럼 생겼기 때문에 마치 함수형 프로그래밍에서 함수를 인자로 넘기고 리턴하듯이 사용하게 되어 간접적으로 함수형 프로그래밍을 구현할 수 있다.

#참조 문헌

[https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)

남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, (402-411 , 794-), 2016

이상민, 자바의 신, 2판 1쇄, 로드북, 416-426, 2017