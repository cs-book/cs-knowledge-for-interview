# JWT(feat. Session)

## Content

- [Session의 문제점](#session의-문제점)
- [Json Web Token](#Json-Web-Token)
- [JWT 동작 원리](#JWT-동작-원리)
- [JWT의 장점과 한계](#jwt의-장점과-한계)

## Session의 문제점

클라이언트의 인증 방식으로 Session을 사용하는 경우 여러 문제점이 발생할 수 있다.

### 메모리 용량 부족

Session은 일반적으로 server의 메모리에 저장되는데 동시 접속자가 많을 경우 메모리가 부족해질 수 있다.

### 중복된 Session 정보

서버가 여러 대로 증설되는 경우 클라이언트의 요청을 처리하는 서버가 매번 달라질 수 있다.

- 첫 번째 요청을 서버 A가 수행하면 서버 A의 메모리에 Session 정보가 저장된다.
- 두 번째 요청을 하였을 때 서버 A의 부하가 높아 서버 B가 요청을 처리할 수 있는데, 이때 서버 B에는 클라이언트의 Session 정보가 없으므로 새롭게 Session을 생성한다. 즉, 중복된 Session 정보가 생기는 것이다.
- 새롭게 Session을 생성하는 오버헤드를 줄이려면 Session을 생성하고 다른 모든 서버에 새롭게 생성된 Session 정보를 복사할 수 있다. 하지만, 이 과정도 충분히 오버헤드가 크다.

### 속도 저하

- Session 정보를 저장할 물리 디스크 기반 데이터베이스를 사용함으로써 여러 서버에 중복 생성된 Session 정보를 한 곳에서 관리할 수 있다. 하지만 속도 저하가 발생한다.
- 일반적으로 Session 정보는 메모리에 저장되는데 물리 디스크에 저장하는 경우 속도 저하가 심각하게 발생한다.

위와 같은 문제를 해결하기 위해 메모리 기반 데이터베이스(eg. redis)를 사용할 수 있다. 여러 서버가 공유할 수 있는 메모리에 Session 정보를 저장할 수 있다. 하지만 첫 번째 문제가 여전히 발생한다. 이와 같은 문제를 JWT로 해결할 수 있다.

## Json Web Token

- JWT는 데이터를 JSON 객체로 안전하게 주고 받기 위한 표준 기술이다([RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519)).
- JWT에는 서명 알고리즘으로 서명된 signature가 포함되는데 이 정보를 검증한 후에 클라이언트의 요청을 수행하거나 거부할 수 있다.
- JWT의 서명 알고리즘으로는 해시 알고리즘(eg. HMAC)이나 공개키 알고리즘(eg. RSA)이 사용된다.

JWT는 기밀성보다 무결성에 중점을 둔다. 즉, 전송되는 데이터가 변조되거나 위조되지 않았다는 것을 보장할 수 있다. 이러한 점을 고려하여 JWT를 다음과 같은 상황에서 사용할 수 있다.

#### 인가

클라이언트가 JWT를 서버에 전송한 경우 signature을 검증하고 클라이언트의 권한을 확인한다. 권한이 있는 경우 요청한 내용을 수행한다. 권한이 없는 경우에는 요청을 거부한다.

#### 암호화

JWT는 RSA와 같은 알고리즘으로 데이터를 안전하게 주고 받을 수 있다.

![image](https://user-images.githubusercontent.com/68716284/165310041-80b14cae-60cb-417e-a05d-a829ba19be70.png)

### 구조

JWT는 header, payload, signature로 구성되어 있다.

|   구분    | 용도                                                                                                       |
| :-------: | :--------------------------------------------------------------------------------------------------------- |
|  header   | 일반적으로 서명 알고리즘의 종류와 토큰 유형(JWT)으로 포함된다.                                             |
|  payload  | claim으로 구성된다. claim은 만료 시간, 발급자, 개인 claim(username, email 등) 등으로 구성할 수 있다.       |
| signature | `base64Encode(header) + base64Encode(payload) + server private key`를 서명 알고리즘으로 서명한 데이터이다. |

JWT는 header, payload, signature를 각각 base64 알고리즘로 인코딩하고 합쳐서 사용한다. 이때 header와 payload에 중요 데이터를 담아서는 안된다. base64 알고리즘로은 누구나 복호화할 수 있기 때문이다.

## JWT 동작 원리

### 서명

![image](https://user-images.githubusercontent.com/68716284/165311715-5268b3b7-a4ce-4b79-9bea-c703059f7c02.png)

![image](https://user-images.githubusercontent.com/68716284/165313787-09d172ab-b7cd-4863-92c0-f21d32bfe44f.png)

JWT의 signature는 header와 payload를 각각 base64 알고리즘으로 인코딩하고 서명 알고리즘으로 서명(암호화)한다. 이때 Server가 보관 중인 private key가 포함되기 때문에 외부에서 위조하기가 매우 어렵게 된다.

### 검증

server는 전달받은 JWT의 signature를 아래와 같이 검증한다.

- 전달받은 header와 payload를 server가 보관 중인 private key와 합치고 서명한다.
- 새롭게 서명한 signature와 전달받은 signature가 동일한지 확인하고 요청을 수행하거나 거부한다.

![image](https://user-images.githubusercontent.com/68716284/165313361-20fd2692-fef3-48ec-9d6d-584ee2cb66a6.png)

## JWT의 장점과 한계

### 장점

- 기존 Session 방식의 단점을 완화할 수 있다.
- 메모리나 물리 디스크에 검증을 위한 데이터를 저장할 필요가 없다.

### 한계

- JWT는 HTTP로 데이터를 전송하기 때문에 header와 payload의 크기가 클수록 데이터 전송 비용(대역폭, 전송 속도 등)이 증가한다.
- JWT 토큰은 만료시간이 지나야만 더 이상 사용할 수 없게 된다. 따라서 JWT 토큰이 유출되면 만료될 때까지 제삼자가 사용할 수 있다. 이때 피해를 최소화하기 위해 JWT 토큰의 만료시간을 짧게 지정할 수 있다.
- PC, Mobile 등의 접속 환경 중 한 곳에서만 로그인하는 것이 어렵다. 왜냐하면 서버는 토큰을 저장하지 않기 때문이다. 즉, 서버는 클라이언트의 로그인 상태를 알 수 없다.
