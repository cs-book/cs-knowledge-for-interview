# GC(Garbage Collection)
JVM의 Heap 영역에서 사용하지 않는 **객체를 삭제하는 프로세스**를 말한다.

![image](https://user-images.githubusercontent.com/70699213/171332149-49b42b07-83a1-44ac-9ad5-9e6f9fee131b.png)

그러면 GC는 어떻게 삭제할 객체와 아닌 객체를 구분할까?

![image](https://user-images.githubusercontent.com/70699213/171332335-78c3c48a-586b-4a0f-b085-767bb66f504f.png)

GC Roots에서부터 참조하고 있는 객체들을 탐색해나간다.

GC Root가 될 수 있는 것들은 다음과 같다.
- stack 영역의 데이터
- method 영역의 static 데이터
- JNI에 의해 생성된 객체들

참조되고 있는 객체를 Reachable, 그렇지 않은 객체를 Unreachable이라고 한다.

![image](https://user-images.githubusercontent.com/70699213/171332822-f4a00772-ed74-4396-9622-59d1a1c47d5e.png)

위의 그림에서 2번 객체는 Unreachable하다.

## GC의 동작 방식

GC의 기본적인 동작 방식은 **MARK AND SWEEP** 이다. (알고리즘에 따라 COMPACT 과정이 추가되기도 한다.)

![image](https://user-images.githubusercontent.com/70699213/171334923-c290f3a4-c327-449c-b539-f7ca594a4483.png)

MARK: GC Roots로부터 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾아서 마킹한다.

![image](https://user-images.githubusercontent.com/70699213/171335067-3a09d547-ab9b-40e2-b3c7-a0a7dbe6c426.png)

SWEEP: Unreachable 객체들을 Heap에서 제거한다.

![image](https://user-images.githubusercontent.com/70699213/171335568-5571c3ee-c8cc-4020-b82a-351764138dad.png)

COMPACT: SWEEP 후에 분산된 객체들을 모아서 메모리 단편화를 막아주는 작업이다.

## GC는 언제 일어날까?

![image](https://user-images.githubusercontent.com/70699213/171335853-1094b57c-a533-4e2f-a081-4b3c4335516b.png)

Heap은 크게 Young Generation과 Old Generation으로 나누어진다.

Young Generation은 다시 Eden, Survivor0, Survivor1으로 나누어진다.

새로운 객체가 Eden에 할당되고, Eden이 꽉 차면 이때 **Minor GC**가 발생한다.

![image](https://user-images.githubusercontent.com/70699213/171336725-e735520e-e7fa-4bd4-b160-f41536537414.png)

이 때 Survivor0영역으로 살아남은 객체들이 이동을하며, 이동 시 객체들의 age가 증가한다.

![image](https://user-images.githubusercontent.com/70699213/171336908-6a64739c-e793-4e60-a824-71e80bf72705.png)

다시 Eden이 꽉 차면 이번에는 Survivor1 영역으로 이동한다.

![image](https://user-images.githubusercontent.com/70699213/171337013-6b65684d-9636-4fe7-8dd1-f64fa60a7874.png)

이 때 주의할 점은
- Survivor0과 Survivor1으로 번갈아가며 이동한다.
- Survivor0과 Survivor1 중 한 영역은 반드시 비어있어야 한다.

객체의 age가 특정 임계점에 달하게 되면, Old Generation으로 이동한다. 이러한 이동을 Promotion이라고 한다.
![image](https://user-images.githubusercontent.com/70699213/171337397-6280c7a5-b4d1-4eec-b257-d6cc4cf268a6.png)

Old Generation이 꽉 차게 되면 **Major GC**가 발생하게 된다.

![image](https://user-images.githubusercontent.com/70699213/171337941-bbaf5eda-55f5-4a09-80a9-94667ff87f46.png)

왜 Minor GC와 Major GC를 나눌까?

1. 대부분의 객체는 금방 접근 불가능한 상태가 된다. 즉, 금방 garbage가 된다.
2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

## GC의 종류
- Serial GC
  - GC를 싱글 쓰레드로 수행
- Parallel GC (Java 8의 default GC)
  - young 영역의 GC를 멀티 쓰레드로 수행
- Parallel Old GC
  - Parallel GC와 똑같지만, Old영역까지 멀티 쓰레드로 수행
- CMS GC(Concurrent Mark Sweep)
  - stop-the-world를 줄이기 위해 고안됨
  - compact과정이 없어서 메모리 단편화 문제
- G1 GC(Garbage First)
  - Heap을 일정한 크기의 Region으로 나누어 Region단위로 탐색
  - compact과정을 통해 CMS GC의 메모리 단편화 문제를 개선
  - Java 9이상 default GC

참고자료: https://www.youtube.com/watch?v=Fe3TVCEJhzo