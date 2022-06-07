# Content

- [트랜잭션 격리 수준](#트랜잭션-격리-수준)
- [Read Uncommitted](#read-uncommitted)
- [Read Committed](#read-committed)
- [Repeatable Read](#repeatable-read)
- [Serializable](#serializable)

## 트랜잭션 격리 수준

> **트랜잭션 격리 수준은 여러 트랜잭션이 동시에 수행될 때 한 트랜잭션에서 갱신된 데이터를 다른 트랜잭션에서 조회 시 어떻게 보일지를 결정하는 기준입니다.**

데이터베이스의 격리 수준은 Read Uncommitted, Read Committed, Repeatable Read, Serializable 4가지로 나뉩니다. 뒤로 갈수록 트랜잭션 간의 격리 수준은 높아지고 동시 처리 성능은 낮아집니다. 하지만 Serializable 격리 수준이 아니라면 성능의 저하는 크지 않습니다.

일반적으로 Read Committed, Repeatable Read 격리 수준을 사용합니다. MySQL의 경우 디폴트 격리 수준이 Repeatable Read 입니다.

### 격리 수준에 따른 데이터 부정합 문제

각 격리 수준에 따라 3가지의 데이터 부정합 문제가 발생할 수 있습니다.

| 격리 수준        | Dirty Read | Non-Repeatable Read |  Phantom Read  |
| ---------------- | :--------: | :-----------------: | :------------: |
| Read Uncommitted |     O      |          O          |       O        |
| Read Committed   |     -      |          O          |       O        |
| Repeatable Read  |     -      |          -          | O (InnoDB는 X) |
| Serializable     |     -      |          -          |       -        |

- MySQL의 스토리지 엔진으로 InnoDB를 사용하면 갭 락, 넥스트 키 락 덕분에 Repeatable Read 격리 수준에서도 Phantom Read가 발생하지 않습니다.

## Read Uncommitted

Read Uncommitted 격리 수준에서는 트랜잭션이 Commit되거나 Rollback되지 않더라도 작업 내용이 다른 트랜잭션에서 보이게 됩니다. 이러한 현상을 Dirty Read라 합니다. Dirty Read로 인해서 데이터 정합성에 문제가 발생하므로 최소한 Read Committed 이상의 격리 수준을 사용할 것을 권장합니다.

### [그림 1] Dirty Read 문제가 발생함

![image](https://user-images.githubusercontent.com/68716284/172378027-393bf03f-e391-4ce2-9912-2ac122a6151b.png)

## Read Committed

Read Committed 격리 수준은 가장 많이 선택되는 격리 수준입니다. 그리고 Read Uncommitted 격리 수준의 Dirty Read 문제가 발생하지 않습니다. Commit이 완료된 데이터만 조회할 수 있기 때문입니다. 이 때 Undo 영역을 활용합니다.

아래 그림과 같이 Read Committed 격리 수준에서 데이터를 갱신할 경우 Undo 영역에 데이터를 백업합니다. 다른 트랜잭션에서 갱신 중인 데이터를 참조할 경우 Undo 영역에 백업된 데이터를 참조합니다.

### [그림 2] Dirty Read 문제가 발생하지 않음

![image](https://user-images.githubusercontent.com/68716284/172379896-69fe1246-b01a-4fce-bcda-008ea4b4423f.png)

그런데 또 다른 문제인 Non-Repeatable Read가 발생합니다. 이러한 현상 때문에 Read Committed 격리 수준에서는 트랜잭션 중간에 참조하는 데이터가 달라질 수 있습니다. 다른 트랜잭션이 Commit을 완료한 경우 Undo 영역이 아닌 원본 테이블의 값을 참조하기 때문입니다.

### [그림 3] Non-Repeatable Read 문제가 발생함

![image](https://user-images.githubusercontent.com/68716284/172382193-d0fd9769-0b81-4ef3-9908-8af59fc63750.png)

## Repeatable Read

Repeatable Read는 MySQL의 InnoDB 스토리지 엔진에서 기본으로 사용되는 격리 수준입니다. 이 격리 수준에서는 Read Committed 격리 수준에서 발생한 Non-Repeatable Read 문제가 발생하지 않습니다. Undo 영역에 백업된 데이터를 이용해 동일한 트랜잭션 내에서는 동일한 결과를 보여줄 수 있도록 보장하기 때문입니다. 이 때 트랜잭션 아이디가 사용됩니다.

모든 InnoDB의 트랜잭션은 트랜잭션 아이디를 가지는데, Undo 영역에 백업된 모든 데이터에는 트랜잭션 아이디가 포함되어 있습니다. 트랜잭션 안에서 실행되는 모든 SELECT 쿼리는 자신의 트랜잭션 아이디보다 작은 트랜잭션 번호에서 갱신된 데이터만 참조할 수 있습니다.

### [그림 4] Non-Repeatable Read 문제가 발생하지 않음

![image](https://user-images.githubusercontent.com/68716284/172387940-795c023d-8393-41c0-8405-24b4945d1e5f.png)

> 관련 키워드 : MVCC, InnoDB, Undo 영역으로 인한 성능 저하

하지만 아직 Phantom Read 문제가 남아 있습니다. 아래 그림과 같이 SELECT .. FOR UPDATE 쿼리를 사용하면 Phantom Read 문제가 발생합니다. SELECT .. FOR UPDATE 쿼리는 SELECT 하는 레코드에 쓰기 잠금을 걸어야 하는데 Undo 레코드에는 잠금을 걸 수 없기 때문에 현재 레코드의 값을 참조하게 됩니다. 따라서 현재 트랜잭션보다 늦게 시작한 트랜잭션의 레코드도 참조할 수 있게 됩니다.

### [그림 5] Phantom Read 문제가 발생함

![image](https://user-images.githubusercontent.com/68716284/172396036-db0ac41d-2999-41e7-a71c-df013ec1c410.png)

## Serializable

Serializable 격리 수준은 가장 단순하면서 엄격한 격리 수준입니다. 그리고 동시 처리 성능은 다른 격리 수준에 비해 떨어집니다. 읽기 작업을 할 때도 공유 잠금을 획득해야하기 때문입니다. 이 때 다른 트랜잭션에서 해당 레코드를 갱신할 수 없게 됩니다.

Serializable 격리 수준에서는 Phantom Read 문제가 발생하지 않습니다. 읽기 작업만으로도 해당 레코드에 잠금을 걸 수 있기 때문입니다. 그렇다고 Phantom Read 문제를 해결하기 위해 Serializable 격리 수준을 사용해야 하는 것은 아닙니다. InnoDB 스토리지 엔진에서는 갭 락과 넥스트 키 락 덕분에 Repeatable Read 격리 수준에서도 Phantom Read가 발생하지 않기 때문입니다. 따라서 굳이 Serializable 격리 수준을 사용할 필요는 없습니다.

> 관련 키워드 : 갭 락, 넥스트 키 락
