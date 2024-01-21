# 트랜잭션 격리 수준에 대해 설명해주세요.


## 내용 정리

### 트랜잭션 격리수준(Isolation Level)

여러 트랜잭션이 동시에 처리 될때, 특정 트랜잭션이 다른 트랜잭션을 변경하거나 조회하는 데이터를 볼수 있게 허용할지의 여부를 결정하는 것 

격리 수준은 높은 순서대로
SERIALIZABLE > REPEATABLE READ > READ COMMITTED > READ UNCOMMITED
이다.

### [ SERIALIZABLE ]
- 가장 엄격한 격리 수준으로, 이름 그대로 트랜잭션을 순차적으로 진행시킨다.
- 여러 트랜잭션이 순차적으로 처리되어야 하므로, 어떠한 데이터 부정합 문제도 생기지 않는다.
하지만, 동시 처리 성능이 매우 떨어진다.

- MYSQL에서
`SELECT FOR SHARE/UPDATE` 는 대상 레코드에 각각 읽기/쓰기 잠금을 거는 것이다.
하지만, 순수한 SELECT 작업은 아무런 레코드 잠금 없이 실행된다.

- SERIALIZABLE 격리 수준에서는 순수한 SELECT 작업에서도
대상 레코드에 Next-Key Lock을 읽기 잠금(공유 락, Shared Lock)으로 건다.

- 따라서, 한 트랜잭션에서 Next-Key Lock이 걸린 레코드를 다른 트랜잭션에서는
절대 추가/수정/삭제할 수 없다.

- SERIALIZABLE을 극단적으로 안전해야만 하는 작업이 아닌 이상 사용해서는 안된다.

### [ REAPEATABLE READ ]

#### MVCC ( Multi-Version Concurrnecy Control )
- RDBMS는 일반적으로, 변경 전의 레코드를 UNDO 공간에 백업해둔다.

- 이렇게 되면, 변경 전/후 데이터가 모두 존재하므로
동일한 레코드에 대해 여러 버전의 데이터가 존재한다고 하여
이를 MVCC(Multi-Version Concurrency Control; 다중 버전 동시성 제어)라고 부른다.

- MVCC를 이용하여,
트랜잭션이 RollBack 된 경우 데이터를 복원할 수 있고,
서로 다른 트랜잭션 간에 접근할 수 있는 데이터를 세밀하게 제어할 수 있다.

- 각각의 트랜잭션에는 순차적으로 증가하는 고유 트랜잭션 번호가 존재하고,
백업 레코드에는 어느 트랜잭션에 의해 백업되었는지 여부를 트랜잭션 번호를 통해 저장한다.
해당 데이터가 불필요하다고 판단하는 시점에 Background Thread를 통해 삭제한다.

- REAPEABLE READ는 MVCC를 사용하여, 한 트랜잭션 내에서 동일한 결과를 보장하지만
새로운 레코드가 추가되는 경우에 부정합이 생길 수 있다.

- REPEATABLE READ는 트랜잭션 번호를 참고하여,
자신보다 먼저 실행된 트랜잭션의 데이터만을 조회한다.
만약, 테이블에 자신보다 이후에 실행된 트랜잭션의 데이터가 존재한다면
UNDO Log를 참고해서 데이터를 조회한다.

- 즉, REPEATABLE READ는 어떤 트랜잭션이 읽은 데이터를
다른 트랜잭션이 수정하더라도 동일한 결과를 반환할 것을 보장해준다.

- 다만, REPEATABLE READ는 다른 트랜잭션에 의핸 새로운 레코드의 추가까지는 막지 않는다.
SELECT로 조회한 경우 트랜잭션이 끝나기 전에 다른 트랜잭션에 의해 추가된 레코드가 발견될 수 있는데,
이것을 유령 읽기(Phantom Read)라고 한다.

- 하지만, MVCC 덕분에 일반적인 조회에서 유령 읽기(Phantom Read)는 발생하지 않는다,
자신보다 나중에 실행된 트랜잭션이 추가한 레코드는 무시하면 되기 때문이다.

- 추후 작성 예정
  

### [ READ COMMITTED ]
- 커밋된 데이터만 조회할 수 있는 격리 수준이다.

- Phantom Read에 더해, Non-Repeatabla Read 문제까지 발생한다.

- 추후 작성

### [ READ UNCOMMITTED ]
- 커밋되지 않은 데이터 또한 읽을 수 있는 격리 수준

- Phantom Read, Non-Repeatable Read, Dirty Read 또한 발생 가능