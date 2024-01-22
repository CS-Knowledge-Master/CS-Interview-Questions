# HTTP와 HTTPS의 차이점에 대해 설명해보세요.

-> HTTP와 HTTPS 의 차이는 주고받는 데이터의 암호화 여부입니다.
HTTP는 평문 기반으로 데이터를 주고받기에, 제 3자가 해당 정보를 가로채고 읽을 수 있습니다.
이러한 HTTP 기술에 TLS 기술을 결합하여 암호화된 데이터를 주고받게 됩니다.

### HTTP(Hypertext Transfer Protocol)
- 클라이언트와 서버 간의 통신을 위한 통신 규칙 세트, 프로토콜. 암호화 되지 않은 데이터를 전송한다. 

### HTTPS(Hypertext Transfer Protocol Secure)
- 보안에 취약한 문제가 있는 HTTP 프로토콜을 개선하기 위해, 이에 SSL/TLS 기술을 결합한 프로토콜
대칭키와 비대칭키를 사용하여 보안을 형성한다.

- 보안 형성 과정
0. HTTPS 웹 사이트는 독립된 인증 기관(CA)에서 SSL/TLS 인증서를 획득한다.
1. 사용자 브라우저의 주소 표시줄에 `https://URL` 형식을 입력하여 HTTPS 웹 사이트를 방문한다.
2. 브라우저는 서버의 SSL 인증서를 요청하여 사이트의 신뢰성을 검증하려 한다.
3. 서버는 자신의 Public Key가 포함된 SSL 인증서를 보내준다.
4. 브라우저는 SSL 인증서를 통해 서버 아이덴티티를 증명하고 브라우저에서 이것이 인증되면 같이 온 서버의 Public key를 사용하여 
브라우저의 Secret session key를 포함한 메시지를 암호화하고 전송한다.
5. 웹 서버는 개인 키인 Private Key를 사용해 메시지를 해독하고 Secret session key를 얻는다.
그 다음 세션 키를 암호화하고 브라우저에 승인 메시지를 전송한다. 
6. 이후 브라우저와 서버는 해당 session key를 가지고 메시지를 안정하게 교환하도록 전환한다.

HTTP 데이터의 비 암호화 문제를 해결하기 위해 나온 프로토콜로 HTTP 하단의 레이어에서 데이터 암호화 및 복호화를 실시하여 보안성을 강화해준다.

### [ Handshake Protocol ]
서로의 certificates를 이용해 shared secret key를 교환하는 과정. 절차

1. ClientHello
- Plain Text로 SSL/TLS 프로토콜의 버전 정보, 사용 가능한 암호화 알고리즘과 난수를 보낸다.
2. ServerHello
- Plain Text로 클라이언트와 서버가 이용가능한 SSL/TLS 버전중 가장 최신의 버전과,두 사람이 사용가능한 가장 강력한 암호화 알고리즘, 그리고 난수를 반환해준다.
3. ServerKeyExchange
- 서버는 public key certificate와 public key를 보낸다.
4. ClientKeyExchange
- 클라이언트는 secret key를 만들어서 서버에게 받은 public key로 암호화 해서 서버에게 보낸다.

### [ Record Protocol ]
서로의 Key를 교환한 이후 데이터를 암호화해서 보내는 과정.
- 전체의 데이터를 쪼개서 암호화후 보낸다.

### HTTP/2, HTTP/3, HTTPS
- 1996년 ~ 1997년에 출시된 최초의 HTTP 버전이 HTTP/1.1 이다.

- HTTP/2와 HTTP/3은 프로토콜 자체를 업그레이드한 버전

- 데이터 전송 시스템을 수정하여 효율성을 개선함.
예를 들어, HTTP/2는 텍스트 형식 대신, 바이너리로 데이터를 교환함.
또한, 서버가 새 HTTP 요청을 기다리는 대신, 클라이언트 캐시에 응답을 사전에 전송할 수 있음.
HTTP/3은 비교적 최근에 나온 버전이며, HTTP/2를 한단계 발전시킨 것임.
HTTP/3의 목표는 실시간 스트리밍 및 기타 최신 데이터 전송 요구 사항을 보다 효율적으로 지원하는 것

- HTTPS는 HTTP에서 데이터 보안 문제를 우선시함.
최신 시스템에서는 SSL/TLS와 함께 HTTP/2를 HTTPS로 사용함.

### HTTP vs HTTPS
- 보안

- 권위
: 검색 엔진은 HTTP의 신뢰성이 더 낮기 때문에
보통 HTTP 웹 사이트 콘텐츠의 순위를 HTTPS 웹 페이지보다 낮게 지정함.

- 성능 및 분석
: HTTPS 웹 애플리케이션은 HTTP 애플리케이션보다 로드 속도가 더 빠름.
HTTPS는 참조 링크도 더 잘 추적함.

### CA와 인증서 - HTTPS의 설정 비용
- HTTPS를 사용하려면 서버에서 SSL/TLS 인증서를 획득하고 유지, 관리해야함.
- 과거에는 대부분의 인증 기관(CA)이 인증서 등록 및 유지 관리에 대한 연간 요금을 청구했음.

- 요즘에는 무료 SSL 인증서를 획득할 수 있는 많은 출처가 있다.
ex) AWS Cerficate Manager(ACM) : AWS 서비스 및 내부 연결 리소스와 함께 사용할 수 있는
퍼블릭 및 프라이빗 SSL/TLS 인증서를 프로비저닝, 관리 및 뱊함.