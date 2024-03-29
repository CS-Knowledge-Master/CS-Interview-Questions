# HTTP와 HTTPS의 차이점에 대해서 설명해보세요.
HTTP는 서버 클라이언트 모델을 따라 데이터를 주고 받기 위한 프로토콜이다. 다른말로 인터넷에서 하이퍼 텍스트를 교환하기 위한 프로토콜로 80번 포트를 사용한다. 하지만 따로 암호화 과정을 거치지않아 중간에 패킷을 가로채거나 수정이 가능하다는 문제점이 존재한다. 

이러한 문제를 해결하기 위해 HTTP 에 데이터 암호화가 추가된 프로토콜인 HTTPS 가 등장했다. 443 번 포트를 사용하며 네트워크 통신 시 중간에 제 3자가 정보를 볼 수 없도록 암호화 했다.


---

<HTTPS 암호화 방식>

HTTPS는 대칭키, 비대칭키 암호화 방식을 모두 사용하고 있다.

- 대칭키 암호화

  클라이언트와 서버가 동일 키 사용해 암호화, 복호화 진행 - 노출 시 위험 하지만 연산속도 빠름

- 비대칭키 암호화

  1개의 쌍으로 구성된 공개키와 개인키를 암호화, 복호화하는데 사용함 - 키가 노출되어도 안전하지만 연산속도가 느리다.

    - 대칭키 방식: 암호화와 복호화에 같은 키를 사용, 대칭키 전달 과정에서 누군가 대칭키 획득시 쉽게 복호화 가능
    - 비대칭키: 공개키, 개인키 존재
        - 공개키 암호화: 공개키로 암호화하면 개인키로만 복호화가능
        - 개인키 암호화: 개인키의 소유자가 개인키로 암호화하고 공개키와 함께 전달한다. 이 경우 암호화된 데이터가 공개키로 복호화된다는 것은 개인키에 의해 암호화 되었다는 것으로 데이터 제공자의 신원이 보장됨

HTTPS 는 대칭키 암호화와 비대칭키 암호화 모두 사용한다.
처음 연결을 수립할 때 안전하게 세션키를 공유하기 위해 비대칭키가 사용되고, 그 이후 데이터 교환과정에서 빠른 연산속도를 위해 대칭키가 사용된다.
---
<HTTPS 연결과정>

1. 브라우저 즉 클라이언트가 서버로 연결 시도
2. 서버는 공개키(엄밀히 말하면 인증서) 를 브라우저에게 넘겨줌
    - 서버가 비대칭키를 발급받는 과정
3. 브라우저는 인증서 유효성 검사하고 세션키 발급
4. 브라우저는 세션키 보관하며 추가로 서버의 공개키로 세션키 암호화해 서버로 전송
5. 서버는 개인키로 암호화된 세션키 복호화해 세션 키 얻음
6. 클라이언트와 서버는 동일한 세션키 공유하므로 데이터 전달 시 세션키로 암호화 복호화 진행

<서버가 비대칭키를 발급받는 과정>

서버는 클라이언트와 세션키를 공유하기 위한 공개키를 생성해야하는데, 인증된 기관에 공개키를 전송해 인증서를 발급받는다.

1. A 기업이 HTTP 기반의 애플리케이션에 HTTPS 를 적용하기 위해 공개키와 개인키를 발급함
2. CA 기업에 돈을 지불 후 공개키를 저장하는 인증서 발급 요청
3. CA 기업이 기업이름, 서버의 공개키, 서버의 정보 등을 기반으로 인증서를 생성하고, CA 의 개인키로 암호화해 A 기업에게 제공
4. A 기업은 클라이언트에게 암호화된 인증서 제공
5. 클라이언트는 CA 기업의 공개키를 미리 다운받아 가지고 있어, 암호화된 인증서 복호화
6. 암호화된 인증서 복호화하여 얻은 A기업의 공개키로 세션키를 공유한다.

인증서는 CA 개인키로 암호화되어 신뢰성을 확보할 수 있다.
