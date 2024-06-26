# HTTP와 HTTPS의 차이점에 대해서 설명해보세요.
HTTP는 서버 클라이언트 모델을 따라 데이터를 주고 받기 위한 프로토콜이다. 다른말로 인터넷에서 하이퍼 텍스트를 교환하기 위한 프로토콜로 80번 포트를 사용한다. 하지만 따로 암호화 과정을 거치지않아 중간에 패킷을 가로채거나 수정이 가능하다는 문제점이 존재한다. 

이러한 문제를 해결하기 위해 HTTP 에 데이터 암호화가 추가된 프로토콜인 HTTPS 가 등장했다. 443 번 포트를 사용하며 네트워크 통신 시 중간에 제 3자가 정보를 볼 수 없도록 암호화 했다.


---

## HTTPS 암호화 방식

HTTPS는 대칭키, 비대칭키 암호화 방식을 모두 사용하고 있다.

### 대칭키 암호화

  클라이언트와 서버가 동일 키 사용해 암호화, 복호화 진행 - 노출 시 위험 하지만 연산속도 빠름

### 비대칭키 암호화

  1개의 쌍으로 구성된 공개키와 개인키를 암호화, 복호화하는데 사용함 - 키가 노출되어도 안전하지만 연산속도가 느리다.

  - 공개키 암호화: 공개키로 암호화하면 개인키로만 복호화가능
  - 개인키 암호화: 개인키의 소유자가 개인키로 암호화하고 공개키와 함께 전달한다. 이 경우 암호화된 데이터가 공개키로 복호화된다는 것은 개인키에 의해 암호화 되었다는 것으로 데이터 제공자의 신원이 보장됨

HTTPS 는 대칭키 암호화와 비대칭키 암호화 모두 사용한다.
처음 연결을 수립할 때 안전하게 세션키를 공유하기 위해 비대칭키가 사용되고, 그 이후 데이터 교환과정에서 빠른 연산속도를 위해 대칭키(TLS 핸드셰이크가 진행되는 동안 두 통신 장치에서는 세션 키가 설정되고, 이 키는 나머지 세션 동안 대칭키로 이용된다 생각하자)가 사용된다.


## HTTPS 연결과정

1. 브라우저 즉 클라이언트가 서버로 연결 시도
2. 서버는 공개키(엄밀히 말하면 인증서) 를 브라우저에게 넘겨줌
    - 서버가 비대칭키를 발급받는 과정
3. 브라우저는 인증서 유효성 검사하고 세션키 발급
4. 브라우저는 세션키 보관하며 추가로 서버의 공개키로 세션키 암호화해 서버로 전송
5. 서버는 개인키로 암호화된 세션키 복호화해 세션 키 얻음
6. 클라이언트와 서버는 동일한 세션키 공유하므로 데이터 전달 시 세션키로 암호화 복호화 진행


## TLS HandShake
![image](https://github.com/CS-Knowledge-Master/CS-Interview-Questions/assets/62535887/20e16636-0129-44a6-b01c-22e5cb860a8c)

1. ClientHello
   * 클라이언트가 서버에 연결을 시도하며 전송하는 패킷이다.
   * 자신이 사용 가능한 Cipher Suite 목록, Session ID, SSL 프로토콜 버전, Random Byte 등을 전달한다.
   * Cipher Suite는 SSL 프로토콜 버전, 인증서 검정, 데이터 암호화 프로토콜, Hash 방식 등의 정보를 담고 있는 존재로, 선택된 Cipher Suite의 알고리즘에 따라 데이터를 암호화하게 된다.
2. ServerHello
   * 서버는 클라이언트가 보내 온 ClientHello 패킷을 받아 Cipher Suite 중 하나를 선택한 다음 클라이언트에게 이를 알린다.
   * 또한, 자신의 SSL 프로토콜 버전 등도 같이 보낸다.
3. Certificate
   * Server가 자신의 SSL 인증서를 클라이언트에게 전달(인증서 내부에는 CA 공개키가 들어있어, 이걸로 서버의 공개 키 검증 작업을 진행한다)
(4. Server Key Exchange)
   * 3번 과정에서 서버의 공개 키가 SSL 인증서 내부에 없는 경우, 서버가 직접 전달한다. 공개 키가 SSL 인증서 내부에 있을 경우 Server Key Exchange는 생략된다.

5. Client key Exchange
   * 클라이언트는 서버의 공개키로 대칭키(대칭키는 SSL Handshake의 목적이자 가장 중요한 수단인 데이터를 실제로 암호화할 키이다) 를 암호화 한 후 서버에 전달한다.

6. ChangeCipherSpec/Finished
   * ChangeCipherSpec 패킷은 클라이언트와 서버 모두가 서로에게 보내는 패킷으로, 교환할 정보를 모두 교환한 뒤 통신할 준비가 다 되었음을 알리는 패킷이다.
   * 그리고 Finished 패킷을 보내어 SSL Handshake를 종료
### 인증서 발급 원리
서버의 공개키를 받아, 클라이언트가 암호화해서 전달하면 - 이 때 공개키 전달 과정에서 해킹범이 자신의 공개키를 넘겨줘 정보를 해킹할 가능성이 있다.

따라서 인증기관을 별도로 두어, 서버로부터 받은 공개키가 믿을만한 서버로부터(우리가 기대하는 서버로부터) 왔는지 확인하는 과정이 인증서를 발급해 사용하는 것의 목적이다. (혹은 인증서의 역할은 클라이언트가 접속한 서버가 의도한 서버가 맞는지 보장하는 것이라 생각하자)

SSL(TLS) 인증서는 서버의 공개키와 CA에서 그 공개키가 맞다고 증명한 Signature(Fingerprint)가 포함된다. 사용자는 이 Signature를 이용해 공개키가 신뢰가능하다는 것을 확인할 수 있다.

아래 그림은 왼쪽은 인증서를 만드는 과정, 오른쪽은 인증서를 통해 신원증명하는 과정이다.

![image](https://github.com/CS-Knowledge-Master/CS-Interview-Questions/assets/62535887/36eeb532-04a7-456f-b521-859e1bfb9d81)

1. Signing
  서버는 CA 에게 자신의 공개키를 전달하고, CA 는 서버의 공개키를 해시함수를 통해 해시화한 후 자신의 개인키로 암호화해 클라이언트에게 전달한다.
2. Verification
  클라이언트는 자신이 받은 서버의 공개키를 해시화한 후 시그니처를 CA 의 공개키로 복호화 한 후 둘이 동일하다면 서버의 공개키가 믿을만 하다고 판단한다.

이 방식으로 클라이언트는 신뢰할 수 있는 서버의 공개키를 획득하고, 서버의 공개키로 암호화 해 서버로 전달하면 서버가 자신의 개인키로 복호화해 정보를 사용한다.
