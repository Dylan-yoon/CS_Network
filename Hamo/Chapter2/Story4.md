# Story4. 서버에서 연결을 끊어 소켓을 말소한다.

## 데이터 보내기를 완료했을 때 연결을 끊는다.

데이터 송, 수신 동작이 끝난 후 프로토콜 스택의 움직임을 설명한다.

- 데이터 보내기를 완료한 쪽에서 연결 끊기 단계에 들어간다.
- 서버 측에서 먼저 끊는다면 서버측의 어플리케이션이 Socket 라이브러리의 close를 호출한다.
- 서버측의 프로토콜 스택이 TCP 헤더를 만들고 연결 끊기를 나타내는 정보를 설정한다.
    - 컨트롤 비트의 FIN 비트 1로 설정
- IP 담당 부분에 클라이언트로 송신을 의뢰하고 소켓에 연결 끊기 동작에 들어갔다는 정보를 기록한다.
- 클라이언트에 서버에서 보낸 TCP 헤더가 도착하면 클라이언트측의 프로토콜 스택은 자신의 소켓에 연결 끊기 동작에 들어갔다는 정보를 기록한다.
- ACK 번호를 서버측에 반송한다.(잘 받았다는 사실을 알리기 위함)
- 서버에서 보낸 데이터를 전부 수신하면 클라이언트 측에서도 close를 호출하고 서버에 FIN 비트가 1인 TCP 헤더 만들어서 보내고 서버에서 ACK 번호 돌아오면 서버와의 대화가 끝난다.

## 소켓을 말소한다.

서버와의 대화가 끝나면 소켓을 쓸모없지만 바로 말소하지 않고 잠깐 기다린다.

- 오동작을 막기 위함이다.
- 이유는 너무 다양하다.
    - 소켓이 말소되서 해당 포트번호를 다른 소켓이 가지면 오동작할 수도 있다.)
- 기다리는 시간은 보통 몇 분 정도 기다리다가 말소한다.
    - 패킷이 없어졌을 때 몇 분 정도 다시 보내다가 회복 전망이 없으면 멈추기 때문이다.

## 데이터 송, 수신 동작 정리

1. 소켓을 작성하는 단계
    1. 보통 먼저 서버측에서 어플리케이션이 동작하기 시작했을 때 소켓을 만들고 접속 대기 상태로 만든다.
2. 소켓을 만들면 클라이언트에서 서버를 향해 접속 동작을 실행한다.
    1. 클라이언트가 SYN을 1로 만든 TCP 헤더를 만들어 서버로 보낸다.
    2. 해당 TCP 헤더에는 시퀀스 번호의 초기값과 윈도우의 값이 기록돼있다.
    3. 서버에 도착하면 서버에서 SYN을 1로 만든 TCP 헤더가 돌아온다.
    4. 이 헤더에도 시퀀스 번호와 윈도우가 기록되어 있고 ACK 번호도 기록되어 있다. (컨트롤 비트의 ACK를 1로 만듬)
    5. 클라이언트는 잘 받았다고 TCP 헤더에 ACK 번호를 기록해서 서버에 돌려보낸다.
    6. 접속동작 끝 (서버랑 클라이언트랑 이것 저것 정보 실어서 주고 받음)
3. 데이터 송, 수신 단계
    1. 어플리케이션의 종류에 따라 다르다.
    2. 웹의 경우에는 클라이언트에서 서버에 리퀘스트 메시지를 보내는 것부터 시작한다.
    3. TCP가 적당한 크기의 조각으로 분할하고 TCP 헤더를 맨앞에 부가해서 서버에 보낸다.
    4. 서버가 응답 메시지를 반송한다.
    5. 끝 (윈도우 값을 클라이언트에 전달하는 내부 동작은 앞에서 찾아보도록)
4. 연결 끊기 동작
    1. 웹의 경우 서버에서 연결 끊기 동작에 들어간다.
    2. 먼저 서버에서 FIN을 1로 만든 TCP 헤더가 클라이언트로 흐른다.
    3. 이걸 받았다는 것을 나타내는 ACK 번호의 TCP 헤더가 돌아온다.
    4. 이제 클라이언트에서 FIN을 1로 만든 TCP헤더가 서버로 흐른다.
    5. 서버도 잘 받았다는 것을 나타내는 ACK 번호의 TCP 헤더를 보낸다.
    6. 조금 있다가 소켓이 말소된다.
