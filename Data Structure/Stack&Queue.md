# Stack & Queue
## Stack(스택)
LIFO(Last In First Out, 후입선출) : 가장 나중에 들어간 원소가 가장 먼저 나옴

ex) 함수의 call stack, 연산자 후위표기법 등

스택에는 스택 포인터(SP)가 존재하며, 처음에는 -1의 위치에 있다. push()를 하게되면 스택 포인터가 가리키는 위치가 증가하고, pop()을 하게되면 위치가 감소한다.

![그림0](https://user-images.githubusercontent.com/70699213/166485733-3abf6ff2-2a44-45ac-bcc3-2049537f9f16.jpg)

## Queue(큐)
FIFO(First In First Out, 선입선출) : 가장 먼저 들어간 원소가 가장 먼저 나옴

ex) Buffer, BFS 등

큐에서 가장 첫 원소를 front, 끝 원소를 rear라고 부른다.

큐는 rear로 들어와서 front로 빠져나가는 특징을 가진다.

큐에 원소를 삽입하면 rear가 한 칸 전진하고, 큐의 원소를 빼면 front가 한 칸 전진한다.

다음 그림은 큐가 비어있는 상태에서 front와 rear가 둘 다 0을 가리키고 있는 상태이다.

![그림1](https://user-images.githubusercontent.com/70699213/166471574-302e9a2e-f0e6-4ad1-a251-7498f0f4f1d3.jpg)

만약 5개의 원소가 큐에 들어오고, 2개의 원소가 빠져나가면, front와 rear는 다음 그림과 같다.

![그림2](https://user-images.githubusercontent.com/70699213/166471692-4f0225f2-01f4-40f3-80eb-eb80b7a7b1b5.jpg)


큐의 여러 메소드 중 isFull()이라는 메소드는 큐가 꽉 차있는지 확인하는 메소드이며, rear가 (큐의 사이즈-1) 과 같아지면 가득찬 것이다.

![그림3](https://user-images.githubusercontent.com/70699213/166471718-ed5fba49-96e6-43a2-bd07-29cdb74dfcf4.jpg)



`일반 큐의 단점: rear가 끝에 도달했을 때 큐에 빈 공간이 남아 있어도, 꽉 차있는것으로 판단할 수도 있다.`

이것을 개선한 것이 바로 **'원형 큐'** 이다.

![그림4](https://user-images.githubusercontent.com/70699213/166482783-84f2c94e-e0f2-4c5d-a813-499a1f8a446d.jpg)


논리적으로 배열의 처음과 끝이 연결되어 있는 것으로 간주하며 (index + 1) % size로 순환시킨다.

원형 큐도 초기에 front와 rear가 0이다.

원형 큐도 isFull()이라는 메소드를 통해 큐가 꽉 차있는지 확인할 수 있으며, (rear+1)%size가 front와 같으면 가득찬 것이다.

`원형 큐의 단점: 메모리는 잘 활용하지만, 배열로 구현되어 있기 때문에 큐의 크기가 제한적이다.`

이것을 개선한 것이 바로 **'연결리스트 큐'** 이다.

![그림5](https://user-images.githubusercontent.com/70699213/166484395-0b70f9bd-56e6-439d-b7a9-2b5f9891c69e.jpg)

연결리스트 큐는 크기의 제한이 없으며, 삽입과 삭제가 시간복잡도 O(1)으로 구현이 가능하며 편리하다.

참고: https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Data%20Structure/Stack%20%26%20Queue.md