# Story2. 서버에 접속한다.
소켓을 만들면 어플리케이션은 connect를 호출한다.

## 접속의 의미

- 소켓을 만든 직후는 그곳에 아무것도 기록되어 있지 않으므로 통신 상대가 누구인지 모릅니다.
    - 어플리케이션쪽에서는 IP 주소나 포트 번호를 알고 있지만 프로토콜 스택에는 전달되지 않는다.
    - 따라서 서버의 IP 포트 번호를 프로토콜 스택에 알리는 동작이 필요하다. → 접속 동작의 한 가지 역할
- 마찬가지로 서버도 소켓을 만드는 동작을 한 직후에는 아무것도 모른다.
    - 클라이언트에서 통신하려는 클라이언트가 있다는 정보를 전달해줘야 한다. → 이것도 접속 동작의 한 가지 역할
- **접속 동작의 첫 번째 동작은 통신 상대와의 사이에 제어 정보를 주고받아 소켓이 필요한 정보를 기록하고 데이터 송, 수신이 가능한 상태로 만드는 것이다.**
    - 위에 내용이 구체적인 예시
- 데이터 송, 수신 동작을 실행할 때 데이터를 일시적으로 저장하는 메모리 영역이 필요하다. → 버퍼 메모리
    - 버퍼 메모리의 확보도 접속 동작을 실행할 때 실행되는데 이것이 접속 한다는 동작의 의미이다.

## 맨 앞부분에 제어 정보를 기록한 헤더를 배치한다.

제어 정보는 크게 두 종류가 있다.

1. 클라이언트와 서버가 서로 연락을 절충하기 위해 주고받는 제어정보 (헤더에 기입되는 정보)
    1. TCP 프로토콜의 사양으로 규정하고 있다.
    2. 접속, 송, 수신, 연결 끊기의 각 단계에서 클라이언트와 서버가 대화할 때마다 거기에 이 제어정보를 부가합니다.
    3. 패킷의 맨 앞 부분에 부가한다.
    4. 접속 동작에서는 데이터가 없고 제어 정보만 이루어져 있다.
    5. 헤더라고 부른다. (여러 가지 헤더가 있어서 앞에 무엇의 헤더인지 붙여준다. 여기서는 TCP 헤더)
    6. 클라이언트와 서버는 이 헤더에 필요한 정보를 기록하여 연락을 취하면서 통신을 진행한다.
2. 소켓에 기록하여 프로토콜 스택의 동작을 제어하기 위한 정보 (소켓에 기록되는 정보)
    1. 어플리케이션에서 통지된 정보, 통신 상대로부터 받은 정보 등이 수시로 기록된다.
    2. 프로토콜 스택안에 있는 소켓의 메모리 영역에 기록된다.
    3. 이 제어 정보는 상대측에서 볼 수 없다.
    4. OS 따라서 프로토콜 스택을 만드는 방법이 다르기 때문이다.

## 접속 동작의 실제

어플리케이션이 connect를 호출하는 곳부터 시작

- 서버측의 IP 포트 번호 프로토콜 스택의 TCP로 전달
- TCP 담당이 상대 서버의 TCP 담당과 제어 정보를 주고 받는다.
    - 송, 수신 동작의 개시를 나타내는 제어 정보를 기록한 헤더를 만든다.
    - 여기서 중요한 헤더의 항목은 송신처와 수신처의 포트번호이다. → 클라이언트측 소켓과 서버측 소켓을 지정할 수 있다.
    - 접속해야 하는 소켓이 어느 것인지 확실히 하고 컨트롤 비트인 SYN이라는 비트를 1로 만든다.
- 위에서 만든 TCP 헤더를 IP 담당에게 건네주어서 송신하도록 의뢰한다.
- IP 담당이 패킷 송신 동작을 실행한다.
- 서버측의 IP 담당이 받아서 TCP 담당에게 전달
- TCP 헤더를 조사해서 수신처 포트 번호에 해당하는 소켓을 찾아낸다.
    - 접속을 기다리는 소켓 중에서 일치하는 소켓

위 과정이 끝나면 서버의 응답을 돌려보낸다.

- 클라이언트랑 똑같이 TCP 헤더를 만들어서 전달한다.
    - ACK라는 컨트롤 비트도 1로 만든다.
    - 패킷을 받았다는 것을 알리기 위한 동작이다.
- IP담당 한테 전달해서 반송한다.
- 클라이언트가 받아서 똑같이 헤더 조사
    - SYN이 1이면 접속 성공하고 제어 정보를 기록
- 마지막으로 ACK 비트를 1로 만든 TCP 서버측에 다시 반송
    - 잘 도착한 것을 알리기 위함
    - TCP 3 Way-Handshake

이제 소켓은 데이터를 송, 수신할 수 있는 상태가 된다.

파이프로 연결된 것으로 생각할 수 있다.(실제로 연결된 것은 아니다.) → 네트워크 업계의 습관, 커넥션이라고 한다.

커넥션은 close를 호출해서 연결을 끊을 때까지 계쏙 존재한다.
