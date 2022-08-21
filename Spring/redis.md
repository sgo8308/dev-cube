### spring.session.store-type=redis을 application properties에 설정해주면 스프링 부트는 어떻게 Redis를 이용한 Session으로 변경할까?
    
이 설정은 기본적으로 @Configuration이 붙은 클래스 위에 @EnableRedisHttpSession을 붙힌 것과 동일하게 동작한다.

이 경우 Filter를 구현하는 `springSessionRepositoryFilter` 를 빈으로 등록한다. 그리고 이 필터는 HttpSession의 구현체를 Spring Session이 제공하는 구현체로 변경한다.

---

[참고](https://docs.spring.io/spring-session/docs/2.5.6/reference/html5/guides/boot-redis.html#:~:text=Session%20store%20type.-,Under%20the%20hood%2C,-Spring%20Boot%20applies)
