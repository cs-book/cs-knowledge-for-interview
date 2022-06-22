# AVL-Tree VS Red-Black-Tree

>균형 이진 탐색 트리 대표 두 가지를 알아보고 차이점을 알아보자

<br/>

## Binary Search Tree (BST)

BST, 이진탐색트리를 먼저 알아야한다.

이진탐색트리란 이진탐색과 연결리스트를 결합한 자료구조의 일종으로,<br/>
이진 탐색의 효율적인 탐색 속도를 유지하면서 빈번한 자료 입력과 삭제를 가능하게끔 고안되었다.

<br/>

### BST의 특징
- 모든 노드의 왼쪽 서브트리는 해당 노드의 값 보다 작은 값들만 가진다.<br/>
- 모든 노드의 오른쪽 서브트리는 해당 노드의 값 보다 큰 값들만 가진다.
- 중복되는 노드가 없어야 한다.

<br/>

### BST 순회 방법
BST를 순회 할 때는 중위 순회(inOrder) 방식을 쓰는데,

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Binary_search_tree.svg/1920px-Binary_search_tree.svg.png" width="300" />

> 중위 순회 방식은 재귀적으로 왼쪽 서브트리 - 현재 노드 - 오른쪽 서브트리를 순회하는 방식임

위의 트리를 중위 순회 하면 1-3-4-6-7-8-10-13-14 가 출력 되는데, <br/>
BST내부의 값들을 순서대로 출력한 것과 같다.

<br/>

### 노드의 후임자, 선임자
후임자 = 해당 노드보다 값이 큰 노드들 중에서 가장 값이 작은 노드 <br/>
선임자 = 해당 노드보다 값이 작은 노드들 중에서 가장 값이 큰 노드 <br/>
위의 그림을 예시로 들면 8의 후임자는 10이고 8의 선임자는 7이 될 것 이다.

<br/>


### 이진탐색트리의 시간복잡도
|   | 검색 | 삽입 | 삭제 |
| - | --- | ---- | --- |
| Average | O(logN) | O(logN) | O(logN) |
| Worst | O(N)| O(N)| O(N) |


<img src="https://user-images.githubusercontent.com/31895069/174824063-0768bc35-684c-43d0-90a8-5ba81d319421.png" width="300" />

검색을 할 수록 데이터가 절반 씩 줄어드므로, 평균 O(logN)이라는 걸 알 수 있다.

그러나

<img src="https://user-images.githubusercontent.com/31895069/174824596-fc193cd5-820d-4ce1-964d-d6599197b2d7.png" width="200" />

다음과 같이 한 쪽으로 치우친 모양이 나오게 된다면 그저 List와 다른게 없으므로 <br/>
O(N)이 나오는걸 알 수 있다.

이런 단점을 해결하기위해 만들어진게 바로 **균형 이진 탐색 트리** 다.

<br/>

### 균형 이진 탐색 트리에 들어가기 전에!!
트리의 회전이란 개념을 알아야한다.

<img src="https://azrael.digipen.edu/~mmead/www/Courses/CS280/RotateRight-1.gif" width="600" />

오른쪽 회전

<img src="https://azrael.digipen.edu/~mmead/www/Courses/CS280/RotateLeft-1.gif" width="600" />

왼쪽 회전

높이가 더 큰 쪽, 빗대어서 말하자면 무게가 무거운 쪽을 가볍게 해줘서 균형을 맞춘다 라고 이해해도 좋다.

회전 연산은 단순히 레퍼런스를 바꿔주는 것이기 때문에 상수시간이 걸린다.

이 트리 회전을 통해서 트리의 균형을 맞추는것이다.

<br/>
<br/>

## AVL Tree
이진탐색트리의 한 종류로, Balance Factor를 통해서 스스로 균형을 잡는 트리이다.

>Balance Factor란 임의의 노드 X에 대해서 <br/>X의 왼쪽 서브트리의 높이와 X의 오른쪽 서브트리의 높이의 차이를 계산 한 값.

AVL Tree에서는 모든 노드의 Balance Factor값이 -1 또는 0 또는 1 이어야 한다. <br/>
만약, 다른 값이라면 트리의 균형을 맞춰주는 과정이 일어나게 된다.


### AVL트리의 시간복잡도
|   | 검색 | 삽입 | 삭제 |
| - | --- | ---- | --- |
| Average | O(logN) | O(logN) | O(logN) |

검색 과정은 BST와 동일 하다.

### 삽입 (24 노드를 삽입)

<img src="https://user-images.githubusercontent.com/31895069/174837749-8a94205f-190c-4e42-be88-e0df0eaa1f67.gif" width="400" />


<img src="https://user-images.githubusercontent.com/31895069/174840597-105ae6a0-ae82-4071-85da-0b82c1a9e09f.png" width="400" />

14번 노드에서 왼쪽 서브트리의 높이는 0 오른쪽 서브트리의 2이므로 <br/>
Balance Factor가 -2가 나오므로 조정이 필요하다.

<br/>


### 삭제 (79 노드를 삭제)

<img src="https://user-images.githubusercontent.com/31895069/174838781-eb50a690-5517-4a35-89bc-8d5c4e866169.gif" width="400" />


<img src="https://user-images.githubusercontent.com/31895069/174839937-2b4ffe58-1988-4203-aec2-409c2bb58aac.png" width="400" />

57번 노드에서 왼쪽 서브트리의 높이는 2 오른쪽 서브트리의 0이므로 <br/>
Balance Factor가 2가 나오므로 조정이 필요하다.

삽입, 삭제가 일어난 노드부터 루트 노드까지 거슬러 가고 그 과정마다 Balance Factor를 확인하고 트리구조를 조정하는 것이 반복 된다는것을 알아야한다. <br/>
즉, AVL은 엄격하게 균형을 조정하나 반복적인 작업 때문에 시간이 다소 소요된다.
이러한 점을 개선하기 위해 Red-Black-Tree가 나오게 되었다.

(AVL트리의 삽입과 삭제과정에는 여러 케이스가 있기 때문에 따로 더 알아보기 바란다.)

<br/>

## Red-Black-Tree
이진탐색트리의 한 종류로, 다섯가지의 RB트리 속성을 만족하여 스스로 균형을 잡는 트리이다.

### 사전 개념
>nil노드 = RB트리에서 leaf노드를 nil노드라고 한다

<img src="https://user-images.githubusercontent.com/31895069/174845247-0db9acf2-eb42-42a8-ad4c-c860e08d5aa7.png" width="600" />



### RB트리의 속성
1. 모든 노드는 Red or Black 색상
2. 루트 노드는 무조건 Black
3. 모든 nil 노드는 Black
4. Red의 자녀들은 Black, 즉 Red가 연속적으로 존재 할 수 없다.
5. 임의의 노드에서 nil 노드까지 가는 경로에서의 Black 노드의 수는 같다.

5번 속성에서의 Black 노드의 수를 Black Height(BH) 라고 한다.

즉 BH의 조건때문에 시간복잡도 logN을 만족하게 되는것이다.

<img src="https://mblogthumb-phinf.pstatic.net/20110328_24/albertx_13012483354979uAP0_PNG/img_2.png?type=w2" width="400" />

(실제로는 이렇게 구현됨)

### RBT 삽입/삭제 절차
1. 삽입/삭제 전 RB 속성 만족한 상태
2. 삽입/삭제 방식은 일반적인 BST와 동일
3. 삽입/삭제 후 RB속성 위반여부 확인
4. 재조정
5. RB 속성 만족


### Red-Black 트리의 시간복잡도
|   | 검색 | 삽입 | 삭제 |
| - | --- | ---- | --- |
| Average | O(logN) | O(logN) | O(logN) |


검색 과정은 BST와 동일 하다.


### 삽입 (삽임되는 노드의 색은 무조건 Red)
<br/>
10번 노드 삽입

<img src="https://user-images.githubusercontent.com/31895069/174856662-b079fb4d-db2a-4f5e-a0a0-06713d6aaef7.gif" width="600" />

<br/>
12번 노드 삽입

<img src="https://user-images.githubusercontent.com/31895069/174856669-1c379ff4-1f13-4844-8a76-1b44f17482c2.gif" width="600" />

노드가 삽입 된 후 위의 5가지 속성 위반여부를 확인하고 트리를 재조정 하는 것을 볼 수 있다.

<br/>

### 삭제
<br/>
11번 노드 삭제

<img src="https://user-images.githubusercontent.com/31895069/174857659-d65386fd-041c-4f20-92ee-9de66394afc2.gif" width="600" />

<br/>
12번 노드 삭제

<img src="https://user-images.githubusercontent.com/31895069/174857669-c359cac9-79e9-45ed-84c6-1b8f903ffe08.gif" width="600" />

노드가 삭제 된 후 위의 5가지 속성 위반여부를 확인하고 트리를 재조정 하는 것을 볼 수 있다.

(RB트리의 삽입과 삭제과정에는 여러 케이스가 있기 때문에 따로 더 알아보기 바란다.)
 

RBT가 O(logN)임을 귀납적으로 증명
<img src="https://user-images.githubusercontent.com/31895069/174993319-bb22953e-d3e4-4a65-9f3b-11225e859347.png" width="600" />

<br/>
<br/>
<br/>

## AVL-Tree VS Red-Black-Tree

|      | AVL-Tree | Red-Black-Tree | 
| ---- | ---- | ---- | 
| 삽입/삭제 성능 | RB-Tree에 비해 느리다 | AVL-Tree에 비해 빠르다 | 
| 검색 성능 | RB-Tree에 비해 빠르다 | AVL-Tree에 비해 느리다 | 
| 응용 사례 | dictionary, <br/> 한 번 만들어 놓으면 삽입/삭제가 거의 없고 <br/> 검색이 대부분인 상황에서 사용 | linux kernel 내부에서 사용, <br/> java TreeMap, HashMap 충돌 시 <br/> c++ std::map |

AVL트리는 삽입 삭제 후 루트까지 거슬러 가면서 Balance factor를 확인하고 조정하는 과정을 
반복한다. 즉 엄격하게 균형을 맞추기때문에 트리 회전 연산이 많아 시간이 많이 소요됨. <br/>
그에 비해 RBTree는 비교적 덜 엄격하게 균형을 맞춤 예를들어 단순히 색상만 바꿔서 균형을 맞추는 경우 등등 

그렇기에 삽입/삭제가 빈번한 경우에는 RB-Tree가 더 적합하다.

균형이 더 엄격하기 때문에 검색면에서는 AVL-Tree가 더 적합하다.

<hr/>


## Reference

https://ratsgo.github.io/data%20structure&algorithm/2017/10/28/rbtree/ <br/>
https://m.blog.naver.com/min-program/221231697752 <br/>
https://ebongzzang.github.io/algorithm/Red-Black-tree-%EA%B7%B8%EB%A6%AC%EA%B3%A0-AVL-tree%EC%99%80%EC%9D%98-%EB%B9%84%EA%B5%90/ <br/>
https://jwdeveloper.tistory.com/280 <br/>
https://devidea.tistory.com/36 <br/>
https://lgphone.tistory.com/90 <br/>
https://suhwanc.tistory.com/197?category=730826 <br/>
https://ferrante.tistory.com/46 <br/>

## Simulator
https://www.cs.usfca.edu/~galles/visualization/RedBlack.html <br/>
https://www.cs.usfca.edu/~galles/visualization/AVLtree.html <br/>