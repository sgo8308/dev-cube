### wrapper class란?
    
    기본형 값을 감싸는 클래스로 , 기본형 값을 객체로 다룰 수 있게 해준다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 490p, 2016
    
### 기본 자료형이 있는데 wrapper class가 필요하는 이유는 뭘까?
    
    기본형 값들을 객체로 저장해야 할 때(ArrayList<Integer>) 필요하다.
   
    java.util package가 오브젝트들을 다루는 유틸리티 클래스를 제공하기 때문에 이들 클래스들을 기본 자료형에 사용하려면 wrapper class가 필요하다.
    
    ---
    
    [https://www.geeksforgeeks.org/wrapper-classes-java/#:~:text=A+Wrapper+class+is+a,into+a+wrapper+class+object](https://www.geeksforgeeks.org/wrapper-classes-java/#:~:text=A+Wrapper+class+is+a,into+a+wrapper+class+object).
    
### primitive type vs wrapper type ★
    
    primitive type은 산술 연산자나 비교 연산자 사용이 가능한 반면 wrapper type은 사용이 불가능하다. 
    (wrapper class에 이 연산자들을 사용하면 primitive type으로 언박싱 된 후 사용된다.)
    
### Integer(int value)생성자는 자바9 이후로 왜 deprecated됐을까?
    
    Integer.valueOf(int)라는 팩토리 메소드를 쓰는게 더 낫기 때문이다.
    
    왜냐하면 기본적으로 -128에서 127까지의 값을 캐싱하고 있기 때문에
    이 범위에 있는 값을 요청하면 기존에 가지고 있던 객체를 리턴하는 방식으로
    시간과 공간 효율성이 높기 때문이다.
    
    (캐싱하는 최대값인 127은 -XX:AutoBoxCacheMax = option이라는 jvm 옵션을 통해서 더 늘릴 수 있다.)
    
    ---
    
    java11 api document, Integer class, Integer(int value), valueOf(int) 설명 부분 
    
### 왜 어떤 숫자를 가진 Integer는 == 비교가 되고 다른 Integer는 안될까?
    
    IntegerCache 범위에 있는 Integer들은 모두 같은 객체를 가르키기 때문에 ==비교가 가능하다.
    반면 IntegerCache범위 밖에 Integer들은 같은 값을 갖더라도 서로 다른 객체를 가르키기 때문에 == 비교가 불가능하다.
    
    ---
    
    java11 api document, Integer class, valueOf(int), IntegerCache class
    
### Integer a = 5는 내부적으로 어떻게 Integer 객체를 생성하는 것일까?
    
    Integer a = 5 는 컴파일 후 다음과 같이 변한다. Integer a = Integer.valueOf(5);
    
### 래퍼 클래스는 같은 값을 갖고 있다면 같은 객체를 가리켜도 상관없을까?
    
    Integer 클래스 같은 경우 서로 다른 객체는 모든 부분이 같고 감싸고 있는 int value만이 다르다.
    
    또 이 int value는 final로 써 재할당이 불가능하기 때문에, 같은 int value를 갖고 있는 Integer 타입 변수는 서로 같은 객체를 가리켜도 상관없다.
    
    그래서 valueOf(int value)가 객체를 미리 캐싱하는 것이 가능한 것 같다.
    
### wrapper class는 == 비교로 정확한 비교를 할 수 있을까??
    
    할 수 없다.
    
    wrapper class도 객체기 때문에 == 비교를 하면 주소값을 비교하게 되서 정확한 비교를 할 수 없다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 492p, 2016
    
### wrapper class는 < > ≤ ≥ 비교나 + = * / 등의 연산은 왜 잘 되는 것일까?
    
    컴파일러가 언박싱을 진행하기 때문이다. 
    
    ---
    
    직접 실험
    
### 언박싱, 오토박싱이란?
    
    오토박싱은 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것을 말하고,
    반대로 변환하는 것을 언박싱이라고 한다.
    
    ---
    
    남궁 성, 자바의 정석 3rd Edition, 1판 2쇄, 도우출판, 495p, 2016

### primitive type과 wrapper type을 같이 연산하면 안 좋은 이유는 뭘까?
    
    예를 들어 다음과 같은 코드가 있을 때
    
    public static void main(String[] args) {
      Long sum = 0L;
      for (long i = 0; i < 1000000; i++) {
      	sum += i;
      }
    }
    
    실제 연산 과정은 먼저 sum이 primitive type인 long으로 형변환되는 언박싱이 이루어진다.
    
    이후 i와 더한 후 이 값이 다시 Long으로 형변환되는 박싱이 이루어지고 sum에 할당이 된다.
    
    이런 식으로 불필요한 과정이 연산에 추가가 되므로 성능이 느려질 수 밖에 없다.  
    
    또한 1~ 100만까지의 Long 객체가 힙 메모리에 생기게 되므로 가비지 컬렉팅을 유발하여 성능이 느려진다.
    
    실제로 다음과 같은 코드과 비교했을 때 성능이 5배 정도 느리다.
    
    public static void main(String[] args) {
        long sum = 0L;
        for (long i = 0; i < 1000000; i++) {
            sum += i;
        }
    }
    
    ---
    
    [https://sas-study.tistory.com/407](https://sas-study.tistory.com/407)
    
    [https://medium.com/@bpnorlander/java-understanding-primitive-types-and-wrapper-objects-a6798fb2afe9](https://medium.com/@bpnorlander/java-understanding-primitive-types-and-wrapper-objects-a6798fb2afe9)