### 직렬화란 무엇이고 왜 사용할까?
    
직렬화란 객체를 영구적으로 저장하거나 전송하기 위해서 특정한 형태로 변환하는 것을 말한다. 예를 들어 Json의 형태로 객체를 변환하는 것도 직렬화라고 할 수 있다.

---

[https://techblog.woowahan.com/2550/](https://techblog.woowahan.com/2550/)
    
### 자바 직렬화란 무엇이고 왜 사용할까?
    
자바에서 직렬화는 객체를 바이트의 형태로 변환하여 저장하거나 전송할 수 있도록 만드는 것을 말한다. 이렇게 변환된 객체는 다른 자바 시스템에서 다시 객체의 형태로 변환하여 사용할 수 있다.

---

[https://techblog.woowahan.com/2550/](https://techblog.woowahan.com/2550/)
    
### 자바 직렬화의 장점과 단점은 무엇일까?
- 장점
    1. 쉽고 단순하다. Json 형태로 변환시키는 것은 추가적인 라이브러리를 이용해야 쉽게 변환이 가능하지만 자바 직렬화는 단순히 직렬화를 원하는 객체 클래스에 Serializable을 붙혀주고 ObjectOutputStream을 이용하면 된다.
- 단점
    1. 변경에 취약하다.  매우 엄격하기 때문에 클래스에 변화가 일어날 경우 역직렬화가 실패할 가능성이 많다. 예를 들어 serialVersionUID를 설정하지 않을 경우 클래스에 멤버 변수가 추가되거나 삭제되면 역직렬화에 실패한다. serialVersionUID를 동일하게 유지하면 이 부분은 괜찮지만 변수의 타입이 달라질 경우도 역직렬화에 실패한다.
    2. 용량이 커진다. 객체 안에 내용물에 대한 클래스 타입 정보들을 저장해야 하기 때문에 Json을 이용한 방식에 비해서 최소 2배부터 10배 까지 커질 수 있다.
    3. 자바 시스템끼리만 사용이 가능하다. 따라서 다른 언어를 이용해서 스크립트를 이용해서 여러 가지 처리를 하고 싶어도 불가능에 가깝다
    

---

[https://techblog.woowahan.com/2550/](https://techblog.woowahan.com/2550/)
[https://techblog.woowahan.com/2551/](https://techblog.woowahan.com/2551/)
        
### Serializable은 왜 필요할까 모두 직렬화 가능하면 되지 않을까?
    
모두 직렬화가 가능하면 보안상 문제가 있다. 어떤 자바 오브젝트를 직렬화해서 전송한다고 할 때 그 안에서 참조되고 있는 직렬화되면 안되는 민감한 오브젝트가 같이 전송될 수 있다. 만약 지금처럼 직렬화가 필요한 것은 선택적으로 Serializable을 선언하도록 한다면 위와 같은 문제를 사전에 방지할 수 있다.

---

[https://stackoverflow.com/questions/11600213/why-doesnt-java-lang-object-implement-the-serializable-interface#:~:text=are themselves serializable.-,In other words,-%3A all classes](https://stackoverflow.com/questions/11600213/why-doesnt-java-lang-object-implement-the-serializable-interface#:~:text=are%20themselves%20serializable.-,In%20other%20words,-%3A%20all%20classes)
    
### 자바 직렬화를 사용할 때 어떤 문제가 있고 어떻게 해결해야할까?
    
단점에서도 나오지면 역직렬화가 실패할 가능성이 꽤 크다.

따라서 역직렬화가 실패했을 경우를 항상 대비해서 예외처리를 잘해 놓아야 한다.