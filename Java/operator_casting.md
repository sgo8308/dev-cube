# 연산자
### -1 * 50을 간단하게 나타내면?
    
    -50 , -는 단항 마이너스 연산자라고 한다.
    
    단항 플러스 연산자와 마이너스 연산자를 이용해 다음과 같은 연산이 가능하다.
    
    +5 - -5
    
    답은 10이다.
    
### ~는 뭐하는 연산자일까?
    
    비트 값의 0을 1로 1을 0으로 바꾸는 연산자이다.
    
    ~5는 -6이 된다. 
    
### boolean에 연산을 적용할 때 && 와 & 그리고 || 와 | 연산자의 차이는 뭘까? ★
    
    &&같은 경우 좌항에 값이 false라면 우항은 보지도 않고 넘어간다.
    
    & 같은 경우 좌항이 false더라도 우항을 확인한다.
    
    || 와 |의 차이도 비슷한 맥락이다.
    
### 모든 참조 자료형에 + 연산자를 할 수 있을까?
    
    그렇다.
    
    참조 자료형에 + 연산자를 수행하면 해당 클래스에 있는 toString() 메소드의 결과와 그 연산자 뒤에 있는 문자열을 더한다.
    
### ^이 연산자는 뭘까?
    
    boolean간 연산에 쓰일 때는 좌항과 우항 둘 다 true거나 둘 다 false일 때는 false이고,
    좌항과 우항이 다르면 true를 주는 연산자이다.
# 형변환
### 형변환을 왜 해야할까?
    
    같은 타입끼리만 연산이 가능하기 때문에,
    타입이 다른 애들끼리는 형변환을 통해 타입을 맞춘 후 연산해야한다.
    
    예를 들어 int와 float을 계산한다고 해보자.
    
    int와 float은 둘 다 4byte이지만 서로 저장하는 방식, 연산하는 방식이 다르기 때문에
    이 방식을 통일시켜준 후 연산해야 한다.
    
    byte와 short의 경우에는 서로 저장하는 크기가 다르기 때문에 연산했을 때,
    예상하지 못한 값이 나올 수 있다.
    
    예를 들어 byte -1과 short 1을 더하면 0이 나와야 한다.
    
    short  1은 0000000000000001이고
    byte  -1은         11111111이다.
    
    이렇게 다른 메모리 크기의 값을 연산할 수 있는지도 모르겠지만,
    연산이 가능하다고 해도 답은 00000000000100000000으로 0이 아니다.
### 타입이 다른 정수나 실수끼리는 어떻게 연산을 할까?
    
    연산 전에 더 큰 타입으로 형변환을 한 후 연산을 한다.
    
    왜냐하면 더 작은 타입으로 형변환을 할 경우 데이터를 잃을 가능성이 있기 때문이다.
    
    int + long → long + long
    
    int + float → float + float
    
    float + double → double + double
    
    특이한 것은 int보다 작은 값들을 계산할 때는 모두 int로 형변환한 후 계산한다는 것이다.
    
    byte + short → int + int
    
    왜냐하면 byte와 short의 표현 범위가 좁아서 연산중에 오버플로우가 발생할 가능성이 높기 때문이다.

    ```
### 타입 프로모션과 타입 캐스팅은 무엇일까? ★
    
    타입 프로모션은 Widening Conversion이며, 타입 캐스팅은 Widening Conversiong , Narrowing Conversion 둘다 가능하지만
    주로 Narrowing Conversion으로 주로 쓰이기 때문에 Narrowing Conversion이라고 불리는 것 같다.
    
    타입 프로모션은 컴파일러에 의해 암묵적으로 일어나지만, 타입 캐스팅은 명시적으로 프로그래머가 캐스팅 오퍼레이터를 사용함으로써 일어난다.
    
    타입 프로모션은 데이터를 잃지 않지만, 타입 캐스팅은 데이터를 잃는다(Narrowing 일 때).
    
    ---
    
    [https://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.16](https://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.16)
    
    [https://www.geeksforgeeks.org/difference-between-type-casting-and-type-conversion/](https://www.geeksforgeeks.org/difference-between-type-casting-and-type-conversion/)
    
    [https://www.examtray.com/java/last-minute-java-type-casting-or-type-conversion-or-type-promotions-tutorial](https://www.examtray.com/java/last-minute-java-type-casting-or-type-conversion-or-type-promotions-tutorial)