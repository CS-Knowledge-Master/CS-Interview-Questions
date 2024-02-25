# HTTPS에 대해서 설명하고 SSL Handshake에 대해서 설명해보세요.    

## 답변
```
HTTPS란 HTTP의 비 암호화 통신의 문제를 해결하기위해 나온 프로토콜로 보내는 데이터를 암호화해서 통신을 진행합니다. 
HTTPS는 SSL Handshake라는 과정을 통해 암호화 형성을 진행하고 record protocol을 통해 암호화된 데이터의 통신을 진행합니다.
암호화에는 대칭키,비대칭키 알고리즘이 사용됩니다.
```

### 예상 꼬리질문 및 답변
- SSL Handshake에 대해 자세히 설명해주시겠어요?
  ```
  SSL Handshake는 암호화 형성 과정에서 진행되는 프로토콜로 대칭키와 비대칭키 알고리즘이 사용됩니다.
  우선 사용자가 서버에 요청을 보내면 서버는 자신의 public key를 CA의 인증서와 함께 보냅니다. 사용자는 인증서를 통해 public key의 신뢰성을 확인하고 대칭키를 만들어 서버의 public key로 암호화해서 다시 서버에게 보냅니다.
  이후 서버는 암호화된 public key를 받아 본인의 private key로 복호화를 하여 대칭키를 얻게됩니다. 이후 호스트와 서버는 해당 대칭키를 가지고 암호화 및 복호화를 진행하게 됩니다.
  ```

## 내용 정리
- HTTPS

  HTTP의 암호화되지 않은 데이터 전송으로 인한 보안 취약점을 보완하기 위해 나온 프로토콜로, 대칭키와 비대칭키를 사용하여 보안을 형성한다.

- SSL HandShake

  HTTPS의 암호화 형성과정에서 진행되는 프로토콜로 해당 프로토콜 이후에는 서로 암호화된 데이터를 전달하는 Record 프로토콜이 있다.

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

      handshake Protocol 형성 이후 데이터를 암호화 해서 보내는 프로토콜로 전체 데이터를 쪼개서 각각을 암호화후 보낸다.