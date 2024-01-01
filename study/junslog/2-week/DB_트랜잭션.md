### 트랜잭션에 대해 설명해주세요.

일련의 작업이 모두 실행되거나,
하나라도 실패 시 모두 취소될 수 있는 작업의 논리적 단위
특정 응용에 대한 비즈니스 로직에서 한 묶음으로 실행되어야할 SQL의 집합이라 할 수 있습니다.

### 커밋, 롤백 ( TCL : Transaction Control Language )
- 커밋 : 2차 메모리 ( DB )에서 RAM으로 가져온 데이터의 사본을 다시 DB에 반영하는 행위 ( 참고 단어 : MVCC : Multi-Version Concurrency Control )
- 롤백 : 트랜잭션 중 하나의 연산이 실패하였을 때, 해당 트랜잭션의 모든 연산을 취소하고 원래의 상태를 유지시키는 행위

### TPS ( Transaction Per Second )

### 트랜잭션과 동시성 이슈
- 커밋 완료 전 READ ( Dirty Read )
- 커밋 완료 전 OverWrite
- Read 중 데이터가 변경되는 경우
- 변경한 데이터 유실 ( Lost Update )

### 트랜잭션의 특성
- Atomicity : 트랜잭션 내 연산은 전부 시행되거나, 전부 시행이 되지 않아야한다. 
- Consistency : 트랜잭션이 진행되는 동안 다른 트랜잭션에 의해 DB가 변해도, 업데이트된 DB로 트랜잭션이 진행되는 것이 아니라, 처음에 트랜잭션을 진행하기 위해 참조한 DB로 진행한다.
- Isolation : 한 트랜잭션의 연산이 진행되는 동안 다른 트랜잭션의 간섭으로 인한 데이터 손실이 없어야 한다.
- Durability : 트랜잭션이 커밋되고 난 뒤의 결과는 영구히 지속되어야 한다,.

### 트랜잭션 Lock과 Isolation Level

여러 트랜잭션이 동시에 처리될 때, 
특정 트랜잭션이 다른 트랜잭션을 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 여부를 결정하는 정책

- Read Uncommitted 
- Read Committed 
- Repeatable Read 
- Serializable

[ 참고하면 좋을 블로그 ]
1. 트랜잭션
- https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98Transaction-%EC%9D%B4%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC
2. 트랜잭션 격리 수준 
- https://mangkyu.tistory.com/299