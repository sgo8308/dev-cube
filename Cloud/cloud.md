<details>
<summary>클라우드란?</summary>
<br>

클라우드란 인터넷 기반의 컴퓨팅이다. 클라우드를 제공하는 회사가 서버, 네트워크, 스토리지 등의 물리적인 리소스를 제공하고 클라우드 서비스를 이용하는 고객은 인터넷을 통해 이 하드웨어 리소스들을 사용하는 것.
</details>

<details>
<summary>클라우드를 왜 사용할까?</summary>
<br>

직접 인프라를 구축하는 것은 비용이 많이 든다. 또 유연하지 않다. 반면에 클라우드 서비스를 이용하면 직접 구축하는 것보다 비용을 아낄 수 있으며 상황에 따라 유연하게 서버를 늘렸다 줄였다 할 수 잇다. 또 보안에 있어서 뛰어나다.
</details>

<details>
<summary>Public Cloud vs Private Cloud vs On-Premise</summary>
<br>


퍼블릭 클라우드란 우리가 흔히 생각하는 클라우드를 말한다. 클라우드 서비스를 제공하는 기업에서 제공하는 인프라를 인터넷을 통해 이용한다.

프라이빗 클라우드란 기업에서 독자적으로 만들고 내부에서 또는 계열사들끼리만 사용하는 클라우드를 말한다. 보통은 기업에서 추후 퍼블릭 클라우드 사업을 시작하기 위하여 프라이빗 클라우드를 만들어서 사용하는 경우가 많다.

On-Premise는 기업에서 인프라를 직접 구축해서 사용하는 것을 말한다.

그럼 Private Cloud와 On-Premise는 무엇이 다른 것일까?

바로 ‘신속함’에서 차이가 있다.

서비스에 사용자가 몰리면 신속하게 해당 서비스를 위한 인프라를 증설할 필요가 있다. Private Cloud는 가능하지만 On-Premise는 불가능하다.

이것이 가능하려면 인프라 가상화가 필요하다. 가상화란 네트워크 장비, 서버, 스토리지(저장 장치) 등 데이터 센터 내의 인프라 전체를 가상화 솔루션을 통해 하나의 거대한 인프라 파워(능력)로 환산하는 기술이다. 이렇게 환산된 인프라 파워 내에서 가상의 네트워크 장비, 서버, 스토리지 등을 생성한 후 이를 조합해 가상의 인프라(가상머신)를 만들고 여기에 운영체제를 설치하고 소프트웨어와 서비스를 구동하는 것이다.

가상머신 위에 설치된 운영체제와 소프트웨어는 한 대의 인프라에서 실행되는 것이 아니다. 수 십 대의 실제 인프라가 각자의 능력을 조금씩 각출해서 가상머신을 만든 후 운영체제와 소프트웨어를 실행하는 것이다. 가상화 솔루션은 이렇게 각출한 능력을 조합해 가상머신이 기존 인프라와 동일한 능력을 발휘할 수 있도록 하고 있다. 인프라가 고장 나면 서비스도 함께 중단되는 온프레미스와 달리 가상화를 통해 생성된 가상머신은 지탱하는 여러 인프라 가운데 일부가 고장 나더라도 바로 다른 인프라에서 능력을 각출해오기 때문에 아무런 문제없이 소프트웨어를 실행하고 서비스를 제공할 수 있다.

---

[https://it.donga.com/27139/](https://it.donga.com/27139/)

[https://judo0179.tistory.com/36#:~:text=가상화 - Virtualization-,인프라의 기본 가상화,-가상화 (Virtualization)은](https://judo0179.tistory.com/36#:~:text=%EA%B0%80%EC%83%81%ED%99%94%20%2D%20Virtualization-,%EC%9D%B8%ED%94%84%EB%9D%BC%EC%9D%98%20%EA%B8%B0%EB%B3%B8%20%EA%B0%80%EC%83%81%ED%99%94,-%EA%B0%80%EC%83%81%ED%99%94%20(Virtualization)%EC%9D%80)
</details>

<details>
<summary>EC2에 DB 설치해서 사용하기 vs RDS 사용하기</summary>
<br>

EC2에 DB를 설치해서 사용하면 DB 관련 여러 설정들을 개발자가 직접 설정하고 튜닝할 수 있다. RDS는 AWS 측에서 DB를 전부 관리해준다. 버전 업데이트, 자동 백업, 보안패치 등을 모두 자동으로 진행해준다. failover 등을 위한 클러스터, 리플리카 등을 편하게 만들고 관리하게 해준다.

EC2는 개발자가 하나부터 열까지 모두 관리해야 하는 만큼 부담이 있지만 RDS에 비해 비용이 싸다.

---

[https://dingrr.com/blog/post/rds를-써야-하나요-ec2에-설치하면-안되나요](https://dingrr.com/blog/post/rds%EB%A5%BC-%EC%8D%A8%EC%95%BC-%ED%95%98%EB%82%98%EC%9A%94-ec2%EC%97%90-%EC%84%A4%EC%B9%98%ED%95%98%EB%A9%B4-%EC%95%88%EB%90%98%EB%82%98%EC%9A%94)
</details>
    
<details>
<summary>하이퍼바이저란?</summary>
<br>

hypervisor는 가상 머신 프로세스를 실행하고 관리 감독하는 프로그램, 펌웨어, 하드웨어 등을 말한다. 또는 호스트의 리소스를 분리해주는 프로그램이라고 할 수도 있다.

SuperVisor란 감독관이라는 의미다. 일반적으로 kernel을 supervisor라고 부른다. hypervisor는 supervisor들을 감독하는 더 상위의 감독관이라고 할 수 있다.

type-1 hypervisor, type-2 hypervisor가 있다.

type-1 htpervisor는 host의 하드웨어에서 직접 실행되며 type-2 hypervisor는 OS 위에서 작동한다. VMWare workstation이 type-2 hypervisor의 예이다.


hypervisor를 사용하면 호스트의 리소스를 더 효율적으로 사용할 수 있으며 하드웨어에 독립적이기 때문에 가상 머신들을 서로 다른 서버간에 옮기기도 쉽다. 예를 들어 네이버 클라우드에서는 호스트 하드웨어에서 장애가 발생할 경우 가상 머신들을 다른 호스트로 옮기는 Live Migration 작업을 지원한다.

---

[https://en.wikipedia.org/wiki/Hypervisor](https://en.wikipedia.org/wiki/Hypervisor)
</details>

<details>
<summary>vcpu란?</summary>
<br>
vcpu란 가상의 CPU라는 의미로 쓰레드와 비슷하게 생각할 수 있다. 

하나의 컴퓨터에서 hypervisor에 의해 여러 가상 머신이 실행될 수 있는데, hypervisor가 이 가상 머신에 실제 CPU의 time을 얼만큼 배분할건지, 또 실제 CPU의 성능은 어떤지에 따라 vCPU의 성능이 결정된다.

마치 프로세스가 운영체제에 의해 CPU 스케쥴링 되는 것처럼, 가상 머신이 hypervisor에 의해 CPU 스케쥴링 된다.

만약 vCPU와 CPU의 비율이 5:1라고 한다면, 하나의 실제 CPU에 5개의 vCPU가 할당되는 셈이다.

5개의 vCPU를 사용한다고 할 때 5개의 쓰레드를 병렬적으로 실행할 수 있다고 할 수는 없다. 5개의 vCPU가 5개의 실제 코어에 대응될 수도 있고 아닐 수도 있기 때문이다.

---

[https://download3.vmware.com/vcat/vmw-vcloud-architecture-toolkit-spv1-webworks/index.html#page/Core Platform/Architecting a vSphere Compute Platform/Architecting a vSphere Compute Platform.1.019.html](https://download3.vmware.com/vcat/vmw-vcloud-architecture-toolkit-spv1-webworks/index.html#page/Core%20Platform/Architecting%20a%20vSphere%20Compute%20Platform/Architecting%20a%20vSphere%20Compute%20Platform.1.019.html)

[https://www.howtogeek.com/devops/what-is-a-vcpu-and-how-much-performance-is-it/](https://www.howtogeek.com/devops/what-is-a-vcpu-and-how-much-performance-is-it/)

[https://www.datacenters.com/news/what-is-a-vcpu-and-how-do-you-calculate-vcpu-to-cpu](https://www.datacenters.com/news/what-is-a-vcpu-and-how-do-you-calculate-vcpu-to-cpu)
</details>