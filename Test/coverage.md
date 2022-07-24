### 코드 커버리지란?

    코드 커버리지란 테스트 코드가 실제 코드에 얼마나 많은 부분을 실행했는지를 나타내는 것이다.
    
    화이트 박스 테스팅이다.
    
    ---
    
    [https://www.lambdatest.com/blog/code-coverage-vs-test-coverage/](https://www.lambdatest.com/blog/code-coverage-vs-test-coverage/)

### 테스트 커버리지란?

    기능 요구 사항을 만족하는 정도이다.
    
    수동으로 블랙박스 테스팅을 통하여 측정된다.
    
    ---
    
    [https://www.lambdatest.com/blog/code-coverage-vs-test-coverage/](https://www.lambdatest.com/blog/code-coverage-vs-test-coverage/)

### 코드 커버리지를 측정하는 이유는?

    코드 커버리지는 내가 작성한 코드에 대해 놓치는 부분 없이 테스트를 작성할 수 있게 도와준다. 하지만 애초에 내 코드에 문제가 있었다면 코드 커버리지가 100%를 달성한다고 해도 버그가 생길 수 있다. 
    
    또한 코드에는 가장 중요하고 위험 부담이 큰 부분이 있고, 사소한 부분이 있다. 
    
    따라서 코드 커버리지를 100%를 달성하는데 시간을 쓰기 보다는 중요한 부분에 집중해서 코드 커버리지는 일정 수준만 유지하고 나머지 시간에는 내가 생각하지 못한 부분이 없을까 생각하는데 시간을 쏟는게 더 좋다. 
    
    ---
    
    [https://tecoble.techcourse.co.kr/post/2020-10-24-code-coverage/](https://tecoble.techcourse.co.kr/post/2020-10-24-code-coverage/)
    
    [https://dublin-java.tistory.com/57](https://dublin-java.tistory.com/57)

### 코드 커버리지의 종류는?
    - 구문/라인 커버리지
        
        테스트 케이스가 코드 한 줄이 한 번이상 실행하면 충족된다.
        
    - 조건 커버리지
        
        테스트 케이스가 모든 조건식 안에 각각의 조건들에 true/false를 만족하면 충족된다.
        
    - 결정 커버리지
        
        테스트 케이스가 모든 조건식의 true, false를 만족하면 충족된다.
        
    
    웬만하면 기본으로 라인 커버리지를 많이 사용한다. 조건과 결정은 분기문에서 주로 쓰이므로 커버하지 못하는 부분이 많다. 
    
    - 결정 커버리지는 왜 필요할까?
        
        어떤 코드 안에 있는 조건문이 true인 경우만 테스트되면 false인 경우에 문제가 생기는 경우를 테스트를 통해 잡아낼 수 없다.
        
    - 조건 커버리지는 왜 필요할까?
        
        테스트에서 커버되지 않은 조건에 상태를 변화시키는 메소드가 있다면 그 메소드로 인해 프로그램의 문제가 생길 수 있는데 이 문제를 테스트로 발견할 수 없다.
        
        ex.
        
        ```java
        if( true || statusChange()){}
        ```
        
        이 때 statusChange()가 false를 반환할 때는 어떤 문제가 생긴다고 가정하자.
        
        테스트 케이스가 statusChange()에서 false를 반환하는 경우를 테스트 못한다면 이 문제를 발견할 수 없다. 하지만 조건 커버리지가 만족됐다면 이 경우도 테스트 된 것이기 때문에 발견이 가능하다.

### 코드 커버리지를 높다는 것은 좋은 테스트 suit를 갖고 있다는 의미일까?
    
    코드 커버리지가 낮다는 것이 테스트가 충분치 않다는 것을 보여줄 수 는 있지만 코드 커버리지가 높다고 해서 좋은 테스트 suit을 갖고 있다는 것은 아니다.
    
    왜냐하면 
    
    첫째로, 커버리지가 100%라고 해서 모든 결과를 테스트한 것이 아니다.
    
    - 한 메소드가 내부에서 어떤 값을 변경하는 작업과, 어떤 값을 return하는 두 작업을 한다고 하자. 테스트로는 return된 값만 테스트하더라도 어떤 값을 변경하는 작업도 같이 실행시켰다면 커버리지는 100%가 나온다. 하지만 변경하는 작업은 실행된 것이지 테스트 된 것은 아니다.
    - 외부 라이브러리의 커버리지는 포함하지 않는다.
### 코드 커버리지는 어떻게 사용하는 것이 좋을까?
    
    단지 지표로서만 사용하는 것이 좋다. 좋은 테스트를 작성하는 것은 이미 어렵다. 
    
    코드 커버리지를 60% 70% 이런 식으로 목표로 잡아버리면 개발자로 하여금 정작 중요한 테스트를 작성하기 보다는 필요없더라도 이 커버리지에 맞추기 위한 테스트에 집중하게 만들 수 있다.
    
    하지만 이것은 좋은 테스트에 대한 열의가 있는 팀에 해당하는 것 같고 그렇지 않은 팀에 대해서는 커버리지로 강제성을 두는 것이 나름 효과적일 수 있을 것 같다.
    
    ---
    
    단위 테스트 15p
### 코드 커버리지 툴의 종류와 장단점

    JaCoCo, OpenClover, JCov 등이 있다.
    
    JaCoCo가 활발하게 개발이 되고 있고 가장 많이 사용되기 때문에 문서가 풍부하다. 
    
    또한 유일하게 Instruction Coverage를 제공하고 있어 정확한 커버리지 측정이 가능하다. (instruction coverage는 bytecode를 기준으로 측정하기 때문에 소스코드 포맷에 좌우되는 line coverage 방식보다 정확하다.)
    
    ---
    
    [https://openclover.org/doc/manual/4.2.0/general--comparison-of-code-coverage-tools.html](https://openclover.org/doc/manual/4.2.0/general--comparison-of-code-coverage-tools.html)
    
    [https://bottom-to-top.tistory.com/37](https://bottom-to-top.tistory.com/37)

### JaCoCo란?

    Java 코드의 커버리지를 체크하는 라이브러리.
    
    테스트코드를 돌리고 그 커버리지 결과를 눈으로 보기 좋도록 html이나 xml, csv 같은 리포트로 생성한다. 그리고 테스트 결과가 내가 설정한 커버리지 기준을 만족하는지 확인하는 기능도 있다.
    
    ---
    
    [https://tecoble.techcourse.co.kr/post/2020-10-24-code-coverage/](https://tecoble.techcourse.co.kr/post/2020-10-24-code-coverage/)

