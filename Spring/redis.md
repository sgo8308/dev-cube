### spring.session.store-type=redis을 application properties에 설정해주면 스프링 부트는 어떻게 Redis를 이용한 Session으로 변경할까?

이 설정은 기본적으로 @Configuration이 붙은 클래스 위에 @EnableRedisHttpSession을 붙힌 것과 동일하게 동작한다.

이 경우 Filter를 구현하는 `springSessionRepositoryFilter` 를 빈으로 등록한다. 그리고 이 필터는 HttpSession의 구현체를 Spring Session이 제공하는 구현체로 변경한다.

또 Spring Session Redis를 사용할 때 원래는 다음과 같이 설정을 해주어야 한다.

```java
@EnableRedisHttpSession 
public class Config {

	@Bean
	public LettuceConnectionFactory connectionFactory() {
		return new LettuceConnectionFactory(); 
	}

}
```
레디스 서버와의 Connection을 가져올 ConnectionFactory를 빈으로 등록해주어야 한다. 그러나 이 부분 또한 Spring Boot가 자동으로 빈을 등록해주며 spring-boot-starter-data-redis를 사용하면 자동으로 Lettuce 클라이언트를 이용한 ConnectionFactory를 빈으로 만든다.

---

[참고](https://docs.spring.io/spring-session/docs/2.5.6/reference/html5/guides/boot-redis.html#:~:text=Session%20store%20type.-,Under%20the%20hood%2C,-Spring%20Boot%20applies)
[참고](https://docs.spring.io/spring-boot/docs/2.5.12/reference/htmlsingle/#features.nosql.redis.connecting:~:text=7.12.1.-,Redis,-Redis%20is%20a)