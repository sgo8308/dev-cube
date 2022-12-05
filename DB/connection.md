# 커넥션 풀
<details>
<summary>커넥션 풀은 왜 사용할까? ★</summary>
<br>
커넥션을 생성하고 닫는 데는 시간이 소모되기 떄문에 동시 접속자가 많은 웹사이트에서는 전체 성능이 낮아진다. 
따라서 커넥션을 미리 만들어 놓는 커넥션 풀을 사용하는 것이다.
</details>

<details>
<summary>커넥션은 왜 오래 걸릴까?</summary>
<br>
커넥션에는 TCP 연결 수립 과정이 있다.

또한 각 커넥션은 버퍼를 유지하는데 이 버퍼를 할당하는 과정에도 시간이 걸린다.

---

[https://dba.stackexchange.com/questions/16969/how-costly-is-opening-and-closing-of-a-db-connection](https://dba.stackexchange.com/questions/16969/how-costly-is-opening-and-closing-of-a-db-connection)
</details>

<details>
<summary>커넥션 풀을 사용지 않았을 때 동시 접속자 수가 몰리면 어떻게 될까?</summary>
<br>
각 커넥션에는 버퍼가 생성된다. 

커넥션이 제한되지 않고 무한정 생성되면 이 버퍼가 메모리를 가득채우고 OutOfMemoryError를 일으켜서 다운될 것으로 예상된다.

또한 커넥션 개수가 비대하게 늘어나 DBMS가 수용할 수 있는 수준을 넘어서면 전체 성능에 좋지 않은 영향을 끼칠 수 있다.

---

최범균, JSP2.3 웹프로그래밍, 가메출판사, 초판12쇄, 425p, 2021
</details>

<details>
<summary>Hikaricp란?</summary>
<br>
커넥션 풀 기능을 제공해주는 오픈소스 라이브러리 중 가장 안정성이 좋고 뛰어남

Java라면 DriverManager를 사용해서 커넥션을 미리 생성하여 풀에 넣어놓고 관리한다.
</details>
<details>
<summary>커넥션과 세션의 관계는 어떻게 될까?</summary>
<br>
커넥션이 생기면 DB에서는 내부적으로 커넥션과 관련된 세션이 생성된다.

커넥션을 통해 SQL이 전달되면 이렇게 만들어진 세션이 SQL을 처리한다.

세션 하나에서 트랜잭션이 시작하고 커밋되고, 다시 시작하고 커밋되고를 반복하다가 커넥션이 끊어지거나 세션을 강제로 종료하면 세션은 종료된다.

커넥션 풀에서 커넥션을 10개 생성할 경우 세션도 10개가 생성된다.

---

인프런, 스프링 DB 1편, 데이터베이스 연결 구조와 DB 세션 파트
</details>