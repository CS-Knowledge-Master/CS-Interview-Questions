### Blocking I/O와 Non-Blocking I/O에 대해 설명해주세요.

Block / Non-Block
Sync / Async

[ Blocking vs Non-Blocking ]
- 어떤 함수를 호출한 주체(A)와, 호출한 함수(B) 간의 제어권이 넘어가는지 아닌지의 여부로 구분한다.
- Blocking의 경우, 제어권이 A에서 B로 넘어간다.
A의 실행은 B의 실행이 끝난 이후 진행된다.
- Non-Blocking의 경우, 제어권은 A로 바로 넘어온다.
A는 B가 하는 일을 기다리면서, 다른 일을 진행할 수 있다.

[ Synchronous vs Asynchronous ]
- 어떤 함수를 호출한 주체(A)가 호출한 함수(B)의 수행 결과나 종료 상태를 신경쓰는지 여부로 구분한다.
- Synchronous의 경우, 함수 A는 함수 B가 일을 하는 중에 기다리며, 현재 상태를 계속 체크한다.
- Asynchronous의 경우, 함수 B의 수행 상태를 B 혼자 직접 신경쓰면서 처리한다. (Callback)

- 비동기는 호출 시, Callback을 전달하여, 작업의 완료 여부를 호출한 함수에게 답하게 된다.

[ I/O Process & Kernel mode ]
- I/O 작업은 Kernel Level에서 수행 가능하다.
User Level의 Process / Thread는 커널에게 I/O를 요청해야 한다. ( System Call )

[1] Blocking I/O
- I/O Blocking 형태의 작업은

1) Process 혹은 Thread 가 Kernel에게 I/O 요청 System Call 호출 ( Blocking -> Kernel에게 CPU 제어권이 넘어감 )
2) Kernel이 작업을 완료하면 작업 결과를 반환받음.

특징
- I/O 작업이 진행 중, User Process / Threa는 자신의 작업을 중단한 채 대기한다.
- I/O 작업은 CPU를 거의 쓰지 않으므로, CPU가 그만큼 idle을 하니, 자원 낭비가 심하다.

< 여러 Client가 접속하는 Server를 Blocking 방식으로 구현하면? >
1) I/O 작업을 진행하는 작업을 중지
2) 다른 Client가 진행중인 작업을 중지하면 안되므로, Client 별로 별도의 Thread 생성
3) Thread 수가 많아짐
4) 많아진 Thread로 인한 Context Switching 횟수 증가 -> 오버헤드 증가

[2] Non-Blocking I/O
- I/O 작업이 진행되는 동안 User Process의 작업을 중단하지 않음.

진행 순서
1) User Process가 revcfrom 함수 호출 ( Kernel 에게 해당 Socket으로부터 데이터를 받고 싶다고 요청 )
2) Kernel은 해당 요청에 대해, 곧바로 recvBuffer를 채워서 보내지 못하으모,
"EWOULDBLOCK"를 return 한다.
3) Blocking 방식과 달리, User Process는 다른 작업을 진행할 수 있음.
4) recvBuffer에 user가 받을 수 있는 데이터가 있는 경우, Buffer로 부터 데이터를 복사해서 받아온다.
   ( revcBuffer는 Kernel이 가지고 있는 메모리에 적재되어 있으므로, Memory 간 복사로 인해 I/O보다 훨씬 더 빠르게 data를 받아올 수 있다. )
5) recvfrom 함수는 빠른 속도로 data를 복사하고, 복사한 데이터 길이와 함께 반환한다.