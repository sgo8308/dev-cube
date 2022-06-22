# Stickey Session
### 정의는?
    
    로드 밸런서가 특정 고객의 첫 요청 이후 이 고객의 모든 요청을 세션이 살아있는 기간동안 특정 서버로 고정하여 세션을 한 서버에서만 관리해도 문제없게 만드는 방식.
    
### 사용해야 하는 이유는?
    
    네트워크에 있는 서버들이 세션 데이터를 공유할 필요가 없다. 이 작업은 서버 규모에 따라 비용이 많이 드는 작업이다.
    
### 어떤 방식으로 동작할까?
    
    로드 밸런서가 쿠키에 가야할 서버를 저장하거나 IP hashing 방법을 이용하여 특정 고객이 하나의 서버하고만 통신할 수 있게 한다.
    
### 단점은?
    
    특정 서버가 과부하 올 수 있다.
    
    특정 서버가 문제가 생기면 그 서버에 붙어 있는 세션들이 소실될 수 있다.
    

---

[https://www.imperva.com/learn/availability/sticky-session-persistence-and-cookies/](https://www.imperva.com/learn/availability/sticky-session-persistence-and-cookies/)

[https://smjeon.dev/web/sticky-session/](https://smjeon.dev/web/sticky-session/)
# WAS Session clustering
### 클러스터링이란?
    
    여러개를 마치 하나인 것처럼 묶는 것
    
### WAS Session clustering이란?
    
    한 서버의 세션을 다른 모든 서버가 복제해서 동일한 세션정보를 유지하는 것
    
### 장점과 단점은?
    
    장점
    
    - Sticky Session에서 발생하는 한 서버로 트래픽이 몰릴 수 있는 문제를 해결해준다.
    
    단점
    
    - 세션 저장소에 변경이 일어날 때마다 다른 모든 서버들이 세션 저장소를 업데이트해주어야 한다. 이 때 네트워크를 통하여 그 정보가 전달되기 때문에 서버 수에 비례해서 네트워크 트래픽이 늘어나는 등 성능 저하가 발생한다.

### 어떤 방식으로 동작할까?
    
    한 서버의 세션 저장소 정보가 업데이트 될 때마다 네트워크를 통하여 그 내용을 공유하고 다른 서버가 그 내용에 따라 세션 저장소를 똑같이 업데이트해준다.
    

---

[https://hyuntaeknote.tistory.com/6](https://hyuntaeknote.tistory.com/6)

# In-memory session storage
### 세션 스토리지 방식이란?
    
    세션을 세션 스토리지라고 하는 저장소에서 따로 관리하고 서버들이 이 저장소를 공유하는 방식.
    
### 세션 스토리지 방식의 장점과 단점은?
    
    장점
    
    - 한 서버로 트래픽이 몰릴 일이 없다. (Sticky Session의 단점없음)
    - 세션의 정합성을 위해 네트워크 트래픽에 부하가 생길 일이 없어 Scale out이 쉽다. 세션스토리지에 대한 정보만 추가되는 서버에 입력해주면 된다.
    
    단점
    
    - 한 곳에서 관리하기 때문에 이 스토리지에 장애가 발생하면 세션 기능이 마비될 수 있다. 따라서 백업 세션 스토리지 구성 필요.
    - 메모리 방식의 세션과 달리 세션 스토리지로 네트워크 요청이 한 번 더 이루어져야 하기 때문에 성능 저하
### 인메모리 세션 스토리지 방식이란?
    
    데이터베이스는 디스크 방식과 인메모리 방식이 있다.
    
    디스크 방식은 데이터를 디스크에 저장하는 것이고 인메모리 방식은 메모리에 저장하는 방식이다.
    
    따라서 인메모리에 방식은 데이터를 영구 저장할 수 없다.
    
    하지만 세션은 영구 저장을 위한 목적이 아니기 때문에 인메모리 데이터베이스를 세션스토리지로 사용할 수 있다.
    
### 인메모리 세션 스토리지 방식의 장점과 단점은?
    
    장점
    
    - 디스크 방식에 비해 훨씬 빠르게 읽고 쓸 수 있다.
    
    단점
    
    - 데이터를 영구 저장할 수 없다. 따라서 중간에 인메모리 세션 스토리지에 문제가 생기면 세션이 모두 소실될 수 있다. 따라서 Replication을 통해 백업 스토리지를 구성하는 것이 좋다.

---

[https://hyuntaeknote.tistory.com/7?category=867120](https://hyuntaeknote.tistory.com/7?category=867120)

[https://hyuntaeknote.tistory.com/6?category=867120](https://hyuntaeknote.tistory.com/6?category=867120)