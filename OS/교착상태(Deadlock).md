![교착상태](https://velog.velcdn.com/images/yhd520/post/8d3b086b-1967-4af7-ba53-04a349d49a2a/image.png)
## 교착상태(Deadlock)

두 개 이상의 작업이 서로 상대방의 작업이 끝나기 만을 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태를 가리킨다. 
즉, 둘 이상의 프로세스들이 자원을 점유한 상태에서 서로 다른 프로세스가 점유하고 있는 자원을 요구하며 무한정 기다리는 현상을 의미합니다.
![순환 대기](https://velog.velcdn.com/images/yhd520/post/a2746818-a8e9-43c6-98f6-e1955778d72c/image.png)

#### 자원(Resource)이란?
- 하드웨어, 소프트웨어 등을 포함하는 개념
- ex) I/O device, CPU cycle, memory space 등
- 프로세스가 자원을 사용하는 절차
  - Request(요청), Allocate(할당), Use(사용), Release(반납)
  
## 데드락의 발생 조건 네 가지
아래 네 가지 조건을 모두 만족해야 데드락이 발생한다. 어느 한 가지 조건이라도 풀리면 데드락을 방지할 수 있다.
- Mutual exclusion(상호 배제)
  - 매 순간 하나의 프로세스만이 자원을 사용할 수 있음
- No preemption (비선점)
  - 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음
- Hold and Wait (점유와 대기)
  - 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음
- Circular wait (순환 대기)
  - 자원을 기다리는 프로세스간에 사이클이 형성되어야 함



### 자원 할당 그래프
- R -> P : 자원이 R이 프로세스 P에 할당됨
- P -> R : 프로세스 P가 자원 R을 요청함(필요로 함)
- R안의 점의 개수 : 자원의 인스턴스 개수 즉, 해당 자원을 사용할 수 있는 프로세스의 수
![자원 할당 그래프](https://velog.velcdn.com/images/yhd520/post/de806d5d-efe2-4da8-b658-134a20101f9d/image.png)
- 왼쪽 그래프: 자원을 기다리는 프로세스간의 사이클이 형성되었기 때문에 데드락 발생
- 오른쪽 그래프: 사이클이 형성되어 보이지만 P2와 P4가 작업을 끝내고 자원을 반납할 수 있기 때문에 데드락이 형성되지 않음

## 데드락의 방지/처리 방법
### Deadlock Prevention (예방 기법)
자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것, 자원의 낭비가 가장 심한 기법

- No preemption (비선점)
  - process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
  - 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다.
  - 모든 자원에서 사용하기 어렵고 state를 쉽게 save하고 restore할 수 있는 자원에서 사용 가능하다. (CPU, memory)
- Hold and Wait (점유와 대기)
  - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.
  - 방법1. 프로세스 시작 시 모든 필요한 자원을 할당받게 하는 방법
  - 방법2. 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청
  - 자원 과다 사용으로 인한 비효율, 프로세스가 요구하는 자원을 파악하는 데에 대한 비용, 자원에 대한 내용을 저장 및 복원하기 위한 비용등의 문제가 있다.
- Circular wait (순환 대기)
  - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당
  - 예를 들어 자원2을 얻기위해서는 자원1을 가지고 있어야한다는 조건을 부여한다.
### Deadlock Avoidance (회피 기법)
프로세스의 시작부터 종료까지 필요로하는 모든 자원에 대한 정보를 미리 파악하고 그 것을 이용해서 자원 할당시 deadlock 발생의 가능성을 판단하여 안전한 경우에만 자원을 할당한다.
2개의 avoidance 알고리즘을 경우에 따라 사용
- 자원의 인스턴스가 1개인 경우
  - [자원 할당 그래프 알고리즘](https://www.geeksforgeeks.org/resource-allocation-graph-rag-in-operating-system/)
- 자원의 인스턴스가 여러개인 경우
  - [은행원 알고리즘](https://en.wikipedia.org/wiki/Banker%27s_algorithm)
  
### Deadlock Detection and Recovery (발견 기법)
Deadlock에 대한 방지를 따로 하지 않고 Deadlock이 발생했는지 검사하고 발견시 처리하는 방법
- Deadlock Detection
  - 자원 할당 그래프에서 사이클이 생성되었을 경우 Deadlock발생
- Deadlock Recovery
  - Deadlock이 발생된 모든 프로세스를 종료시키는 방법
  - 하나씩 종료시키면서 Deadlock이 사라지는지 확인하는 방법
  - 프로세스의 자원을 내놓게 하는 방법
  
### Deadlock Ignorance (무시 기법)
Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않는다.
- Deadlock이 매우 드물게 발생하므로 Deadlock이 대한 조치 자체가 더 큰 overhead일 수 있다.
- 만약, 시스템에 Deadlock이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 process를 죽이는 등의 방법으로 대처
- UNIX, Windows 등 대부분의 범용 OS가 채택