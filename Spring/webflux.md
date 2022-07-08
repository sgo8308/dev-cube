### back pressure란?
    [세면대 아래로 물이 흐를 때 하부 배관의 구부러진 곳에서 생기는 역방향의 저항을 back pressure라고 한다.](https://m.cafe.daum.net/harleydavidson/1tiq/64063#:~:text=%EC%9C%A0%EC%B2%B4%EA%B0%80%20%ED%9D%90%EB%A5%B4%EB%8A%94%C2%A0%EB%B0%A9%ED%96%A5%EA%B3%BC,%EC%97%AD%EC%95%95%27%EC%9D%B4%EB%9D%BC%EA%B3%A0%EB%8F%84%20%ED%95%A9%EB%8B%88%EB%8B%A4.)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a642b33-d03a-4572-a943-e8ff7c0b2b02/Untitled.png)
    
    반응형 프로그래밍에서는 데이터의 흐름이 공급이 되고 공급된 데이터를 처리하는 소비자가 있다. 공급자가 데이터를 공급하는 속도보다 소비자가 데이터를 처리하는 속도가 느릴 때가 있다. 이 때 데이터가 빠르게 처리되지 않으므로 데이터가 계속 쌓이다가 OutOfMemoryError로 이어질 수 있다.
    
    이 때 이 느려지게 하는 원인은 보통 소비자 쪽의 컴퓨팅 속도와 같은 것이고 이것을 back pressure라고 한다.
    
    [back pressure라는 용어는 이 back pressure를 해결하기 위한 방안이라는 의미로도 쓰인다.](https://www.baeldung.com/spring-webflux-backpressure#:~:text=Eventually%2C%20people%20also%20apply%20this%20term%20as%20the%20mechanism%20to%20control%20and%20handle%20it.) 그래서 상당히 헷갈린다.
    
    backPressure를 해결하기 위한 전략은 3가지가 있다.

    1. 공급자 속도 조절하기

       가장 좋은 방법이지만 사용자의 인풋이 공급자일 경우 조절할 수가 없다.

    2. 버퍼링

       아직 처리되지 않은 데이터를 모아두는 기술이다. 크기를 제한해놓지 않는다면 OOM이 발생할 여지가 있다. 다라서 크기를 벗어나는 인풋 데이터는 버려야 한다.

    3. 버리기

    ---

    [https://medium.com/@jayphelps/backpressure-explained-the-flow-of-data-through-software-2350b3e77ce7](https://medium.com/@jayphelps/backpressure-explained-the-flow-of-data-through-software-2350b3e77ce7)
