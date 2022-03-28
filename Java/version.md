### 버전과 관련해서 LTS란 무엇인가요? 자바의 LTS는 무엇무엇이 있나요? ★
    
    LTS는 Long-term support란 의미로 일반적인 경우보다 장기간에 걸쳐 지원하도록 특별히 고안된 소프트웨어의 버전 또는 에디션이다.
    
    (여기서 지원이란 기능 업데이트, 버그&크래시 수정이나 보안성 개선에 대한 업데이트를 말한다.)
    
    Oracle jdk 경우 LTS 버전이 있으며 open jdk는 LTS가 없다.
    
    자바의 LTS 버전은 업데이트할 때 안정성, 보안 및 성능 향상의 업데이트만 진행한다.
    
    즉 자바의 LTS 버전은 오래 지원해주고 안정적이기 때문에 최신 기능을 빨리 써보길 원하는 개발자들 외에 안정적으로 프로덕트를 운영해나가고 싶은 대부분의 회사에서 많이 사용된다.
    
    자바의 LTS에는 7, 8, 11, 17 버전 등이 있다.
    
    ---
    
    [https://en.wikipedia.org/wiki/Long-term_support](https://en.wikipedia.org/wiki/Long-term_support)
    
    [https://www.oracle.com/java/technologies/java-se-support-roadmap.html](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)
    
    [https://blogs.oracle.com/javamagazine/post/java-long-term-support-lts](https://blogs.oracle.com/javamagazine/post/java-long-term-support-lts)
    
    [http://taewan.kim/post/openjdk/](http://taewan.kim/post/openjdk/)  - openjdk는 lts를 지원하지 않는다.
    
    [https://www.baeldung.com/oracle-jdk-vs-openjdk](https://www.baeldung.com/oracle-jdk-vs-openjdk) - openjdk는 lts를 지원하지 않는다.
    
### 왜 LTS버전과 그렇지 않은 버전을 나눌까? 왜 그런 식으로 개발할까?
    
    최신 기능을 빨리 써보고 싶은 개발자들과, 안정적으로 오래 쓸 수 있는 버전을 원하는 개발자들의 수요를 맞추기 위해서인 것 같다.
    
### 패치와 서비스 릴리스의 차이는?
    
    패치는 특정한 버그나 자잘한 이슈를 해결한 버전인 것 같다.
    
    서비스 릴리스는 좀더 광범위한 버그의 처리나 기능 추가를 말하는 듯하다.
    
    ---
    
    [https://www.ibm.com/support/pages/what-difference-between-patch-and-service-release#:~:text=Service Releases address a wide,is focused on specific issues](https://www.ibm.com/support/pages/what-difference-between-patch-and-service-release#:~:text=Service%20Releases%20address%20a%20wide,is%20focused%20on%20specific%20issues).
    
    [https://www.lawinsider.com/dictionary/patch-release](https://www.lawinsider.com/dictionary/patch-release)
    
### 가장 많이 쓰이는 자바 버전은 무엇일까?
    
    2019년 기준으로 자바 8을 가장 많이 사용하는 것 같다.
    
    ---
    
    [https://www.jetbrains.com/lp/devecosystem-2019/java/](https://www.jetbrains.com/lp/devecosystem-2019/java/)
    
### OpenJDK는 무엇인가?
    
    오라클이 운영하는 java.net과 오라클에 의해서 OCTLA 체결과 검증을 받은 회사들이 같이 개발하는 오픈소스 JDK이자 공식적인 자바 참조 구현체이다.
    
    오라클을 포함한 모든 JDK는 OpenJDK를 근간으로 개발되고 OpenJDK가 발표되고 6개월 간 오라클은 OracleJDK에 적용되는 모든 패치를 OpenJDK에도 똑같이 적용하므로 6개월 간 OracleJDK와 OpenJDK는 같은 JDK라고 할 수 있다.
    
    ---
    
    [http://taewan.kim/post/openjdk/](http://taewan.kim/post/openjdk/)