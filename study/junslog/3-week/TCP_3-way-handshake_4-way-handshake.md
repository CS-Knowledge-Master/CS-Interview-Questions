## TCP의 3-way handshake와 4-way handshake에 대해 설명해주세요.

### [ 3-way handshake ]
TCP/IP 네트워크 상에서
TCP는 두 응용 프로세스간 정확한 전송을 보장하기 위한
논리적인 연결 성립을 위해 3-way handshake 과정을 진행한다.

1) 클라이언트가 서버에게 SYN Packet을 보낸다. ( Client Sequence Number : x )
2) 서버가 SYN(x)를 받고, 클라이언트에게 패킷을 받았다는 신호인 ACK와 SYN Packet을 보낸다.
   (Server Sequence Number = y, ACK = x + 1)
3) 클라이언트는 서버의 응답 ACk(x+1)과 SYN(y) 패킷을 받고, ACK(y+1)을 서버로 보낸다.

연결이라는 것은 결국 순서번호(Sequence Number)로 구현된다.

### [ 4-way handshake ]

TCP/IP 네트워크 상에서
두 응용 프로세스 간의 연결을 해제하는데 필요한 과정으로
4-way handshake를 진행한다.

1) Client Process 가 Server Process에 FIN 패킷을 보낸다.
   ( 연결 종료 요청 )
2) Server Process 가 Client Process에 ACK 패킷을 보낸다.
   ( 연결 종료 패킷을 받았음을 확인 )
3) Server Process 가 연결 종료를 수행할 상태가 되면,
 Client Process에게 FIN 패킷을 보낸다.
4) Client Process 가 Server Process에게 연결 종료 요청을 확인했다는 의미의
   ACK 패킷을 보낸다.