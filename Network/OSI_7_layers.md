# OSI 7계층

## OSI 7계층이란?

![Untitled](https://user-images.githubusercontent.com/52027965/165341040-e8d0f6d3-9abe-4f1d-b20e-ae7a3fef7491.png)

- 네트워크에서 통신이 일어난 과정을 7단계로 나눈것 <br><br>

## 등장 배경

- 기업들이 각자 모델에 맞춰 장치를 개발해 다른 회사에서 만든 장치끼리 통신이 안 됨
- 이 기종 컴퓨터 간 통신 위해 네트워크 표준화가 필요해짐. 이를 위해 국제 표준화 기구(ISO)에서 통신 장비를 설계할 때 기준으로 삼으라고 제시해 준 표준 모델이 OSI 참조 모델
- 단계별로 파악할 수 있고 흐름을 한눈에 볼 수 있으며 특정 단계에서 문제발생시 다른 부분은 건들지 않고 그 부분만 고치면 해결 할 수 있기 때문
- OSI 7계층 = 컴퓨터 통신 구조의 표준적 모델 <br><br>

## 계층별 특징<br><br>

- 상위 계층은 하위 계층의 부족한 점을 보완함<br><br>

1. 물리계층
    
    ![Untitled](https://user-images.githubusercontent.com/52027965/165341016-0fc4af64-19d8-43a9-998b-809124f76ee4.png)
    
    > 물리적으로 신호를 주고받음
    
    - 사용되는 통신 단위는 비트이며 이것은 1과 0으로 나타내어진다.
    - 단지 데이터 전송만 하고 어떤 에러가 있는지, 데이터가 무엇인지는 신경 쓰지 않는다.
    - 장비로는 리피터, 허브가 있다.<br><br>
    
2. 데이터 링크 계층
    
    ![Untitled](https://user-images.githubusercontent.com/52027965/165341022-ad8fd9da-77ce-43dd-8455-1041ed150257.png)
    
    > MAC주소를 부여 하고 에러검출,재전송,흐름제어를 한다.
    
    - 물리계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 도와주는 역할을 한다. 통신에서의 오류도 찾아주고 재전송한다.
    - 같은 네트워크에 있는 여러 대의 컴퓨터들이 데이터를 주고 받기 위해서 필요
    - MAC 주소를 가지고 통신하게 된다.
    - 장비로는 브리지, 스위치가 있다.
    - 프로토콜로는 이더넷이 있다.<br><br>
    
3. 네트워크 계층(Network Layer)
    
    ![Untitled](https://user-images.githubusercontent.com/52027965/165341025-b05cffc6-e258-4008-93b1-9df64531983a.png)
    
    > 데이터를 목적지까지 가장 안전하고 빠르게 전달하는라우팅 기능을 한다
    
    - 경로를 선택하고 주소를 정하고 경로에 따라 패킷을 전달해준다
    - 장비로는 대표적으로 "라우터(router)" 이다.
    - 프로토콜로는 IP, ARP, RARP, ICMP, IGMP<br><br>

4. 전송 계층(Transport Layer)
    
    ![Untitled](https://user-images.githubusercontent.com/52027965/165341030-d99a21a0-db82-48be-81f9-f674af6f111d.png)
    
    > 양 끝단의 사용자들이 데이터를 주고 받을 수 있게 한다
    
    - 네트워크 계층에서 네트워크 주소(컴퓨터-컴퓨터)까지 전송한다면 전송계층은 포트 주소(프로세스-프로세스)까지 전송 담당
    - 1,2,3계층으로 인해 전세계로 데이터를 송수신할 수 있게 됨. 하지만 데이터를 전달만할뿐 수신 호스트가 패킷을 수신할 준비가 되었는지, 전송과정에서 패킷이 손상되거나 유실되지는 않았는지 등의 문제들은 신경쓰지 않음
    - 패킷이 전송 과정에서 아무 문제 없이 제대로 수신지 컴퓨터에 도착할 수 있도록 패킷 전송을 제어하는 역할은 전송 계층이 담당 (시퀀스 넘버 기반 패킷 흐름제어, 오류 제어 등)
    - 프로토콜은 TCP, UDP<br><br>


5. 세션 계층(Session Layer)
    
    > 데이터가 통신하기 위한 논리적인 연결 담당
    
    - 1~4계층까지 주된 내용이 데이터 전달 이라면, 5계층부터는 컴퓨터 내의 프로세스들에 의해 데이터를 만드는 작업
    - 1~4계층의 정상적인 역할 수행으로 네트워크 세션이 생성
    - 네트워크상 양쪽 연결을 관리하고 연결을 지속 시켜 주는 계층
    - 응용프로그램간의 대화를 유지하기 위한 구조 제공
    - 동기화 담당
    - 특정 지점에서 송수신 오류가 발생하면 이전에 동기점으로 설정한 통신이 완벽했던 지점부터 다시 재전송.
    - 암호화, 로그인 기능, 호스트 인증 기능
        - **세션이란?**
            
            세션이란 일정 시간 동안 같은 사용자로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 일정하게 유지하는 기술을 말한다. 여기서 일정 시간이란, 방문자가 웹 브라우저를 통해 웹 서버에 접속한 시점으로부터 웹 브라우저는 종료함으로써 연결을 끝내기 까지의 기간을 말한다. 즉, 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 세션이라고 부른다.
            
    - 프로토콜로는 SSH, TLS<br><br>

6. 표현 계층(Presentation Layer)
    
    > 데이터변환, 압축, 암호화 지원

    - 응용 계층의 데이터를 세션계층이 다룰 수 있는 형태로 바꾸고 세션계층의 데이터를 응용계층이 이해할 수 있는 형태로 바꾸고 전달하는 일 담당
    - 응용 계층에서 표현하는 다양한 표현양식을 공통적인 전송형식으로 변환
    - 한 시스템의 애플리케이션에서 보낸 정보를 다른 시스템의 애플리케이션 층이 읽을 수 있도록 하는 층
    - 데이터를 하나의 표현 형태로 변환하여 두 장치가 일관되게 전송데이터를 이해할 수 있도록 한다
    - 코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어 준다.
    - 데이터 번역자 로서의 역할
    - 프로토콜로는 JPEG, MPEG(오디오, 비디오)<br><br>

7. 응용 계층(Application Layer)
    
    > 사용자가 네트워크에 접속 가능 하게 해주는 계층
    
    - 사용자가 보는 소프트웨어의 UI, 사용자의 입출력 부분(I/O) 등을 담당하는 계층이다.
    - 사용자에게 서비스를 제공하고 사용자가 제공한 정보나 명령을 하위계층으로 전달하는 역할
    - 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다.
    - 포로토콜로는 HTTP, FTP, DNS, SMTP, Telnet<br><br>
    

## TCP/IP 4계층과 OSI 7계층?

![Untitled](https://user-images.githubusercontent.com/52027965/165341032-9bdb2ed6-026c-46f2-a494-602f774be15b.png)
<br><br>

## 한 단어 요약

- 상위 계층은 하위 계층의 부족한 점을 보완한다
- 1-전기, 2-맥, 3-IP, 4-포트, 5-세션, 6-표현, 7-사용자
<br><br>

## reference

[https://dncjf64.tistory.com/379](https://dncjf64.tistory.com/379)
