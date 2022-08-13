### Contents

- [요청 메시지](#요청-메시지)
- [응답 메시지](#응답-메시지)
- [메시지 인코딩](#메시지-인코딩)
- [Multipart](#Multipart)
- [Range Request](#Range-Request)

# HTTP 메시지

HTTP 메시지는 클라이언트 <-> 서버 간 데이터를 교환하는 방식입니다. HTTP 메시지는 `요청` 메시지와 `응답` 메시지로 나뉩니다. 각 메시지의 역할은 다음과 같습니다.

|    구분     | 역할                                                   |
| :---------: | ------------------------------------------------------ |
| 요청 메시지 | 클라이언트가 서버에게 특정 동작을 취하도록 요청하는 것 |
| 응답 메시지 | 요청에 대한 서버의 답변                                |

HTTP 메시지는 개행 문자를 기준으로 헤더와 바디로 나뉘는데, 바디는 생략될 수 있습니다.

```
메시지 헤더
──────────────────────
개행 문자 (CR + LF)
──────────────────────
메시지 바디 (생략 가능)
```

헤더, 개행 문자, 바디의 역할은 다음과 같습니다.

|   구분    | 역할                                                      |
| :-------: | --------------------------------------------------------- |
|   헤더    | 요청 또는 응답에 대한 정보와 바디에 대한 정보를 가집니다. |
| 개행 문자 | 더 이상 헤더 정보가 없음을 의미합니다.                    |
|   바디    | 요청 또는 응답에서 필요한 페이로드를 가집니다.            |

## 요청 메시지

### 요청 메시지 헤더

요청 메시지 헤더의 구조는 다음과 같습니다. 요청 라인에는 `요청 메소드`, `요청 URI`, `HTTP 버전`이 포함됩니다.

```
요청 라인
───────────────
요청 헤더 필드
───────────────
일반 헤더 필드
───────────────
엔티티 헤더 필드
```

### 요청 메시지 예제

```
POST /users HTTP/1.1
Host: google.com
Accept-Language: ko
Cache-Control: no-cache

username=kim&password=passw0rd
```

## 응답 메시지

### 응답 메시지 헤더

응답 메시지 헤더의 구조는 다음과 같습니다. 응답 메시지 헤더는 요청 메시지 헤더와 달리 `요청 라인`과 `요청 헤더 필드` 대신 `상태 라인`과 `응답 헤더 필드`를 가집니다. 그리고 상태 라인에는 `HTTP 버전`, `상태 코드`, `상태 메시지`가 포함됩니다.

```
상태 라인
───────────────
응답 헤더 필드
───────────────
일반 헤더 필드
───────────────
엔티티 헤더 필드
```

### 응답 메시지 예제

```
HTTP/1.1 201 Created
Date: Mon, 8 Aug 2022 10:06 GMT
Server: Apache
Content-Type: application/json

{
    userId: 1
    username: "kim"
    password: "passw0rd"
}
```

## 메시지 인코딩

HTTP로 데이터를 전송할 때 인코딩을 적용하면 데이터를 효율적을 전송할 수 있습니다.

### 압축, Content-Encoding

대용량 파일과 같이 용량이 큰 데이터를 요청할 경우 데이터를 압축하여 받을 수 있습니다.

클라이언트는 `Accept-Encoding` 필드를 요청 헤더에 추가하여 압축 방법을 협상할 수 있습니다.

```
GET /files/1 HTTP/1.1
Accept-Encoding: gzip, deflate
```

서버는 `Content-Encoding` 필드를 응답 헤더에 추가하고 데이터를 압축하여 클라이언트로 전달합니다.

```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Encoding: gzip

compressed body
```

[참고](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Encoding)

### 분할, Transfer-Encoding

대용량 파일과 같이 용량이 큰 데이터를 요청하고 화면에 표시할 때 데이터를 분할하여 표시할 수 있습니다.

클라이언트는 `Accept-Encoding` 필드를 요청 헤더에 추가하여 분할 전송을 받을 수 있습니다.

```
GET /images/2 HTTP/1.1
Accept-Encoding: chunked
```

서버는 `Transfer-Encoding` 필드를 응답 헤더에 추가하고 데이터를 분할하여 전송합니다. 이때 헤더는 `Content-Length` 필드를 가지지 않습니다. 바디의 `chunk`마다 길이가 명시됩니다.

```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked

7\r\n
Mozilla\r\n
9\r\n
Developer\r\n
7\r\n
Network\r\n
0\r\n
\r\n
```

## Multipart

메일의 경우 본문에 텍스트를 작성하고 파일을 첨부하여 전송할 수 있습니다. HTTP로 여러 종류의 데이터를 한 번에 보내야 하는 경우에 `멀티파트` 타입을 사용하여 전송할 수 있습니다.

[참고](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

### multipart/form-data

`multipart/form-data`는 클라이언트에서 서버로 여러 종류의 데이터를 전송할 수 있습니다.

`Content-Type` 필드에 `multipart/form-data`를 설정하고 바디에 각 데이터를 `-` 문자로 시작하는 `boundary`로 구분하여 전송합니다.

```
POST / HTTP/1.1
Content-Type: multipart/form-data; boundary=--xyz
Content-Length: 465

--xyz
Content-Disposition: form-data; name="name"
Content-Type: text/plain

John
--xyz
Content-Disposition: form-data; name="age"
Content-Type: text/plain

23
--xyz
Content-Disposition: form-data; name="photo"; filename="photo.jpeg"
Content-Type: image/jpeg

[JPEG DATA]
--xyz--
```

### multipart/byteranges

`multipart/byteranges`는 클라이언트에서 복수 범위의 데이터를 요청할 때 사용합니다.

각 데이터는 `boundary`에 의해 구분되어 바디에 포함되며, 각각 `Content-Type`, `Content-Range` 필드를 가집니다.

```
HTTP/1.1 206 Partial Content
Accept-Ranges: bytes
Content-Type: multipart/byteranges; boundary=3d6b6a416f9b5
Content-Length: 385

--3d6b6a416f9b5
Content-Type: text/html
Content-Range: bytes 100-200/1270

eta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="vieport" content
--3d6b6a416f9b5
Content-Type: text/html
Content-Range: bytes 300-400/1270

-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: "Open Sans", "Helvetica
--3d6b6a416f9b5--
```

## Range Request

데이터를 요청할 때 일부 데이터만 요청할 수 있습니다.

[참고](https://developer.mozilla.org/ko/docs/Web/HTTP/Range_requests)

```
GET /images/3 HTTP/1.1
Host: github.com
Range: bytes=0-1023
```
