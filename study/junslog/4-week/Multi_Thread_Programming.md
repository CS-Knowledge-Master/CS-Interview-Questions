# 멀티스레드 프로그래밍에 대해 설명해보세요.


## 내용 정리

프로세스는 여러 개의 독립된 작업들이 필요할 때
Thread가 필요하다.

만약, 웹서버가 여러 사용자들의 작업을 처리하기 위해
Process를 매번 생성한다면 시간과 자원을 많이 소비할 것이다.


### Multi-Threading의 이점

- Responsiveness
: 프로그램이 계산 집중적인 작업 시에도
프로그램의 실행이 계속되는 것을 허용한다.
이를 통해 사용자에 대한 응답성을 증가시킨다.

: 한 Thread에서 파일을 로드할 때,
다른 Thread에서 사용자와 상호작용을 할 수 있다.
  ( Thread Context-Switching )

- Resource Sharing
: Multi Process는 Process간 IPC(Inter Process Communication)의
공유 메모리나 메시지 전달 기법을 통해 자원을 공유할 수 있다.
하지만, Multi-Thread는 같은 Process 내 자원을 공유한다.
이로 인해 자원을 절약할 수 있다. 하지만, 이에 따른 동기화 문제가 발생할 수 있다.

- Economy
: Process를 여러 개 만드는 것보다, Thread를 여러 개 만드는 것이
시간과 자원에서 절약된다. 또한, Context-Switching 시 Thread가 Process보다
자원 소모량이 적다.

- Scalability
: Multi-Thread로 구성된 프로그램은
Multi-Core 구조에서 Core 하나 당 Thread 하나씩 할당되어
병렬로 처리된다.
Single-Thread로 구성된 Process는 Multi-Core라 하더라도 하나의 Core에서만 실행된다.

### Multi-Core Programming
- 최근 CPU의 경향은 칩 하나에 여러 개의 코어를 넣는 것임.
- OS 입장에서는 각 Core가 독립된 Processor로 보임.
- Multi-Core Programming은 Multi-Core를 더 효율적으로 사용하고
병행성(Parallelism)을 향상시킬 수 있는 기법을 제공함.
- OS는 Multi-Core에서 병렬 실행이 될 수 있도록, Scheduling Algorithm을 제공해야 한다.
- 프로그래머는, Multi-Core의 장점을 활용하기 위해 Thread를 잘 활용해야 한다.

### Multi-Core Programming을 위해 고려해야할 사항 5가지
1. Dividing Acitivies
- Process를 독립된 병행가능 Task로 나누는 작업

2. Balance
- Task들이 균등한 기여도록 가지도록 하는 것

3. Data Spliting
- 각 Task에서 개별적으로 접근 가능한 Data가 있어야 함.

4. Data Dependency
- 어떤 Data가 Task 사이에 종속성을 가질 때, 동기화를 해야한다. ( Queue )

5. Testing and Debugging
- 단일 Thread와 달리 다양한 실행 경로가 존재하므로,
Test와 Debugging이 힘들다.


### MultiThreading Models
- Thread에는 두가지 종류가 있다.
프로그래머가 사용하는 User Thread.
Kernel이 사용하는 Kernel Thread.

- User Thread는 Kernel Thread 위에서 관리,
Kernel의 지원 없이 관리된다.

- Kernel Thread는 운영체제에 의해 직접 지원되고 관리된다.

- User Thread와 Kernel Thread는 연관 관계가 존재해야 하고,
일반적으로 앞으로 나올 세 가지 방법 중 하나를 사용한다.

1-1. Many-To-One Model ( User = N : Kernel = 1 ) 
- 다수의 User Thread가 하나의 Kernel Thread에 연관된다.

Thread 관리는 사용자 공간에 존재하는 Thread Library에 의해 진행된다.

다만, 한 Thread가 Blocking System Call을 할 경우, 전체 Process가 Block 된다.

Kernel Thread가 하나의 CPU에만 접근 가능하므로, Many-To-One model에서는
user thread들에 여러 CPU를 할당되도록 할 수 없기 때문이다.

1-2. One-To-One Model ( User = 1 : Kernel = 1)
- Kernel Thread를 User Thread 하나 당 하나씩 만든다.
어떤 User Thread가 Blocking System Call을 하더라도, 각각의 Kernel Thread가 다른 CPU의 Core를
User Thread에 할당할 수 있으므로, Many-To-One Model 보다 더 많은 병렬성을 제공한다.

- User Thread를 생성할 때마다 Kernel Thread를 생성해야 하므로
Overhead가 발생한다. -> 이는 System을 느려지게 할 수 있다.

- 대부분 이 모델을 구현할 때 만들어질 수 있는 Thread 수를 제한한다.
Windows 와 Linux가 One-To-One Model을 구현하고 있다.

1-3. Many-To-Many Model ( User = N : Kernel = M )

- 앞의 Many-To-One, One-to-One model의 장점을 살려
User Thread 개수와 같거나 적은 개수의 Kernel Thread를 만들고 Multiplex 한다.

- Blocking System Call이 전체 System을 Block하지 않는다.

- Kernel Thread의 개수가 One-To-One Model보다 적으므로, 앞의 두 모델의 두 가지 단점을 어느 정도 해결한다.

### Thread Library
- 프로그래머에게 Thread를 생성하고 관리하도록 API를 제공한다.

- 두가지 방법이 있다.

- 첫번째, Kernel의 지원없이 완전히 User Space에서만 Library를 제공하는 것이다.

- 두번째, 운영체제에 의해 Kernel Space에서 구현하는 것이다.

- 전자는 User Space에서 구현된 API만 관련되고, Kernel은 관련이 없다.

- 후자는 Kernel System Call을 하게 되고, Kernel이 관계된다.

- POSIX Pthreads : POSIX 표준 thread의 확장판이다. Kernel 수준의 Library이다.

- Win32 threads : Windows System에서 제공되는 Kernel 수준의 Library이다.

- Java Threads : Java는 JVM위에서 돌아가므로, JVM이 실행되고 있는 OS와 HW에 기반해서 구현된다.
Windows System에서는 Win32 Threads로, Linux System에서는 Pthreads로 구현된다.

### Thread Pool

- Thread는 생성하고 실행하는데 비용이 든다.

- 웹서버에서 매 요청마다 Thread를 생성하면 생성하는데 소요되는 시간이 Overhead가 된다.

- 또한, Thread를 무작정 많이 만들면 Context-Switching이나, 메모리 소모 등 시간/공간적 자원이 많이 소모된다.

- 이런 문제점들을 해결할 수 있는 방법 중 하나가 Thread Pool이다.

- Process가 시작할 떄 일정한 수의 Thread를 미리 Pool로 만들어두는 것이다.

- 생성된 Thread들이 대기하고 있다가, 요청이 들어오면 Pool의 Thread가 요청을 처리한다.

- 요청을 다 처리하면 Pool에 다시 돌아간다.


- 멀티 쓰레드 

  하나의 프로세스 내에서 둘 이상의 쓰레드가 동시에 수행하는 것을 의미.

    - 장점
        - 컨텍스트 스위칭시 공유 메모리만큼의 자원을 아껴 효율적인 프로그래밍이 가능함.
        - 여러 쓰레드가 stack을 제외한 모든 메모리 영역을 공유하기에 통신이 적고 응답시간이 빨라짐.
    - 단점
        - 한 쓰레드가 자원을 오염시키면 프로세스 전체가 종료됨.
        - 자원 공유로 인한 동기화 문제를 피해야한다.
- 멀티 프로세스

  여러개의 CPU를 사용하여 여러 프로세스를 동시에 수행하는 것.

- 데몬 스레드

  다른 일반 스레드의 작업을 돕는 보조적인 역할을 하는 스레드로 일정 시간마다 자동으로 수행되는 저장 및 화면 갱신등에 이용되고 있다.