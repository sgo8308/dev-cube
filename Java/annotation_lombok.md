# 어노테이션
### 어노테이션이란?
    
    Annotation은 주석이라는 뜻을 갖고 잇다.
    
    보통 주석은 프로그램을 읽는 개발자를 위한 것이다.
    
    Annotation은 개발자 뿐만 아니라 프로그램을 읽는 다른 프로그램을 위한 주석이다.
    
    예를 들어 컴파일러는 어노테이션을 통해서 어노테이션이 붙은 클래스나 메소드에 대한 정보를 얻는다.
    
    테스트 프로그램은 @Test라는 어노테이션을 통해 어떤 메소드를 테스트해야 할지 알 수 있다.
    
### 어노테이션이 없었을 때는 어땠을까?
    
    어노테이션이 없었을 때는 자바 애플리케이션의 설정을 XML이나 properties라는 파일에 지정해왔다. 
    하지만 설정이 복잡해지고, 어떤 설정이 어디에 쓰이는 지 이해하려면 많은 시간이 소요되었다. 
    
### 어노테이션은 컴파일 후에 사라질까?
    
    어노테이션에 붙어 있는 @Retention이라는 메타 어노테이션에 따라 컴파일 후에 어노테이션 정보가 사라질 수도 있고 안 사라질 수도 있다.
    
### 어노테이션 프로세서란?
    
    어노테이션 프로세서는 javax.annotation.processing 패키지에 들어 있는 Processor 인터페이스를 구현한 구현체다.
    
    어노테이션이 처리되는 과정에서 어노테이션에 맞는 적절한 어노테이션 프로세서가 필요하다.
    
### @Retention에서 쓰이는 RetentionPolicy들을 설명해주세요.
    
    source가 붙은 어노테이션은 컴파일 시 어노테이션 정보가 사라지므로 어노테이션 정보는 컴파일러에 의해서 사용됩니다.
    
    class가 붙은 어노테이션은 run time에 유지되지 않으므로 클래스 로더에 의해서 사용되는 것으로 보입니다. 또한 이미 .class 파일로 존재하는 라이브러리에 타입체커나 IDE의 부가기능(@NonNull이 붙어 있는 변수에 null 넣으면 빨간줄)을 위해서 사용됩니다.
    
    runtime이 붙은 어노테이션은 run time 도중에도 유지되므로 Reflection API를 통해 어노테이션 정보를 얻을 수 있습니다.  
    
    ---
    
    참조 자료
    
    [https://jeong-pro.tistory.com/234](https://jeong-pro.tistory.com/234)
    

---

참고 자료

남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 702-720, 2016

이상민, 자바의 신, 2판 1쇄, 로드북, 432-444, 2017

# 롬복
### 롬복이란?
    
    롬복은 getter나 setter같은 boilerplate code들을 간단한 어노테이션만으로 자동 생성하게 해주는 자바라이브러리다.
    
    - boilerplate code란?
        
        다양한 곳에서 변화없이 반복적으로 사용되는 코드
        
### 롬복은 왜 사용할까?
    
    개발자를 반복노동에서 해방시켜준다.
    
    도메인 특화적인 코드만 남김으로써 클래스가 무엇을 하는지 명확하게 알 수 있다.

### 롬복은 어떤 원리로 작동할까? ★
    - Main
        
        컴파일 시점에 바이트 코드를 다시 조작해서 코드를 만들어낸다.
        
    - Sub
        
        javac은 컴파일시 다음과 같은 사전 단계를 거친다.
        
        1. 소스 파일은 파싱하여 AST(Abstract Syntax Tree)를 만든다.
        2. 어노테이션 프로세싱 과정을 거치면서 AST가 수정된다.
        
        이후 수정된 AST를 javac이 컴파일하게 된다.
        
        lombok은 2번 과정에서 관여를 한다. 
        
        먼저 어노테이션 프로세싱 과정에서 프로세서로 롬복 어노테이션 프로세서가 선택된다.
        
        이 프로세서는 롬복 어노테이션 핸들러에게 AST 오브젝트를 넘긴다.
        
        롬복 어노테이션 핸들러는 AST 오브젝트에  다른 노드(메소드나 필드나 표현)들을 삽입한다. (@Getter라면 Getter메소드에 맞는 노드들을 삽입)
        
    
    ---
    
    [http://notatube.blogspot.com/2010/12/project-lombok-creating-custom.html](http://notatube.blogspot.com/2010/12/project-lombok-creating-custom.html)
    
### 주의해야할 롬복 어노테이션은 어떤게 있을까?
    - @ToString, @EqualsAndHashCode
        - @ToString, @EqualsAndHashCode 공통
            
            상호 참조하는 클래스에서 둘 다 이 어노테이션을 사용할 경우 무한루프가 될 수 있다.
            
            @EqualsAndHashCode
            @ToString
            class A{
                B b;
            }
            
            @EqualsAndHashCode
            @ToString
            class B{
                A b;
            }
            
            이 경우 A에서 toString()을 호출하면 그 안에서 B의 toString()을 호출하는데 이 안에서는 다시 A의 toString()을 호출하는 식으로 무한루프에 빠진다.
            
            @EqualsAndHashCode도 마찬가지다.
            
            - 테스트 코드
                1. @ToString
                    
                    public class LearningTest {
                    
                        public static void main(String[] args) {
                            A a = new A();
                            B b = new B();
                            b.setA(a);
                            a.setB(b);
                    
                            System.out.println(a.toString()); // 무한루프 발생
                        }
                    }
                    
                    @Data
                    class A{
                        public B b;
                    }
                    
                    @Data
                    class B {
                       public A a ;
                    
                    }
                    
                2. @EqualsAndHashCode
                    
                    public class LearningTest {
                    
                        public static void main(String[] args) {
                            A a = new A();
                            B b = new B();
                            b.setA(a);
                            a.setB(b);
                    
                            A a2 = new A();
                            B b2 = new B();
                            b2.setA(a2);
                            a2.setB(b2);
                    
                            System.out.println(a.equals(a2)); // 무한루프 발생
                        }
                    }
                    
                    @Data
                    class A{
                        public B b;
                    }
                    
                    @ToString
                    @EqualsAndHashCode
                    class B {
                       public A a ;
                    }
                    
    - AllArgsContstructor, RequiredArgsConstructor
        
        리팩토링하면서 필드의 순서를 바꿀 경우 롬복은 바뀐 순서에 맞게 새로운 생성자를 생성할 것이다.
        하지만 기존 생성자를 사용하던 코드는 자동으로 변환이 안되기 때문에 문제가 생길 수 있다.