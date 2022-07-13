# [Network] Multiplexing and Demultiplexing

![](https://velog.velcdn.com/images/yhd520/post/a53e72c0-1762-4e00-bb4b-ac0abe151d59/image.png)


## 1. Multiplexing 이란?
Apllication layer에서 패킷(Message)들이 여러 socket을 통해 Transport layer로 전달 될 때 각 패킷(Message)에 헤더를 추가하고 세그먼트로 캡슐화하여 Network layer로 전달하는 과정을 Multiplexing이라고 한다.

![](https://velog.velcdn.com/images/yhd520/post/3987e48b-c0db-4044-843d-200eeb3ecb9c/image.png)

## 2. Demultiplexing 이란?
Transport layer에 의해서 세그먼트가 Application layer의 올바른 소켓으로 전달되는 과정을 Demultiplexing이라고 한다.

![](https://velog.velcdn.com/images/yhd520/post/32d88efa-8586-4f3f-8bca-9ddc10d14ea0/image.png)

### 2.1. Demultiplexing의 방식
세그먼트의 헤더에 있는 `dest port`정보를 가지고 demultiplexing을 한다.

- source post: 자기자신 포트넘버
- dest port: 목적지 포트넘버

![](https://velog.velcdn.com/images/yhd520/post/c51a4b94-749e-4a0d-a9cd-ced833f9d49a/image.png)

### 2.2. UDP에서의 Demultiplexing
![](https://velog.velcdn.com/images/yhd520/post/85586e10-c2c8-4bd5-918a-c7c202739ad1/image.png)
**UDP에서의 Demultiplexing은 `dest ip`, `dest port`만을 가지고 이루어진다.**
즉, `source ip` 또는 `source port` 가 달라도(출처가 달라도) `dest ip`, `dest port`만 같으면 같은 소켓으로 전달 된다.



### 2.3. TCP에서의 Demultiplexing

![](https://velog.velcdn.com/images/yhd520/post/15237299-c7f0-4c68-b245-b41cb124ded6/image.png)

TCP에서 Demultiplexing은 4-tuple로 이뤄진다.
- 소스 IP 주소
- 소스 포트 번호
- 대상 IP 주소
- 대상 포트 번호

위 4개의 값을 모두 사용하여 적절한 소켓으로 패킷을 전달한다. **하나만 달라도 다른 소켓으로 전달된다.**
> Web server는 동시에 많은 TCP 소켓을 지원하고, 각 연결된 클라이언트마다 서로 다른 소켓을 갖는다. 즉, 소켓이 1:1로 대응된다.