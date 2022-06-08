# 웹 브라우저에 www.naver.com을 검색하면 일어나는 과정

![image-20220608135224901](C:\Users\guseh\AppData\Roaming\Typora\typora-user-images\image-20220608135224901.png)



### 1. URL을 IP주소로 변환

www.naver.com 이라는 도메인 이름으로는 컴퓨터끼리 통신할 수 없다. 이를 인터넷 상에서 컴퓨터가 읽을 수 있는 IP주소로 변환해야 서로 통신이 가능하게 된다.
먼저, 브라우저 캐시에 해당 URL이 존재하는지 확인하고 있다면 그것을 불러온다. 존재하지 않는다면 도메인 주소를 IP주소로 변환해주는 DNS(Domain Name System) 서버에 요청하여 해당 URL의 IP주소를 반환받는다.

> 처음 PC가 IP주소를 할당 받을 때, 2개의 DNS IP 주소를 함께 받는다. 이 DNS IP 주소를 저장해두기 때문에 DNS를 바로 찾아갈 수 있다.

##### DNS 동작 과정

![image-20220608164507228](img/image-20220608164507228.png)
출처: https://1-7171771.tistory.com/134

1 : PC에 미리 설정되어 있는 DNS(Local DNS)에게 [www.naver.com이라는](http://www.naver.xn--com-f42mh0r1y0a/) hostname에 대한 IP 주소를 물어봅니다.

2 : Local DNS에 해당 URL이 존재한다면 바로 응답을 보내고 존재하지 않는다면 root DNS 서버에 해당 URL의 IP 주소를 다시 요청한다.

3 : Root DNS 서버에 IP주소를 물어보는 DNS Query를 보내지만 Root DNS서버는 알지 못하기 때문에 com DNS 서버의 주소를 알려줍니다.

4 : 이를 수신한 Local DNS 서버는 다시 com DNS 서버에 정보를 요청하고 com DNS 서버도 자신의 하위 레벨 Domain인 naver.com의 DNS서버 주소를 알려줍니다.

5 : 이를 수신한 Local DNS 서버는 다시 naver.com DNS 서버에게 정보를 요청하고 www.naver.com에 대한 IP 서버 주소를 수신 받습니다.

6 : Local DNS 서버는 위와 같이 www.naver.com 에 대한 IP주소를 수신 후 자신의 DNS Cache에 등록하고 해당 정보를 요청했던 Client에게 응답메세지로 답변합니다.

위 과정까지 마치게 되면 URL 주소를 Local DNS에 캐싱하게되기 때문에 다음 요청 부터는 1번 과정에서 바로 IP 주소를 응답받게 된다.



### 2. 대상 서버와 TCP 소켓 연결

브라우저가 IP 주소를 받게 인터넷 프로토콜을 사용해서 서버와 연결을 한다. 인터넷 프로토콜의 종류는 여러가지가 있지만, 웹사이트의 HTTP 요청의 경우에는 일반적으로 TCP(전송 제어 프로토콜)를 사용한다.

클라이언트와 서버간 데이터 패킷들이 오가려면 TCP connection이 되어야 한다. TCP/IP 3-way handshake라는 프로세스를 통해서 클라이언트와 서버간 connection이 이뤄지게 된다. 단어 그대로 클라이언트와 서버가 SYN과 ACK메세지들을 가지고 3번의 프로세스를 거친 후에 연결이 된다. 추가적으로 HTTPS의 경우 3-way-handshake에 TLS(Transport Layer security, SSL) handShake가 추가된다.

#### 3-way Handshaking

- A클라이언트는 B서버에 접속을 요청하는 SYN 패킷을 보낸다. 이때 A클라이언트는 SYN 을 보내고 SYN/ACK 응답을 기다리는SYN_SENT 상태가 되는 것이다. 

- B서버는 SYN요청을 받고 A클라이언트에게 요청을 수락한다는 ACK 와 SYN flag 가 설정된 패킷을 발송하고 A가 다시 ACK으로 응답하기를 기다린다. 이때 B서버는 SYN_RECEIVED 상태가 된다. 

- A클라이언트는 B서버에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 되는것이다. 이때의 B서버 상태가 ESTABLISHED 이다.위와 같은 방식으로 통신하는것이 신뢰성 있는 연결을 맺어 준다는 TCP의 3 Way handshake 방식이다.

![image-20220608163106074](img/image-20220608163106074.png)
출처: https://sjlim5092.tistory.com/35?category=763426

이 과정이 끝나면 TCP connection이 완성되는 것이다.



### 3. HTTP(HTTPS) 프로토콜로 요청, 응답

이제 연결이 확정되었으니 해당 페이지 **www.naver.com**를 달라고 서버에게 요청한다. 서버에서 해당 요청을 받고, 이 요청을 수락할 수 있는지 검사하고 서버는 이 요청에 대한 응답을 생성하여 브라우저에게 전달한다.



### 4. 브라우저에서 응답을 해석

서버에서 응답한 내용은 HTML, CSS, Javascript 등으로 이루어져 있다. 이를 브라우저에서 해석하여 그려준다. 각종 텍스트를 정해진 형식으로 해석하여 우리가 원하는 **www.naver.com** 페이지가 그려진다.






>참고
>https://peemangit.tistory.com/52
>https://deveric.tistory.com/97
>https://1-7171771.tistory.com/134