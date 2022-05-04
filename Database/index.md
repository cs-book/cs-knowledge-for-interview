## 인덱스(index)란?

- 데이터베이스에서 테이블에 대한 동작의 속도를 높여주는 자료 구조
- 테이블에서 원하는 데이터를 쉽고 빠르게 찾기 위해 사용

ex) 책의 목차

![Untitled](https://user-images.githubusercontent.com/63090006/166668037-3edd234b-c325-49cb-972a-5efd736b0aae.png)

> **데이터베이스 파일 구성**
>
> - 테이블 생성 시, 3가지 파일 생성
>   - MYD : 실제 데이터 관리 파일
>   - MYI : 인덱스 관리 파일
>   - FRM : 테이블 구조에 관한 파일
> - SELECT 검색시 테이블을 검색하는 것이 아니라 MYI 파일 내용 검색

<br>

## 인덱스 특징

### 장점

- 테이블에서 검색과 정렬 속도를 향상시킴
- 기본키(primary key)는 자동으로 인덱싱됨

### 단점

- 인덱스 관리를 위한 추가 잡업 필요
- 추가적인 저장 공간 필요
- 잘못된 사용은 오히려 검색 성능을 저하시킴
  - INSERT : 테이블에는 입력 순서대로 저장되지만, 인덱스 테이블에는 정렬하여 저장
    하기 때문에 성능 저하 발생
  - DELETE : 테이블에서만 삭제되고 인덱스 테이블에는 남아있어 쿼리 수행 속도 저하
  - UPDATE : DELETE, INSERT 두 작업 수행하여 부하 발생

### 인덱스를 사용하면 좋은 경우

- 규모가 큰 테이블
- 삽입(Insert), 수정(Update), 삭제(Delete) 작업이 자주 발생하지 않는 칼럼
- where나 order by, join 등이 자주 사용되는 칼럼
- 데이터의 중복도가 낮은 칼럼

<br>

## 인덱스 자료구조

### B+ Tree 알고리즘

![Untitled](https://user-images.githubusercontent.com/63090006/166668175-4174795a-c4f0-45e3-bebc-7f252a0f888c.png)

- 가장 일반적으로 사용되는 인덱스 알고리즘
- 범위 탐색이 가능
- like와 같은 구문을 실행할 때도 사용할 수 있다

### 해시 알고리즘

- 컬럼의 값으로 해시 값을 계산해서 인덱싱하는 알고리즘
- 매우 빠른 검색을 지원
- 주로 메모리 기반 데이터베이스에서 많이 사용
- 동등 연산(=)에 특화된 자료구조 (range search의 한계로 대부분 B+ Tree 알고리즘 사용)

<br>

## 인덱스의 종류

### 키에 따른 인덱스 분류

- 기본 인덱스 (Primary Index) : 기본키를 포함하는 인덱스
- 보조 인덱스 (Secondary Index) : 기본 인덱스 이외의 인덱스

### 파일 조직에 따른 인덱스

- Clustered Index 

  ![Untitled](https://user-images.githubusercontent.com/63090006/166668260-dcd4bcb3-4f24-4fd8-a68b-8d7f92f65ee9.png)
  - 물리적으로 정렬되어 있음
  - 테이블 당 하나씩 존재
  - 데이터 입력, 수정, 삭제 시에도 정렬 수행, 느림
  - 검색 속도 빠름
- Non-Clustered Index 

  ![Untitled](https://user-images.githubusercontent.com/63090006/166668312-311c5670-8767-4529-8f44-cad7e72dbca7.png) 
  - 순서와 상관 없음
  - 테이블 당 여러개 존재 가능
  - 클러스터형 인덱스보다 검색 속도는 느리지만, 데이터 입력, 수정, 삭제 더 빠름

<br>

### 인덱스 생성

```sql
CREATE INDEX 인덱스이름
ON 테이블이름 (필드이름1, 필드이름2, ...)
```

### 인덱스 정보 보기

```sql
SHOW INDEX
FROM 테이블이름
```

![Untitled](https://user-images.githubusercontent.com/63090006/166668417-5017489a-995d-4bec-a399-6766a9a7de4b.png)
