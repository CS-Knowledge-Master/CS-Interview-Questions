# HTTP와 HTTPS의 차이점에 대해 설명해보세요.

## 답변

```
HTTP와 HTTPS의 차이는 보내는 데이터가 암호화 되어있는지 여부라고 볼수있습니다. 
HTTP는 보내는 데이터를 암호화하지 않고 보내지만 HTTPS는 데이터를 암호화 시켜서 보냅니다. 
여기서 대칭키와 비대칭키 보안 기법이 사용됩니다.
```

## 내용 정리

- HTTP(Hypertext Transfer Protocol)

  클라이언트와 서버 간의 통신을 위한 통신 규칙 세트, 프로토콜. 암호화 되지 않은 데이터를 전송한다.

    - 프로토콜의 동작 과정

      보통의 TCP/IP 통신 방식과 동일하다.

        - 응답 코드
            - 1XX 정보제공
            - 2XX 정상
                - 200 OK: 성공
                - 201 Created: 요청이 처리되어서 새로운 리소스가 생성됨.
                - 202 Accepted: 요청은 접수되었지만 처리가 완료되지 않았다.
            - 3XX 리다이렉션
                - 301 Moved Permanently: 지정한 리소스가 새로운 URI로 이동했다.
                - 303 See Other: 다른 위치로 요청하라.
                - 307 Temporary Redirect: 임시로 리다이렉션 요청이 필요하다.
            - 4XX 클라이언트 에러
                - 400 Bad request: 요청의 구문이 잘못되었다.
                - 401 Unauthorized: 지정한 리소스에 대한 액세스 권한이 없다.
                - 403 Forbidden 금지됨.
                - 404 Not Found:리소스를 찾을수 없음.
            - 5XX 서버 에러
                - 500 Internal Server Error: 내부서버 오류,서버에 에러가 발생함.
                - 501 Not Implemented: 구현되지 않음,요청한 URI의 메서드에 대해 서버가 구현하고 있지 않다.
                - 502 Bad Gateway: 불량 게이트웨이, 게이트웨이 또는 프록시 역할을 하는 서버가 그 뒷단으로부터 잘못된 응답을 받은경우.
- HTTPS(Hypertext Transfer Protocol Secure)

  HTTP는 암호화되지 않은 데이터를 전송하는데  이ㄹ해당 데이터를 외부에서 가로채 볼수 있는 취약점을 보완하기 위해 나온 프로토콜로, 대칭키와 비대칭키를 사용하여 보안을 형성한다.

    - 보안 형성 과정
        1. 브라우저는 서버의 SSL 인증서를 요청한다.
        2. 서버는 Public key가 포함된 SSL 인증서를 보내준다.
        3. 브라우저는 SSL 인증서를 통해 서버 아이덴티티를 증명하고 브라우저에서 이것이 인증되면 같이 온 Public key를 사용하여 Secret session key를 포함한 메시지를 암호화하고 전송한다.
        4. 서버는 private key를 사용해 메시지를 해독하고 Secret session key를 얻는다.
        5. 이후 브라우저와 서버는 해당 session key를 가지고 서로 암호화 및 복호화작업을 진행한다.
    - SSL/TLS (Secret Socket Layer/ Transport Layer Securty)

      HTTP 데이터의 비 암호화 문제를 해결하기 위해 나온 프로토콜로 HTTP 하단의 레이어에서 데이터 암호화 및 복호화를 실시하여 보안성을 강화해줌.

      사실상의 Internet security의 표준이다.

      key 교환과정에서 2가지 프로토콜이 존재

        - Handshake Protocol

          서로의 certificates를 이용해 shared secret key를 교환하는 과정.

            - 절차
                - ClientHello

                  plaintext로 ssl/tls 프로토콜의 버전 정보, 사용 가능한 암호화 알고리즘과 난수를 보낸다.

                - ServerHello

                  plaintext로 클라이언트와 서버가 이용가능한 ssl/tls 버전중 가장 최신의 버전과,두 사람이 사용가능한 가장 강력한 암호화 알고리즘, 그리고 난수를 반환해준다.

                - ServerKeyExchange

                  서버는 public key certificate와 public key를 보낸다.

                - ClientKeyExchange

                  클라이언트는 secret key를 만들어서 서버에게 받은 public key로 암호화 해서 서버에게 보낸다.

        - Record Protocol

          서로의 key를 교환한 이후 데이터를 암호화해서 보내는 과정.

          전체의 데이터를 쪼개서 암호화후 보낸다.

- 둘의 차이점

  HTTP의 비암호화의 문제점을 대칭키와 비대칭키를 기반으로 하여 암호화 시켜서 더욱더 보완한것이 HTTPS이다.