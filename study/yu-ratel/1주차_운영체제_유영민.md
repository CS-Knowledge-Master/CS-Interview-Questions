# Process

- 실행되고 있는 프로그램의 작업장
- heap, stack, code, data 를 기반으로 작업
-

# Thread

- process에서 작업 단위(흐름)
- stack을 제외한 heap, code, data 를 공유해서 작업함

- multi ( process vs thread )

## process

- 장점
  - 메모리가 격리되어 있어서 다른 프로세스에 영향을 주지않음 -> error, 변경 등 위험성이 낮음
- 단점
  - 메모리가 격리되어 있기에 불필요한 메모리를 차지할 수 있고 속도가 thread의 비해서 느림

## thread

- 장점
  - 한 process안에서 여러개의 thread 가 stack을 제외한 메모리영역을 공유하기에 메모리를 효율적으로 사용할 수 있고 통신속도가 빠르다.
- 단점
  - 메모리가 공유되기에 한 곳에서의 문제가 생긴다면 공유하는 모든곳에 문제가 생기기 때문에 위험성이 높음 -> 동시성, 동기화

> 찰떡비유

> 한 가구 -> process
> 집 -> process의 할당 된 가상 메모리(virtual memory)
> 구성원 -> thread
> 각자 개인의 방 -> stack (thread local storage)
> 거실, 화장실, 부엌 등 공용공간 -> heap, data, code
