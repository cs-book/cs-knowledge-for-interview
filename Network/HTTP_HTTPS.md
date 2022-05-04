# HTTP vs HTTPS

# HTTP

HyperText Transfer Protocol 의 약자로 **인터넷에서 데이터를 주고받기 위한 규칙**입니다.

클라이언트가 브라우저나 모바일 앱 등을 통해서 정보를 **요청(request)**하면 서버가 **응답(response)**하는 형태로 동작합니다.

요청 메시지에는 `Method`, `Path`, `Protocol 버전`, `Headers`, `Request Body` 등이 있습니다.

응답 메시지에는 `Protocol 버전`, `Status Code`, `Headers`, `Response Body` 등이 있습니다.

## HTTP Method

- GET: 리소스 조회
- POST: 리소스 추가
- PUT: 리소스 전체 업데이트
- PATCH: 리소스 부분 업데이트
- DELETE: 리소스 삭제
- 그 외
    - HEAD: 리소스에 대한 헤더만 조회
    - OPTIONS: 리소스에 대해 수행 가능한 동작 조회

## HTTP Status Code

- 1XX: 조건부 응답
    - 101: **Swiching Protocols**로 서버가 프로토콜을 변경했음을 의미합니다.
- 2XX: 성공
    - 200: **OK**로 요청이 정상이며 응답 본문에 요청한 리소스를 포함하고 있음을 의미합니다.
- 3XX: 리다이렉션
- 4XX: 요청 오류
    - 401: **Unauthorized**로 인증되지 않아 요청이 서버에 의해 거부되었음을 의미합니다.
    - 403: **Forbidden**으로 권한이 없어 요청이 서버에 의해 거부되었음을 의미합니다.
    - 404: **Not Found**로 클라이언트가 요청한 URL을 서버가 찾을 수 없음을 의미합니다.
- 5XX: 서버 오류
    - 500: **Internal Server Error**

# HTTPS

HTTP에 SSL(Secure Socket Layer) 또는 TLS(Transport Layer Security) 라는 다른 프로토콜을 조합해 **암호화**와 **인증**을 더한 프로토콜입니다.

HTTP는 암호화되지 않은 평문 통신이기 때문에 도청이 가능하고, 통신 상대를 확인하지 않기 때문에 위장이 가능합니다. HTTPS는 이런 HTTP의 단점을 보완할 수 있습니다.

## 주류 주문 과정

![Untitled](https://user-images.githubusercontent.com/38188374/166248115-edbf0885-6b02-487d-9b0f-afb69d0b4948.png)

1. 손님이 주민센터에 주민등록증 발급 신청, 지문 정보 제공
2. 주민센터가 손님에게 주민등록증 발급
3. 손님이 음식점에 주민등록증 제공
4. 성인 확인 과정
    1. 주민등록증 진위 확인 기기가 없다면 주민센터에 요청
    2. 기기 제공
    3. 주민등록증이 진짜임을 확인
5. 주류 제공

## HTTPS 동작 과정

![Untitled](https://user-images.githubusercontent.com/38188374/166248102-f184bed8-27eb-4403-acd7-12bc625c29a9.png)

1. 서버가 CA에게 인증서 발급 신청, 서버의 공개키 제공
2. CA의 비밀키로 암호화한 인증서 발급, CA의 공개키 제공
3. 연결 시작
    1. 클라이언트가 서버에게 연결 요청
    2. 서버가 클라이언트에게 인증서, 서버의 공개키 제공
4. 인증서 확인 과정
    1. CA의 공개키를 가지고 있지 않다면 CA에 요청
    2. CA의 공개키 제공
    3. CA의 공개키로 인증서 복호화 성공 → 인증서 검증 성공
5. 서버의 공개키로 암호화된 클라이언트의 대칭키 서버에 제공