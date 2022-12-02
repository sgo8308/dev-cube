<details>
<summary>LAN WAN ISP와 관련하여 인터넷은 어떻게 구성되어 있을까?</summary>
<br>
LAN은 사설 네트워크로 여러 컴퓨터들이 스위치를 통하여 연결된다.

여러 LAN들을 공유기가 ISP의 공유기를 통해서 연결된 것이 WAN이다.

ISP는 계층적으로 나뉘는데 결국 각각의 ISP가 담당하는 모든 WAN들은 Tier 1이라고 하는 가장 상위 ISP 네트워크에 연결된다.

이렇게 LAN으로부터 WAN WAN으로부터 Tier 1까지 연결된 네트워크를 인터넷이라고 한다. 

SKT KT 등은 Tier 2 ISP이다.

---

그림으로 배우는 네트워크 원리, 21p
</details>

<details>
<summary>네트워크 프로토콜을 계층화한 이유는 무엇일까? ex TCP/IP 4계층</summary>
<br>

프토로콜을 계층화할 경우 프로토콜을 변경하거나 확장하기가 쉬워진다.

예를 들어 어떤 프로토콜을 변경하거나 기능을 추가 해야 할 경우 그 프로토콜만 신경 쓰면 된다.
</details>    
   
<details>
<summary>ip 주소가 있는데 왜 Mac 주소가 필요할까?</summary>
<br>
사실 Mac 주소 없이 IP 주소만으로도 네트워크가 돌아가게 할 수 있다.

그러나 다음과 같은 문제점이 있다.

1. Mac 주소와 관련된 시스템을 모두 바꿔야 한다.
    
    예를 들어 스위치의 경우 2계층까지만 패킷을 열고 MAC 주소를 확인하도록 되어 있다. 이것을 3계층까지 열고 IP 주소를 확인하게끔 변경이 필요할 것이다.
    
2. 3계층에는 IP만 있는 것이 아니다.
    
    당장만 해도 IPv4에서 IPv6 도입이 논의되는 상황이고, 그외에도 IPX와 같은 다른 방식이 있다. 
    
    그러므로 논리적 새로운 주소 방식이 도입될 때마다 스위치 디자인을 또 바꾸어야 한다. 
    
    그러나 이렇게 계층화를 통해 논리적 주소 방식과 물리적 주소 방식을 나눔으로써 수월하게 확장과 변경이 가능하다.
    

---

[https://networkengineering.stackexchange.com/questions/3329/reason-for-both-a-mac-and-an-ip-address](https://networkengineering.stackexchange.com/questions/3329/reason-for-both-a-mac-and-an-ip-address)

</details>

    