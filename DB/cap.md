<details>
<summary>CAP이론이란?</summary>
<br>
분산 환경에서 CAP 중에 2가지는 만족할 수 있지만 3가지를 모두 만족할 수는 없다는 이론이다.
DB가 2개로 이루어진 분산 DB 환경이라고 해보자.

C: Consistency로 DB에 데이터를 요청하면 언제나 두 DB 중 마지막에 업데이트된 값을 리턴받는 다는 것을 의미한다.
A: Availability로 DB에 데이터를 요청하면 언제나 빠른 시간 안에 응답을 받는다는 것을 의미한다.
P: Partition tolerance로 네트워크 장애가 발생하더라도 DB가 정상적으로 동작한다는 것을 의미한다.
---
https://hamait.tistory.com/197
</details>
<details>
<summary>Eventual Consitency란?</summary>
<br>    
두 시스템이 서로 동기화가 필요한 분산 시스템 환경이 있다고 하자. 

이 때 두 시스템에 동기화가 이루어지지 않은 상황에서도 사용자의 접근을 허용하는 것은 eventual consistency라고 한다.

사용자가 언제나 접근이 가능하여 가용성을 보장하지만, 사용자들이 같은 데이터에 대해 서로 다른 내용을 볼 수 있다는 점에서 일관성이 깨지게 된다. 

하지만 결국에는 시스템에 동기화가 이루어져 일관성이 보장된다.

반대되는 개념으로 Strong Consistency라는 개념이 있으며, 이 시스템에서 사용자는 동기화가 완료될 때까지 시스템에 접근하지 못한다.

따라서 일관성을 유지할 수 있지만 가용성이 깨지게 된다.

---

https://soongjamm.tistory.com/145
</details>
