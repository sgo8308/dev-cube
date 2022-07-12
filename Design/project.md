# 회원가입 / 로그인
## 인증 / 인가
### 인증이란?
    
    인증이란 사용자가 등록된 사용자인지를 확인하는 것이며, 사용자가 보낸 메시지가 변조되지 않은 그대로의 것인지를 확인하는 것이다.
    
    첫번째 작업은 아이디와 패스워드를 가입 목록에서 대조하는 방식을 사용하고 두번째 작업은 메시지 인증 부호, 디지털 서명 부호 등이 사용된다.
    
    ---
    
    [https://terms.naver.com/entry.naver?docId=855723&cid=42346&categoryId=42346]
    
### 인가란?
    
    사용자가 특정한 서비스를 사용할 수 있는 권한이 있는지 확인하는 절차. 예를 들어 어떤 글을 수정한다거나 관리자 페이지에 들어간다거나 하는 것 들이 모두 권한이 필요하다. 이러한 일들을 할려면 서버로부터 인가를 받아야 한다.
    
    ---
    
    [https://terms.naver.com/entry.naver?docId=849375&cid=50371&categoryId=50371]
## 패스워드 암호화
### 패스워드는 왜 DB에 암호화해서 저장해야할까?

      패스워드는 사용자 인증할 때 말고는 사용되지 않는다.

      이 인증은 복호화가 불가능한 암호화 방식을 사용하더라도 구현할 수 있다.

      이렇게 해두면 해커한테 DB가 털리더라도 암호화된 패스워드는 해독이 불가능하기 때문에 안전하다.

      여러 웹사이트에서 비밀번호 찾기를 했을 때 그대로 비밀번호를 알려주는게 아니라 새 비밀번호로 바꾸도록하는 이유는 비밀번호가 암호화되어 있기 때문이다.

      또한 법으로도 암호화하도록 정해져 있다.

### 단순 해시 함수로는 복호화가 불가능하더라도 비밀번호가 안전하지 않다 왜그럴까?

      왜냐하면 해커는 다이제스트를 얻어 브루트포스 공격을 통해 원문을 알아낼 수 있기 때문이다.

      브루트포스 공격은 영문 + 숫자 조합 같이 패스워드로 그럴듯한 값들을 무차별적으로 해시 함수에 대입하여 탈취된 다이제스트와 동일한 값을 갖는지 비교하여 원문을 알아내는 방법이다.

      해시 함수가 아무리 빠르더라도 이 작업에는 시간이 많이 걸린다. 따라서 레인보우 테이블이라는 것을 같이 사용하는데 이것은 그럴듯한 비밀번호와 그것의 다이제스트가 무수히 많이 저장되어 있는 테이블이다.

      이 테이블과 탈취한 다이제스트를 이용하여 브루트포스 공격을 더 빠르게 성공시킬 수 있다.

### 단순 해시 함수를 보완하는 방법은 무엇이 있을까?
    - 키 스트레칭(key stretch) 방법

      패스워드를 해싱 함수를 통하여 다이제스트를 얻어낸다.

      이 다이제스트를 해싱 함수를 통하여 새로운 다이제스트를 얻어낸다.

      이런 식으로 여러 번 반복해서 최종 다이제스트를 얻어서 저장하는 방법이다.

      이 방법의 장점은 원문으로부터 최종 다이제스트를 얻기까지 걸리는 시간을 늘리는데 있다.

      해커가 단순 해시 함수를 통해 1초에 50억번 대입해볼 수 있었다면, 키 스트레칭이 적용된 다이제스트를 사용하면 1초에 5번밖에 대입하지 못하게 만들 수 있다.

    - 솔트(salt) 알고리즘

      패스워드에다가 임의적인 값을 덧붙힌 후 해시 함수를 적용하는 방법이다.

      솔트가 붙은 비밀번호는 일반적인 비밀번호와 완전히 달라지기 때문에 브루트포스 공격으로 뚫기가 쉽지 않으며 레인보우 테이블을 무용지물로 만들 수 있다.

      혹시나 솔트가 패스워드와 같이 털리더라도 레인보우 테이블이 무용지물이 되었기 때문에 단순 해시 함수를 사용하는 것보다는 효율적이다.

      이 솔트가 같이 털리지 않기 위해 다른 곳에 저장한다면 그것은 페퍼라고 부른다.


    일반적으로 키 스트레칭과 솔트를 섞어서 사용한다.
    
    ---
    
    [https://d2.naver.com/helloworld/318732](https://d2.naver.com/helloworld/318732)


# 테스트
### 테스트 코드의 장점과 단점은?
    - 장점
        - 잘 작동하는 깔끔한 코드를 얻을 수 있다.
            
            테스트를 쉽게하려면 프로덕션 코드를 테스트하기 쉽게 짜야 한다. 테스트하기 쉽게 짜다보면 코드가 깔끔해진다. 또 API를 사용하는 입장이 되기 때문에 지저분한 인터페이스는 깔끔하게 짜게 된다.
            
            또한 테스트 코드라는 안전망이 있기 때문에 과감하게 리팩토링 할 수 있다.
            
        - 개발자의 시간을 아껴 준다.
            
            무언가 조금씩 바뀔 때마다 모든 테스트를 수동으로 하는 것은 시간이 매우 많이 든다.
            
        - 테스트를 통해 API를 더 잘 이해할 수 있다.
            
            일종의 문서 역할을 할 수 있다. 처음 코드를 보는 개발자들이 테스트 코드를 통해 코드의 동작을 수월하게 이해할 수 있다.
            
    - 단점
        - 매우 귀찮다. 그래서 프로그래머들이 하기 싫어한다.
        - 테스트 코드 또한 관리에 대상이 되어 시간이 들 수 있다.
        - 테스트 코드가 많아질수록 모든 테스트를 수행하기 위해 걸리는 시간도 증가한다. 단위 테스트가 3500개 정도 작성된 상황에서 로컬은 3분 CI 환경은 10분 정도가 걸렸다고 한다.
    
    ---
    
    [https://galid1.tistory.com/783](https://galid1.tistory.com/783)
    
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

# CI / CD
### CI/CD란?
    
    시장과 고객의 요구에 빠르게 반응해서 제품을 출시 업데이트하는 것이 중요하다.
    
    CI/CD는 이를 위해 어플리케이션 개발부터 배포까지 모든 단계들을 자동화를 통해서 더 효율적이고 빠르게 사용자에게 빈번이 배포할 수 있게 만드는 것을 말한다.
    
    - CI(Continuous Integration)
        
        CI는 개발자가 코드를 머지했을 때 빌드가 잘 되는지, 모든 테스트를 통과하는지를 자동으로 해주는 것을 의미한다. 또한 CI는 개발자가 의식적으로 작은 단위로 커밋하는 것을 포함한다. 
        
        이를 통해 버그를 조기에 발견할 수 있고 거대한 머지를 처리하는데 드는 시간을 아낄 수 있다.
        
        - 세부 단계
            1. 개발자가 코드를 작성해서 PR을 올린다.
            2. 리뷰가 통과되면 머지를 시도한다.
            3. 머지된 코드는 빌드와 테스트를 위한 서버에서 자동으로 내려받는다.
            4. 빌드와 테스트를 진행하고 만약 실패하면 개발자에게 알려준다.
            5. 비드와 테스트를 진행하고 만약 성공하면 CD단계로 넘어간다. 
    - CD(Continuous Integration)
        
        CI를 통해 빌드와 테스트가 끝난 코드를 자동으로 릴리스해주는 것을 의미한다.
        
    
    ### 전체 과정
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/146e295e-654e-4803-8e43-63a563aeeebd/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e854f37-d3a5-4c17-99f1-9a8c84018d3a/Untitled.png)
    
    ---
    
    [https://www.youtube.com/watch?v=0Emq5FypiMM](https://www.youtube.com/watch?v=0Emq5FypiMM)- 드림코딩 CI 편
    
    [https://engineering.linecorp.com/ko/blog/build-a-continuous-cicd-environment-based-on-data/](https://engineering.linecorp.com/ko/blog/build-a-continuous-cicd-environment-based-on-data/)
    
    질문
    
    1. 머지했을 때 빌드 또는 테스트가 실패하면 머지가 실패하는 것인가요 아니면 머지는 그대로 되어 있는 걸까요? 
    2. 만약 머지가 그대로 되어 있다면 머지를 다시 취소하고 문제 있는 부분을 해결한 후 다시 머지해야 하는 걸까요? 실제로 어떤 과정일지 궁금합니다.
    3. develop 브랜치가 메인 브랜치이고 feature 브랜치가 있다면 보통은 feature 브랜치에서 develop 브랜치로 머지할 때만 CI가 동작해서 빌드하고 테스트해보는 건가요? 아니면 로컬 컴퓨터에서 원격에 feature 브랜치로 push할 때도 CI가 동작하도록 하나요?
### CI/CD 툴은 뭐가 있고 각각의 장단점은?
    
    고려 사항 : 예산, 프로젝트 요구사항
    
    선택시 고려 사항
    
    1. 프로젝트에 사용된 모든 언어를 지원해야 한다.
    2. 컨테이너 오케스트레이션 시스템을 지원하는게 좋음
    3. 문서가 잘되어 있는지, 지원 포럼이 잘 운영되는지
    4. 팀에 각 개발자가 잘 이해하고 있는지
    
    ---
    
    [https://loosie.tistory.com/789](https://loosie.tistory.com/789)
    
    [https://www.lambdatest.com/blog/travis-ci-vs-jenkins/](https://www.lambdatest.com/blog/travis-ci-vs-jenkins/)
    [https://www.lambdatest.com/learning-hub/cicd](https://www.lambdatest.com/learning-hub/cicd)
    
    [https://www.devopsauthority.tech/2021/02/09/which-is-better-github-actions-circle-ci/](https://www.devopsauthority.tech/2021/02/09/which-is-better-github-actions-circle-ci/)
