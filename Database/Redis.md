# Redis

<aside>
💡 키-값 구조의 비정형 데이터 전용 DBMS (NoSQL)

</aside>

- `인메모리` 데이터 저장소 (속도↑)
- `Key-Value` 구조 (쿼리 사용할 필요 X)
- `다양한 자료구조` 지원
- `Single Thread` 사용
- DB / 캐시 / 메시지브로커로 사용
- 오픈소스

## DB서버 VS 캐시서버

- 기존의 DB서버
    - 소규모 서비스(WEB-WAS-DB 구조)
    - 데이터를 **물리 디스크**에 직접 접근해 write
    - 서버가 다운되도 데이터 손실 x
    - **DB부하** 큼
- 기존의 캐시서버
    - 한번 읽어온 데이터를 임의의 공간에 저장해, read시 **빠르게** 결과값 받음
    - 같은 요청이 여러번 들어올 경우, DB까지 가지 않아도 캐시서버 단에서 저장된 결과값 반환
    - 서버가 다운되면 **데이터 손실**
    - ~~DB부하~~ 적음
    
    - 캐시 서버의 두가지 패턴
        
        ## 1. Look aside cache
        
        1. 클라이언트가 데이터 요청
        2. 웹서버: cache서버에서 데이터 유무 확인
        3. 있을 경우: DB에 도달하지 않고, cache의 결과값을 클라이언트에 반환 (Cache Hit)
        4. 없을 경우: DB에서 데이터 조회해 클라이언트에 반환 & cache 서버에 저장 (Cache Miss)
        
        ## 2. Write Back
        
        1. 웹서버: 모든 데이터를 cache 서버에 저장
        2. cache 서버: 특정시간동안 데이터 저장
        3. DB : cache서버에 있는 데이터를 저장
        4. cache 서버: DB에 저장완료된 데이터를 삭제 
        - insert 쿼리를 모아서 한번에 날려 속도 ↑

<aside>
💡 Redis = 캐시서버 + `영속성` 지원 + `다양한 데이터구조` 지원

</aside>

## Redis: 영속성

인메모리 데이터 저장소이지만, 영속성을 지원한다. 

이를 위해 데이터를 디스크에 저장할 수 있고,

서버가 내려가더라도 디스크로부터 읽어 메모리에 로딩할 수 있다.

디스크에 데이터 저장하는 방식 

1. RDB (Snapshotting) 방식
    
    : 순간적으로 메모리에 있는 모든 내용을 디스크에 옮겨 저장함.
    
2. AOF (Append On File) 방식
    
    : Redis의 모든 write/update 연산을  log 파일에 통째로 기록함.
    

## Redis: 다양한 데이터구조

- String, Lists, Sets, Sorted Sets, Hashes 자료 구조를 지원한다.
    - String : 일반적인 key-value 형태
    - Sets: String의 집합. 여러개의 값을 하나의 value 에 넣을 수 있음
- 메모리 저장소임에도 많은 데이터구조를 지원해 다양한 기능을 구현할 수 있다.

![Untitled](https://user-images.githubusercontent.com/67628725/181292367-89f0de58-415e-4707-b86c-43ea7f2210b2.png)


## Redis 프로젝트

- Redis Replication:   Master-Slave 형식의 데이터 이중화 구조 처리
- Redis Cluster: 분산처리
- Redis Sentinel: 장애복구 시스템
- Redis Topology
- Redis Sharding
- Redis Failover

- 읽기성능: 증대를 위해 서버 측 복제를 지원
- 쓰기성능: 클라이언트 측 샤딩(Sharding)지원
