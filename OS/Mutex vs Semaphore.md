# Mutex vs Semaphore
### Mutex
- 여러 스레드를 사용하는 환경에서 자원에 대한 접근을 동기화하기 위해 사용되는 상호배제(Mutual Exclusion) 기술이다.
- Boolean 타입의 Lock 변수를 사용하여 오직 하나의 스레드만이  임계 영역(Critical Section)에 들어올 수 있다.
-  Lock을 건 스레드만 Lock을 해제할 수 있다.
- 공유자원을 사용중인 스레드가 있을 때, 다른 스레드가 접근하면 Blocking 후 대기 큐로 보낸다.(Sleeping-waiting 방식)

### SpinLock
- 대기하기 않고 자리가 있는지 계속 물어보는 방식(Busy-waiting 방식)
- 어떤 상황에서 유리한가?
  - 컨텍스트 스위칭 시간이 더 짧은 경우
  - 멀티 코어 프로세스인 경우

### Semaphore
- 세마포어는 Signaling 메커니즘이라는 점에서 뮤텍스와 다르다.
- 세마포어는 lock을 걸지 않은 스레드도 Signal을 보내 lock을 해제할 수 있다는 점에서, wait 함수를 호출한 스레드만이 signal 함수를 호출할 수 있는 뮤텍스와 다르다.
- 세마포어는 동기화를 위해 wait와 signal이라는 2개의 atomic operations를 사용한다.

  - atomic operation란 멀티프로세스에서 여러 CPU들이 공유자원에 접근 할 때, 접근 순서를 직렬화하여 의도치 않는 결과를 피하는 operation을 뜻한다.

- 세마포어는 크게 Counting Semaphores와 Binary Semaphore 2종류로 구분할 수 있다.
  - Counting Semaphore는 카운트가 양의 정수값을 가지며, 설정한 값만큼 스레드를 허용하고 그 이상의 스레드가 자원에 접근하면 락이 실행된다.
  - Binary Semaphore는 세마포어의 카운트가 1이며 Mutex처럼 사용될 수 있다.(뮤텍스는 절대로 세마포어처럼 사용될 수 없다.)

참고자료
- https://www.youtube.com/watch?v=oazGbhBCOfU
- https://mangkyu.tistory.com/104