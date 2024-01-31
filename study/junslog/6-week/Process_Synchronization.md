### 프로세스 동기화에 대해 설명해보세요.

여러 개의 프로세스 또는 스레드가
동시에 하나의 공유 자원에 접근할 때,
프로세스의 실행 순서에 따라 데이터의 일치성이 깨질 수 있습니다.
이런 문제를 해결하기 위해 프로세스 간의 공유자원 사용 순서를 정해줘야하는데
이것을 프로세스 동기화라고 합니다.

### 동기화 문제를 해결하는 방법
- Mutex, Semaphore, Monitor와 같은 기법을 사용할 수 있음.

- `Mutex` : 1개의 스레드만이 공유 자원에 접근할 수 있도록 하여 경쟁 상태를 방지함. ( lock 을 사용함 ) <br>
=> 공유 자원을 점유하는 Thread가 Lock을 걸면, 다른 Thread는 자원이 Unlock 상태가 될 때까지 해당 자원에 접근할 수 없음.

- `Semaphore` : S개의 Thread만이 공유 자원에 접근할 수 있도록 제어하는 동기화 기법
<br>=> 정수형 변수 S ( 세마포어 ) 값을 가용한 자원의 수로 초기화.
<br>=> 자원에 접근할 때, `S--` 연산 수행하여 세마포어 값을 감소
<br>=> 자원을 방출할 때, `S++` 연산을 수행하여 세마포어 값을 증가시킴
<br>=> 세마포어 값이 0이 되면, 모든 자원이 사용중임.
<br>=> 이후 자원을 사용하려는 프로세스/스레드는 세마포어 값이 0보다 커질 때까지 `block`됨.

- `lock`을 통해 원자적 연산을 구현할 수 있다.


### 임계 영역 ( Critical Section )
- 둘 이상의 Process/Thread가 동시에 동일한 자원을 접근하도록 하는 프로그램 코드 부분

- 한 Process/Thread가 자신의 임계구역에서 수행하는 동안,
다른 Process/Thread들은 그들의 임계구역 안으로 들어갈 수 없다는 것

- Critical Section 을 수행하는 코드 구조는 다음과 같음.
1) Entry Section : 진입 허가(Lock)을 획득하는 부분
2) Critical Section : 임계 영역
3) Exit Section : Lock을 Release 하는 부분

-> Mutex와 Semaphore는 lock을 `acquire()` 하는 동안
CPU Cycle을 낭비하는 `busy waiting` 문제가 발생할 수 있음.
또한, 이렇게 lock을 획득하기 위해 busy waiting 하는 과정을 spinlock 이라고 함.


### Semaphore
- 동기화 방법 중 하나로, Mutex와 가장 큰 차이는 <br>
공유 자원에 접근할 수 있는 process/thread의 개수가 2개 이상이 될 수 있다는 것.

- `semaphore` 변수 S (세마포어)에 동시 접근 가능한 `process/thread`의 갯수를 저장함.

- S값이 0이 되면, 다른 process/thread는 임계영역으로 접근 불가능.

- 임계영역에서의 작업이 끝나고, 임계영역에서 exit 하면서 S값을 1을 증가.

- 세마포어의 Entry Section / Critical Section / Exit Section 은 다음과 같은 순서이다.

```
// semaphore

wait(S)     // Entry Section

// Critical Section

signal(S)   // Exit Section
```