## 스프링 부트

### 스프링 부트란 대체 무엇일까?

    스프링의 확장판이다.

    스프링 기술을 그대로 사용할 수 있으며 스프링 앱 개발을 훨씬 쉽게 만들어준다.

    ---

    https://www.interviewbit.com/blog/spring-vs-spring-boot/#:~:text=To sum up-,Spring Boot contains all of the functionality of the standard Spring framework while also making application development much easier.,-When compared to

### 사용해야 하는 이유는?

    스프링에 복잡한 설정들을 관례에 따라 간결하게 설정할 수 있도록 해준다.
    
    자동으로 호환되는 라이브러리들을 찾아준다.
    
    톰캣 서버를 내장하고 있어 단독적으로 서버 어플리케이션 실행이 가능하다.
    
    ---
    
    https://www.ibm.com/cloud/learn/java-spring-boot#:~:text=Java Spring Boot

### 왜 나왔을까?

    빠르게 스프링을 이용한 앱 개발을 시작할 수 있게 만들어 새로운 유저들을 스프링에 많이 유입시키기 위해서 
    
    ---
    
    https://spring.io/blog/2013/08/06/spring-boot-simplifying-spring-for-everyone/#:~:text=Spring Boot aims to make it easy to create Spring-powered%2C production-grade applications and services with minimum fuss. It takes an opinionated view of the Spring platform so that new and existing users can quickly get to the bits they need

### AutoConfiguration은 내부적으로 어떻게 동작할까? 

#NoMagic #조건 #클래스패스 #프로퍼티

- 간결한 설명

여러 조건들이 붙은 @Configuraiton 파일들을 대량으로 등록해준다. 어떤 클래스가 클래스 패스에 있을 때만 설정 파일로 인식되거나 사용자가 직접 정의하지 않았을 때만 빈을 생성하거나 하는 조건이 붙어 있다.

- 상세한 설명 

autoconfigure 라이브러리의 META-INF/spring 아래에 있는 org.springframework.boot.autoconfigure.AutoConfiguration.imports 파일을 읽어서 그 안에 명시된 AutoConfiguration 파일들을 이용해서 빈을 생성한다.

AutoConfiguration 파일들은 일반적인 @Configuration이 붙어 있는 설정 파일과 동일하며 대신에 여러가지 조건들이 @Conditional의 형태로 덕지덕지 붙는다.

위 조건들을 다 만족해야 이 Configuration 파일이 평가되며 그 안에 정의된 빈들이 만들어지고 ApplicationContext에 등록이 된다.

주로 어떤 클래스가 클래스 패스에 존재할 때(@**ConditionalOnClass(RedisClient.class))** 어떤 프로퍼티가 어떤 value를 갖고 있을 때**(@ConditionalOnProperty(name = "spring.redis.client-type", havingValue = "lettuce”)** 어떤 빈이 존재하지 않을 때 **(@ConditionalOnMissingBean(ClientResources.class))** 등이 쓰인다.

---

[https://www.marcobehler.com/guides/spring-boot-autoconfiguration](https://www.marcobehler.com/guides/spring-boot-autoconfiguration)
### 단점은?

    사용되지 않는 많은 의존성들이 추가될 수 있고 이는 배치 파일의 크기를 키운다.
    
