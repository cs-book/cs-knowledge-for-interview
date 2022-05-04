# [Web] Restful API

![rest](https://user-images.githubusercontent.com/67628725/166668483-f1c65718-d438-4c8d-889e-0260678c95bf.png)

### REpresentational State Transfer

: `자원(resource)`을 ‘이름’으로 구분해, 해당 자원의 `상태(state)` 정보를 주고 받는 

  서버-클라이언트 통신 방식 중 하나 

<aside>
👉 URI에 **자원(resource)**를 명시하고,

**HTTP Method(POST, GET, PUT, DELETE)**로 행위를 지정함

</aside>

- **HTTP 프로토콜을 그대로 사용**
- 웹의 장점을 최대한 활용할 수 있는 아키텍처
- 다양한 클라이언트(브라우저, 기기) 에서 통신하기 위해 필요
- 표준이 존재하지 않음

<aside>
📗 **CRUD**

1. Create : 생성(Post)
2. Read : 조회(GET)
3. Update : 수정(PUT)
4. Delete : 삭제 (DELETE)
</aside>

## REST 구성요소

- 자원(Resource): **URI**
    - 모든 자원에는 고유한 ID가 존재 (server)
- 행위(Verb): Method
    - GET / POST / PUT / DELETE
- 표현(Representation)
    - Json / xml / txt / rss
    

## REST 특징

1. Server-Client [서버-클라이언트 구조]
    1. Server : 자원을 소유하는 쪽 
    2. Client : 자원을 요청하는 쪽
    3. 서로의 의존성 ↓
2. Stateless [무상태성]
    1. HTTP와 마찬가지로 stateless 프로토콜
    2. 클라이언트의 context(ex_세션, 쿠키)정보는 관여 X
    3. 요청에 대한 응답만을 처리
3. Cacheable [캐시처리 가능]
    1. 캐시 : 대량의 요청 효율적으로 처리 가능 
    2. REST 서버의 트랜잭션이 발생하지 않기 때문 
4. Layered System [계층화]
    1. REST 서버는 다중계층 가능
    2. 보안, 로드밸런싱, 암호화, 인증 등의 기능을 구분 가능 
    3. Proxy, Gateway 등 중간매체 사용가능 
5. Code-On-Demand
    1. 서버로 부터 스크립트를 받아 클라이언트에서 실행 
6. Uniform Interface [인터페이스 일관성]
    1. HTTP 표준 프로토콜
    2. 특정 기술에 종속되지 않음 

## REST API 설계규칙

### API

: Application Programming Interface

1. URI는 정보의 `자원`(resource) 을 표현 !! 
    - 명사(O) ~~동사~~(X)
    - 소문자(O) ~~대문자~~(X)
    
    | Document | DB의 레코드 | 단수명사 |
    | --- | --- | --- |
    | Collection | 서버의 디렉토리 리소스 | 복수명사 |
    | Store | Client의 리소스 저장소 | 복수명사 |
2. 행위는 HTTP Method로 표현 
    - GET / PUT / POST  /DELETE
    - URI 에 HTTP Method 가 들어가면 안됨 !!!
    - URI 에 동사표현 금지 !!
    - URI에 표출되는 value는, ‘자원’ 의 고유식별자(id) 여야 함.
3. 슬래시(/) 는 계층관계를 나타냄
4. URI 마지막에 슬래시(/) 두지 않기 
5. URI에 밑줄(_) 사용금지 
6. URI에 대문자 지양 
7. URI에 파일확장자 포함금지

<aside>
❗ URI 에 들어가면 안되는 것!

⇒ 동사표현, 밑줄(_), 대문자, 파일확장자

</aside>

### RESTful

: REST API 를 제공하는 웹서비스 

### PUT vs PATCH

|  | PUT | PATCH |
| --- | --- | --- |
| 자원교체 | 전체 | 부분 |
| 필요한 필드값 | 모든 필드 | 일부 필드만 |
| 일부 필드값만 제공할                    시 나머지 필드값 | null | 그대로 |
| 일부 update | X | O |
