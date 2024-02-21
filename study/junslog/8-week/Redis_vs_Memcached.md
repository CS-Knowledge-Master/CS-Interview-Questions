# [ Redis에 대해 설명해주시고, Memcached와의 차이를 알려주세요 ]

### Redis

- Key - Value 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 DBMS
- DB, Cache, Message Broker로 사용되며, 인메모리 데이터 구조를 가진 저장소

### Cache Server

- 한번 읽어온 데이터를 임의의 공간에 저장하여, 다음에 읽을 때는 빠르게 결과값을 받을 수 있도록 도와주는 공간
- 같은 요청이 여러번 들어올 때,
  DB를 거치는 것이 아니라 Cache Server를 통해 저장된 값을 바로 가져오기에, DB의 부하를 줄이고, 서비스의 속도도 느려지지 않는 장점이 있다.

### Cache Server Pattern

- ***Look-aside cache***
    - 1) Client가 데이터를 요청
    - 2) WS는 데이터가 존재하는지 Cache Server에 먼저 확인
    - 3) Cache Server에 데이터가 있으면 DB에서 데이터를 조회하지 않고 Cache Server에 있는 결과값을 클라이언트에 반환 ( Cache Hit )
    - 4) Cache Server에 데이터가 없으면 DB에 데이터를 조회, Cache Server에 저장하고 결과값을 클라이언트에 반환 ( Cache Miss )
- ***Write Back***
    - 웹서버는 모든 데이터를 Cache Server에 저장
    - Cache Server에 특정 시간 동안 데이터가 저장됨.
    - Cache Server에 있는 데이터를 DB에 저장
    - DB에 저장된 Cache Server의 데이터를 삭제
    - JPA Flush / DB MVCC

### Redis 특징

- K-V 구조이기에 쿼리 사용할 필요가 없음.
- 메모리에서 데이터를 처리하기에 속도가 빠름.
- String, Lists, Sets, Sorted Sets, Hashes 자료 구조를 지원
- ***Single-Threaded***
    - 한 번에 하나의 명령만 처리 가능,
      중간에 처리 시간이 긴 명령어가 들어오면
      병목현상 발생
    - get, set 명령어의 경우 초당 10만 개 이상 처리 가능

### Memcached

- Redis와 마찬가지로 In-Memory Cache
- K-V 저장소
- 2003년에 만들어짐
- 정적 데이터 ( HTML .. ) 캐싱에 효과적이다.
- 멀티쓰레드 기능을 지원한다
    - Redis에 비해 스케일링에 유리하다.
    - 컴퓨팅 자원을 추가함으로써 스케일 업 가능.
    - 캐시된 데이터를 유실할 확률도 높아짐.

### Redis vs Memcached

- Redis는 다양한 자료구조를 지원하는데 반해,
  Memcached는 단순히 String만 사용한다.

- Redis는 6가지 정도의 다른 데이터 삭제 정책을 제공하지만,
  Memcached는 LRU방식으로 삽입된 새로운 데이터와 크기가 비슷한 데이터를 임의 제거한다.

- Redis는 디스크에 영구 저장 (영속화)가 가능한데 반해,
  Memcached는 지원하지 않음.

- Redis는 하나 이상의 복제본(Replica)을 가질 수 있는 반면,
  Memcached는 3rd Party의 도움을 받아야 복제본을 가질 수 있음.

- Redis는 연산의 원자성 및 트랜잭션 기능까지 지원하지만,
  Memcached는 연산을 원자적으로 동작시키기만 할뿐 트랜잭션을 지원하진 않음.