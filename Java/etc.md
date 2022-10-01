### assert 예약어를 설명해주세요.
    
어떤 불리언 식이 참이 아니면 더 이상 진행하지 못하게 하고 참이면 진행하게 해서 신뢰성 있는 개발을 도와주는 것. 

예를 들어 어떤 메소드를 사용할 때 인자값으로 옳바른 값이 오지 않았다면 더 이상 진행시키지 않고 에러를 내뱉도록 하고 싶다면 다음과 같이 하면 된다.

```java
private String calculateAge(int age){
    assert age > 0;
    //나이를 계산하는 로직 실행
}
```

이런 식으로 사용할 경우 만약 나이가 0살을 넘지 않는다면 AssertionError가 발생한다.

위와 같이 사전에 인자를 체크하는데 사용할 수도 있고, 메소드가 다 실행된 후 결과값을 체크하는데 사용할 수도 있다.

개발시에만 사용되고 실제 production code에는 빠지기 때문에 assert 안에 실제로 프로그램 흐름에 필요한 메소드 같은 것들 집어넣으면 실제 운영 때 이 메소드가 실행이 안되서 문제가 생길 수 있다.

따라서 이렇게 하지 말고

```java
assert doSomething(); // doSomething이 프로그램 실행 흐름 상 꼭 필요한 메소드
```

이렇게 하면 된다.

```java
boolean bool = doSomething();
assert bool;
```
  

### UUID란?
    
Universal Unique Identifier의 약자로 전세계적으로 유일한 값이라는 의미이다. 물론 겹칠 확률이 완전히 0은 아니지만 0으로 봐도 무방할 정도로 겹칠 확률이 낮다.

32개의 문자와 4개의 대시('-')가 추가된 랜덤하게 생성된 128bit가 문자열이라고 보면 된다.

---

[https://en.wikipedia.org/wiki/Universally_unique_identifier](https://en.wikipedia.org/wiki/Universally_unique_identifier)