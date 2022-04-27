# 해시 충돌

### 2022-04-27 (wed) 안영진

### Contents

1. [해시의 탄생 배경](#1-해시의-탄생-배경)
2. [해시 함수](#2-해시-함수)
3. [그래서 해시를 어떻게 활용하는데?](#3-그래서-해시를-어떻게-활용하는건데)
4. [해시 충돌](#4-해시-충돌) 
    
    4.1 [해시충돌 해결방법](#41-해시충돌-해결방법)

    4.2 [뭐가 더 나아?](#42-뭐가-더-나아)

</br>    
</br>

# 1. 해시의 탄생 배경

## **해시란?**

> **해시**(**hash**)란 다양한 길이를 가진 데이터를 고정된 길이를 가진 데이터로 매핑(mapping)한 값이다.
 

**데이터의 저장과 검색을 빠르게 하기위해** 해시를 생각하게됨

우선 데이터를 읽고 쓰는데 가장 쉽게 고려 할 수 있는 </br>
**배열**의 경우 데이터 읽고 쓰기가 O(n)이 걸림

![Untitled](https://user-images.githubusercontent.com/31895069/165303812-fefde74b-d13b-4912-a769-4f7b46a7b4c9.png)

- 해당 위치까지 가야하기 때문에 n번의 반복이 생기기 때문
    
</br>

**트리**도 빠르긴 한데 평균적으로 O(logN)이 걸림

<center><img src="https://user-images.githubusercontent.com/31895069/165303881-d3e4f0f6-e029-4ebf-a7fc-3f390c4d91f1.png" width="500" /></center>

</br>

**해시**를 이용한 자료구조는 평균적으로 O(1)(상수시간)이 걸림

어떻게 이게 가능한 것일까??

</br>
</br>

# 2. **해시 함수**

아까
> **해시**(**hash**)란 **다양한 길이를 가진 데이터**를 **고정된 길이를 가진 데이터**로 매핑(mapping)한 값이다.

라고 했다. (우선 해시값은 정수라고 봐도 된다. (이유는 나중에 설명))

어쨌든 임의의 데이터(I**nput**)가 어떠한 과정을 거쳐서 고정된 길이의 정수 데이터(**Output**)로 변했다는 뜻인데, </br> 이 어떠한 과정을 **해시 함수**라고 한다. 

<center><img src="https://user-images.githubusercontent.com/31895069/165303956-3b39a0c9-bd60-444b-92d1-18c5c674633a.png" width="500" /></center>


예시로 위의 그림처럼 임의의 데이터가 들어오면 32bit의 정수로 바꿔주는 해시함수가 있다.

해시함수는 종류가 엄청나게 많고 다양하다(64bit, 128bit ...) 자기가 짜기 나름이다.

다만,

1. 해시 계산이 간단해야하고
2. 결과 값이 고르게 나와야한다.

이 두 가지를 만족하는게 **좋은 해시함수**이다.

</br>
</br>

# 3. 그래서 해시를 어떻게 활용하는건데?

우리는 **해시 함수**를 통해 **해시(정수값)를** 만들었다.

## 해시 테이블

> **배열**과 **해시 함수**를 이용해 (Key, Value)형식의 데이터를 읽고 쓰는 **자료구조**
> 

(Java의 HashMap, python의 Dictionary와 비슷합니다.)

여기 8만큼의 크기를 가진 배열이 있다. (배열의 공간 하나를 **Bucket**이라 함)

<center><img src="https://user-images.githubusercontent.com/31895069/165304003-f255d455-b37e-4a2c-a7c4-7d94bdb32b68.png" width="300" height="400"/></center>

그리고 전화번호(key)를 넣으면 숫자로 임의의 숫자로 바꿔주는 

해시함수가 있다.

<center><img src="https://user-images.githubusercontent.com/31895069/165304052-b6c7fea2-d14c-4d9b-89f8-eb5ada5e84be.png" width="400" height="350"/></center>

어떻게 배열안에 데이터를 집어 넣을 수 있을까?
<center><img src="https://user-images.githubusercontent.com/31895069/165304077-bae79647-16e5-4265-98ec-efa5c133b478.png" width="800"/></center>


**mod 연산**을 해주면 된다. ~~(mod 연산이 없는 해시 함수도 있음 근데 대부분 mod연산 많이 쓰더라)~~

<center><img src="https://user-images.githubusercontent.com/31895069/165304122-2d69d76d-f618-4e79-b903-5b408d37b727.png" width="800" /></center>

이게 해시 테이블

데이터의 key를 가지고 해시함수를 거친다음 해당 해시값을 테이블의 크기(capa)로 **mod 연산**해준 값이 **데이터가 저장 될 배열의 인덱스**인것 (이 때문에 해시값이 정수여야함)

따라서 해시 함수 과정만 거치므로, **O(1)의 시간**으로 데이터를 읽고 쓰는게 가능한 것이다.

</br>
</br>

# 4. 해시 충돌

아까 해시란

> **해시**(**hash**)란 **다양한 길이를 가진 데이터**를 **고정된 길이를 가진 데이터**로 매핑(mapping)한 값이다.
> 라고했다.

**다양한 길이**를 가진 데이터 → (**무한**함)

**고정된 길이**를 가진 데이터 → (**유한**함)

해시함수는 생각해보면 **무한한** 데이터를 **유한한** 공간에다가 욱여 넣는 것임

<center><img src="https://user-images.githubusercontent.com/31895069/165304173-1275bec6-86d0-4bd5-bb6e-447058e41f66.gif" width="300" /></center>

**비둘기 집의 원리**

<center><img src="https://user-images.githubusercontent.com/31895069/165304198-7693256f-a463-4350-8b2d-4e43bc3d9a2b.png" width="500" /></center>

n+1 마리의 비둘기를 n개의 집에 넣는다고 한다면 어떻게 될까?

<center><img src="https://user-images.githubusercontent.com/31895069/165304220-e07c0719-a375-4cb7-bec8-c888bce75cbb.png" width="300"/></center>

비둘기 집들 중 하나는 **무조건 비둘기 2마리가** 들어가게 됨 

다시 해시로 돌아와서,

**무한한 데이터**를 **유한한 해시 테이블**에 넣기 때문에 **데이터가 겹치는게 반드시 발생 한다**는 뜻임

이렇게 겹치는것을 **해시 충돌**이라고 함

해시충돌이 나는 경우는 2가지가 있다.

<center><img src="https://user-images.githubusercontent.com/31895069/165304261-082524f8-43b1-4a05-90b9-e095e888420f.png" width="800" /></center>

key값도 해시값도 다른데 mod연산 값이 같을 때,

<center><img src="https://user-images.githubusercontent.com/31895069/165304356-326ebce3-25b5-4d6f-bdd0-d49537de4f12.png" width="800" /></center>

key값이 다른데 hash가 같을 때

</br>
</br>

# 4.1 해시충돌 해결방법

## **1. Open Addressing** 방식

쉽게말해, 충돌이 일어나면 데이터가 들어있지 않은 빈 Bucket을 찾는것임

빈 Bucket을 탐색(probe)하는 방법이 매우 다양함

</br>

**Linear Probing** (선형 탐사)

충돌이 일어난 버킷의 1칸뒤 or 2칸뒤 등등 이렇게 선형적으로 탐사하는 방법

<center><img src="https://user-images.githubusercontent.com/31895069/165304421-eb1ad369-0dec-400f-965b-128e245d49d2.png" height="350"/>
<img src="https://user-images.githubusercontent.com/31895069/165304440-4495a084-ae81-4758-b50d-8d6b37d9f780.png" height="350"/></center>

한 데이터를 넣기위해 **앞 단에서 겪었던 체크 과정을 다 거쳐야한다**는 단점이 있음

특정영역에 데이터가 뭉쳐지는 **cluster** 문제도 있음

</br>

**Quadratic Probing** (이차원 탐사)

선형 탐사의 문제를 줄이고자해서 나온 방법

버킷 탐사 범위를 **제곱수 간격**으로 넓혀감

선형 탐사에 비해서 Cluster가 줄었음

</br>

**Double hashing** (이중 해싱)

2개의 해시 함수를 이용한다는거다.

하나는 최초의 해시값을 얻을 때

다른 하나는 해시 충돌 시 탐사 할 이동폭을 얻기위해 사용

(두 번째 해시값 까지 같을 확률은 매우 적다!)

</br>
</br>

## **2. Separate Chaining** 방식

bucket 하나하나를 linked list로 보는것임 충돌이 일어나면 데이터를 이어 붙임

<center><img src="https://user-images.githubusercontent.com/31895069/165304468-130e62bc-0455-47db-b5ac-dea5b1501aad.png" width="400" /></center>

한 버킷에서 충돌이 많이 일어나면 그만큼 링크드 리스트가 길어지게되고

데이터를 탐색하는데 O(N) 까지 갈 수 도있다.

하지만 해시함수도 엄청 좋아졌고 데이터가 계속 늘어나면 해시 리사이징을 하기 때문에

사실상 O(N)까지 갈 일은 없다고 봐도 된다.

</br></br>


# 4.2 뭐가 더 나아?

**CPython**은 **Open addressing** 방식을 사용하고있고,

- **체이닝 시 malloc으로 메모리를 할당하는 오버헤드가 높아 오픈 어드레싱을 택했다.**  라고 cpython 공식문서에  적혀있다고 한다.

**Java**의 **HashMap**은 **Separate Chaining** 방식을 사용한다.

- HashMap은 상대적으로 **remove호출이 빈번**한데, **open addressing방식에서는 데이터 삭제 로직을 효율적으로 구현하기 어렵다**는 점, **S.C** 방식은 **보조해시 함수**가 있어서 충돌이 잘 발생 하지 않도록 조절 할 수 있다는 점 때문에 **S.C**방식을 쓴거 같다.

<center><img src="https://user-images.githubusercontent.com/31895069/165304494-52a99069-815b-4666-ad3c-e5b67135ec8c.png" width="600" /></center>

**Load factor**는 테이블에 데이터가 차있는 정도다 0.5는 50%정도 데이터가 들어있다는 뜻)
</br>
**number of probes**는 탐사 횟수, 이 값이 커질 수록 탐사 횟수가 많아지므로 성능이 떨어진다고 이해하시면 됩니다.


데이터가 테이블에서 적을 때는 **open addressing** 방식이 성능이 좋다고 한다.

- 연속된 공간에 데이터를 저장하여 캐시효율을 높일 수 있다고한다.

그러나 데이터가 많아지게되면 **open addressing** 방식들은 급격하게 성능이 떨어지는게 보인다.

그에 비해 separate 체이닝 방식은 선형적으로 증가하는 그래프를 볼 수 있다.


# Reference

[*https://www.youtube.com/watch?v=ZBu_slSH5Sk*](https://www.youtube.com/watch?v=ZBu_slSH5Sk)

[*https://www.youtube.com/watch?v=Rpbj6jMYKag*](https://www.youtube.com/watch?v=Rpbj6jMYKag)

[*https://d2.naver.com/helloworld/831311*](https://d2.naver.com/helloworld/831311)

[*https://ohtaeg.tistory.com/7*](https://ohtaeg.tistory.com/7)

[*https://recordsoflife.tistory.com/205*](https://recordsoflife.tistory.com/205)

[*https://zin0-0.tistory.com/294*](https://zin0-0.tistory.com/294)

[*https://esoongan.tistory.com/193*](https://esoongan.tistory.com/193)

[*https://siyoon210.tistory.com/85*](https://siyoon210.tistory.com/85)

[*https://ejyoo.tistory.com/72*](https://ejyoo.tistory.com/72)

[*https://it-license.tistory.com/72*](https://it-license.tistory.com/72)

[*https://ihp001.tistory.com/90*](https://ihp001.tistory.com/90)

[*https://velog.io/@injoon2019/자료구조-해시-테이블*](https://velog.io/@injoon2019/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%95%B4%EC%8B%9C-%ED%85%8C%EC%9D%B4%EB%B8%94)

[*https://odol87.tistory.com/4*](https://odol87.tistory.com/4)

[*https://greatzzo.tistory.com/58*](https://greatzzo.tistory.com/58)
