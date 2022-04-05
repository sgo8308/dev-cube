### HTTPS란?
    
    Hypertext transfer protocol secure의 약자로 보안이 강화된 http 프로토콜을 의미한다.
    
    일반 http 통신 과정에서는 데이터 통신 중에 해커가 데이터를 탈취하거나,
    피싱 사이트가 사용자로부터 민감한 데이터를 취할 수 있다.
    
    https 프로토콜을 통해 사용자는 데이터를 암호화해서 보낼 수 있으며 인증 기관의 인증서를 통한 사이트의 신뢰성을 보장받을 수 있다.
    
    ---
    
    [https://www.cloudflare.com/ko-kr/learning/ssl/what-is-https/](https://www.cloudflare.com/ko-kr/learning/ssl/what-is-https/)
    
### HTTPS의 전체적인 연결 과정은?
    1. TCP 3 way hand shake 후 TCP 연결을 한다.
    2. TLS 핸드쉐이크 과정을 거치고 세션 키를 생성한다.
    3. 세션 키를 이용해 메시지를 암호화해 통신한다.
    - TLS 핸드쉐이크 과정은 어떻게 될까?
        
        [https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/](https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/)
        
### HTTPS가 어떻게 암호화를 할까?
    
    TLS HandShaking이라는 과정을 거쳐서 서버와 클라이언트 둘 다 동일한 세션 키(대칭키)를 생성하고 이 세션 키를 이용해 메시지를 암호화한 후 통신합니다.
    
    TLS HandShaking은 다음과 같은 과정을 거칩니다.
    
    1. 클라이언트는 서버와 무작위 난수를 주고 받는다.
    2. 클라이언트는 난수와 함께 받은 TLS 인증서를 통해 사이트를 인증하고 사이트의 공개키로 마스터 암호를 암호화해 전송한다.
    3. 클라이언트는 난수 2개와 마스터 암호를 이용해 세션 키를 생성하고,
    서버도 자신의 비밀키로 마스터 암호를 얻은 후 난수 2개와 함께 세션 키를 생성한다.
    
    이후 세션 키를 이용해 메시지를 암호화해서 통신합니다.
    
### TLS 프로토콜은 어떤 Layer에 속해있을까?
    
    애플리케이션 계층과 TCP/IP 계층 사이에 있다.
    
    ---
    
    [https://docs.microsoft.com/en-us/windows-server/security/tls/transport-layer-security-protocol#:~:text=The TLS (and SSL) protocols,support multiple application layer protocols]