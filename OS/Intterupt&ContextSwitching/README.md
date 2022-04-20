# [OS]인터럽트&Context Switching

# Interrupt

CPU 가 프로그램 실행 중, `예외상황 처리`를 위해  `프로세스를 잠시 중단`하는 것 

CPU는, 한번에 하나의 프로세스를 처리하기 때문에, 이를 핸들링하기 위해 필요 

CPU가 다른 장치와 통신하기 위해 필요. 

- [내부] SW 인터럽트
    - overflow / underflow / 0 division
- [외부] HW인터럽트
    - 전원 이상 / IO관련 / 기계이상 ...

### Interrupt 동작방식

: Interrupt 이벤트마다 각 ‘실행코드’ 를 가리키는 주소를 

  `IDT(Interrupt Descriptor Table)` 에 기록.

  (컴퓨터 부팅 시, OS가 기록)

- 0~31: 예외상황 인터럽트 (내부/ 소프트웨어 인터럽트)
- 32~47: 하드웨어 인터럽트 (주변 장치의 종류 및 개수에 따라 변경 가능)
- 128: 시스템 콜

---

# Context Swithing

CPU 에서 , 프로세스를 번갈아 처리하는 작업.

여러개의 프로세스가 실행되고 있을 때 기존에 실행되는 프로세스를 중단하고 다른 프로세스를 실행 하는 것.

1. 동작중인 프로세스를 중지하며, 해당 프로세스의 상태(context)보관
2. 대기중인 다음 프로세스로 동작 교체 
3. 이전의 프로세스 상태 (context) 복구

## 발생하는 경우

- 멀티태스킹
- 인터럽트 핸들링
- 사용자 모드 & 커널 모드 간 전환

## PCB

 :Process Context Block 

 프로세스 상태값을 저장하는 별도의 메모리 공간. 

 Context Switching은 PCB에 상태값을 저장하고, 다시 찾아 복구하는 방식으로 진행됨

![Untitled](https://user-images.githubusercontent.com/67628725/164216525-d492ae77-f3dd-48d4-899f-98e3fc4a9553.png)

- Process ID (PID)
- register 값 (PC, SP 등)
- process state (프로세스 상태)
    - ex) running, block ...
- Meomory limits (메모리 사이즈)

### Context Switching 발생 시 cost

cost 큰편 

- cache 초기화
- Memory Mapping 초기화
- kernel은 항상 실행됨

---

# Process & Thread

## process

실행된 프로그램

 운영체제로부터 시스템 자원을 할당받는 작업의 단위 

 메모리에 올라와 실행되고 있는 프로그램의 인스턴스 

- 프로세스는 각각, 독립된 메모리 영역 Code / Data / Stack / Heap 구조 를 할당받는다.
- 각 프로세스는 별도의 주소공간에서 실행됨. (다른 프로세스의 변수or 자료구조에 접근 불가)
- 프로세스 당 최소 1개의 thread를 갖고있음

## thread

프로세스 내에서 실행되는 여러 흐름의 단위 

- 스레드는, stack 만 따로 할당 받고, Code / Data / Heap 영역은 공유함
- 같은 프로세스 내의 스레드들은 주소공간, 자원을 공유함

---

## 멀티 Process

ex) 웹 서버

- 장점
    - process 하나의 문제가 다른 process에 영향을 끼치지 않음
- 단점
    - 공유하는 자원 x  → Context Switching 시,  `cache 메모리 초기화`
    - 많은 오버헤드 발생
    - process간 통신 부담 ↑

## 멀티 Thread

 ex) 웹 서버

- 장점
    - 자원을 할당하는 시스템 콜 ↓   → 시스템 자원 소모  ↓
    - 공유하는 메모리 범위 큼 →  thread간 통신 부담 ↓
    - context switching 빠름
- 단점
    - 자원 공유로 인한 동기화 문제
    - thread 하나의 문제 → thread 전체의 문제
    - 단일 프로세스 시스템의 경우 장점이 줄어듦
    
    ### Process vs Thread
    
    context switching 비용 : `process > thread`
    
     thread는 stack 영역을 제외한 모든 메모리를 공유 
    
     즉, 발생시 stack 영역만 변경 
    

---

<aside>
💡 CPU는 한번에 한가지 일을 처리할 수 있다.

</aside>

<aside>
💡 Interrupt 는, CPU가 실행중인 프로세스를 [중단] 하기 위해 필요

</aside>

<aside>
💡 Context Switching 은, interrupt시, Context(진행중이던 작업정보)를 다시 불러와 [멀티태스킹] 하기 위해 필요

</aside>

<aside>
💡 Multi Threading 은, context switching 시 overhead를 최소화 하기 위해 필요

</aside>
