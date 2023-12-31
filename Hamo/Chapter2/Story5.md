# Story5. IP와 이더넷의 패킷 송, 수신 동작

TCP 담당에게 의뢰를 받은 IP 담당이 어떻게 패킷을 상대에게 송신하는지 알아보자

## 패킷의 기본

- 패킷은 헤더와 데이터 두 부분으로 구성된다.
    - 헤더: 수신처를 나타내는 주소 등의 제어 정보가 들어있다.
- 패킷의 흐름
    - 송신처가 되는 기기가 패킷을 만든다.
        - 헤더에는 적절한 제어 정보를 기록하고 데이터 부분에는 얼마간의 데이터를 넣는다.
    - 이 패킷을 가장 가까운 중계 장치에 송신한다.
    - 중계 장치는 도착한 패킷의 헤더를 조사하여 패킷의 목적지를 판단한다.
        - 어느 수신처가 어느 방향에 있는지에 대한 정보를 기록한 표와 같은 것을 사용한다.
    - 계속 쭉쭉 보내다 보면 최종적인 수신처의  기기에 패킷이 도착한다.
    - 보통 수신처에서 송신처로 회답 패킷을 보낸다.
        - 따라서 송신처가 수신처가 될 수 있기 때문에 송, 수신처를 명확하게 구분하지 않고 엔드노드라고 부른다.
    - 엔드노드 == 엔드포인트? 같은건가
    - 이 패킷의 기본은 여러가지 패킷 통신 방식에 적합하다.
        - TCP/IP 패킷 구조는 조금 더 복잡하긴 하다.
- 서브넷은 라우터와 허브라는 두 종류의 패킷 중계 장치에서 역할을 분담하여 패킷을 운반한다.
    - 라우터가 목적지를 확인하여 다음 라우터를 나타낸다.
    - 허브가 서브넷 안에서 패킷을 운반하여 다음 라우터에 도착한다.
- 허브는 이더넷의 규칙에 따라 패킷을 운반하고 라우터는 IP의 규칙에 따라 패킷을 운반한다. 따라서 위 설명을 아래와 같이 바꿀 수 있다.
    - IP가 목적지를 확인하여 다음 IP 중계 장치를 나타낸다.
    - 서브엣 안에 있는 이더넷이 중계 장치까지 패킷을 운반한다.
- 조금 더 구체적으로 TCP/IP 패킷에는 아래와 같은 두 개의 헤더가 붙어있다.
    - MAC 헤더(이더넷용 헤더)
    - IP 헤더(IP용 헤더)
- 흐름
    - 먼저 액세스 대상 서버의 IP 주소를 IP 헤더에 기록한다.
    - IP는 이 수신처가 어느 방향인지 조사하고 그 방향에 있는 다음 라우터를 조사한다.
    - 거기에 패킷이 도착하도록 이더넷에 의뢰한다.
    - 다음 라우터에 할당된 이더넷의 주소를 MAC 헤더에 기록한다.
    - 이렇게 해서 패킷을 송신하면 허브에 도착한다.
    - 허브에 있는 패킷의 목적지를 판단하기 위한 표를 통해서 중계한다.
    - 라우터에 도착하면 라우터에 있는 IP용 표를 이용해서 어느 라우터에 패킷을 중계하면 좋을지 결정하고 이것을 MAC 헤더에 기록한다.
    - 다시 다음 라우터 전송
    - 허브 경유하고 다음 라우터 도착하고 이것 저것 하다가 목적지에 도착한다.
- 위 흐름이 TCP/IP 네트워크에서 목적지까지 전달하는 동작의 전체모습이다.

## 패킷 송, 수신 동작의 개요

프로토콜 스택의 IP 담당 부분의 패킷 송신 동작에 대해 알아보자

- IP 담당 부분은 패킷을 상대에게 송출하기만 하고 실제로 운반하는 것은 허브나 라우터 같은 네트워크 기기의 역할이다.
- 패킷 송, 수신 동작의 출발점은 TCP 담당이 IP 담당에 패킷 송신을 의뢰하는 곳부터 시작된다.
    - 이때 TCP 담당은 데이터 조각에 TCP 헤더를 부가한 것을 IP 담당에 전달한다.
- IP 담당은 받아서 앞에 IP 헤더랑 MAC 헤더를 부가한다.
    - IP 헤더는 IP 프로토콜에 규정된 규칙에 따라 IP 주소로 표시된 목적지 까지 패킷을 전달할 때 사용하는 제어 정보를 기록한 것이다.
    - MAC 헤더는 이더넷 등의 LAN을 사용하여 가장 가까운 라우터까지 패킷을 운반할 때 사용하는 제어 정보를 기록한 것이다.
- 만든 패킷을 네트워크용 하드웨어에 건네준다.
    - 하드웨어는 이더넷이나 무선 LAN 등을 말한다.
    - LAN 어댑터라고 통칭할꺼임
- LAN 어댑터에 건네줄 패킷의 모습은 0이나 1의 비트가 이어진 디지털 데이터라고 생각하면 된다.
- LAN 어댑터에 의해 전기나 빛의 신호 상태로 바뀌어 케이블에 송출된다.
- 신호는 허브나 라우터 등의 중계 장치에 도착하고 중계 장치가 상대가 있는 곳까지 패킷을 전달한다.
- 상대가 보내는 회답은 거꾸로 하면 된다.
- IP 패킷의 송, 수신 동작은 패킷의 역할에 관계없이 모두 같다.(TCP 패킷과 다르다.)
    - 안에 뭐가 들어있는지, 패킷의 순번이 바뀌어서 들어오는지, 패킷이 없어졌는지 상관하지 않는다.
    - 그냥 의뢰받은 내용물을 패킷의 모습으로 만들어서 상대에게 송신하거나 전달한 패킷을 수신할 뿐이다.

## 수신처 IP 주소를 기록한 IP 헤더를 만든다.

IP 담당은 TCP 담당에서 패킷 송, 수신을 의뢰 받으면 IP 헤더를 만들어서 TCP의 헤더의 앞에 붙인다.

- IP 헤더에서 가장 중요한 것은 패킷을 어디로 전달해야 하는지를 나타내는 수신처 IP 주소이다.
    - IP는 스스로 수신처를 판단하지 않고 어플리케이션이 지정한 IP 주소를 그대로 헤더에 설정한다.
        - 잘못 지정해도 보내버린다.
- 송신처 IP 주소도 설정한다.
    - 컴퓨터에 여러개의 LAN 어댑터가 장착될 수 있기 때문에 송신처의 IP 주소도 여러개일 수 있다.
- 프로토콜 번호는 누가 의뢰한 것인지를 나타내는 값을 설정한다.
    - 06번 → TCP, 17번 → UDP

## 이더넷용 MAC 헤더를 만든다.

IP 헤더를 만들었으면 IP 담당 부분의 앞에 MAC 헤더를 붙인다.

- 이더넷은 TCP/IP와 다른 구조로 패킷의 수신처를 판단하기 때문에 MAC 헤더를 사용한다.
- IP 주소처럼 MAC 주소(송신처, 수신처)를 헤더의 맨 앞에 기록한다.
    - 48비트
- 이더 타입은 IP 헤더의 프로토콜 번호와 비슷하다.
    - MAC 헤더 뒤에 이어지는 패킷의 내용물이 무엇인지를 나타내는 값이다.
- 송신처의 MAC 주소는 자체의 LAN 어댑터의 MAC 주소를 설정한다.
    - MAC 주소는 LAN 어댑터를 제조할 때 그 안에 있는 ROM에 기록되므로 여기에 기록되어 있는 값을 읽어와서 MAC 헤더로 설정한다.
- 수신처의 MAC 주소는 일단 패킷을 누구한테 줄지를 먼저 조사해야한다.
    - IP 주소먼저 찾고 IP 주소에서 MAC 주소를 조사하는 동작을 실행한다.

## ARP로 수신처의 라우터의 MAC 주소를 조사한다.

IP 주소에서 MAC 주소를 찾을 때 사용되는 것이 ARP이다. (Address Resolution Protocol)

- ARP의 개념은 이더넷에 연결되어 있는 모두에게 패킷을 전달하는 브로드캐스트 구조가 있는데 이 구조를 이용해서 일치하는 IP 주소가 있는지 브로드캐스트하고 응답을 받는 방식이다.
- 패킷을 보낼 때 마다 이 동작을 하면 ARP의 패킷이 불어나기 때문에 ARP 캐시라는 메모리 영역에 보존해서 다시 이용한다.
- ARP 캐시에 저장된 값이 유효한지 모르기 때문에 특정 시간이 지나면 삭제하게 되어있다.
- MAC 헤더를 IP 헤더의 앞에 붙이면 패킷이 완성된다.
- 여기까지가 IP 담당 부분의 역할이다.

## 이더넷의 기본

이더넷은 다수의 컴퓨터가 여러 상대와 자유롭게 적은 비용으로 통신하기 위해 고안된 통신 기술이다.

- 네트워크의 실체는 케이블만 있다.
    - 트랜시버라는 작은 기기도 있는데 이것은 케이블 사이에 신호를 흘리는 역할만 하고 케이블과 같은 것이다.
- 컴퓨터가 신호를 송신하면 케이블을 통해 네트워크 전체에 신호가 흐르고 전원에게 도착한다.
    - 도착한 신호가 누구것인지 판단하기 위해 수신처 주소를 써둔다.
    - 이 방법을 통해 원하는 상대에게만 패킷이 도착한다. (주소가 다른 놈들은 패킷을 폐기)
    - 이게 이더넷
- 현재는 스위칭 허브를 이용한 형태의 이더넷이 보급되었는데 다른 점은 수신처 MAC 주소로 나타내는 원한느 기기가 존재하는 부분에만 신호가 흐르고, 다른 곳에는 신호가 흐르지 않게 된 것이다.
- 이런 변화가 있더라도 수신처 MAC 주소에 기억된 상대에게 패킷을 전달하고, 송신처 MAC 주소로 송신처를 나타낸 후 이더 타입으로 내용물이 무엇인지 나타내는 3가지 성질은 변하지 않았고 이러한 성질을 가진 것을 이더넷이라고 생각하면 된다.
- 이더넷에 접속된 기기는 이더넷이라는 하나의 사양에 기초하여 동작하기 때문에 클라이언트 PC 뿐만 아니라 서버나 라우터를 포함한 모든 기기에 공통적으로 적용된다.

## IP 패킷을 전기나 빛의 신호로 변환하여 송신한다.

- IP가 만든 패킷은 메모리에 기억된 디지털 데이터이므로 이것을 그대로 상대에게 보낼 수 없다.
- 디지털 데이터를 전기나 빛의 신호로 변환하여 네트워크의 케이블에 송출하는데 이것이 송. 수신 동작의 본질이라고 할 수 있다.
    - 이 동작을 수행하는 것이 LAN 어댑터이다.
- LAN 어댑터는 전원을 공급하면 즉시 사용할 수 있는 것이 아니라 다른 하드웨어와 마찬가지로 초기화 작업이 필요하다.
    - LAN 드라이버가 하드웨어의 초기화 작업을 수행해야 사용 가능하다.
    - 이것 저것 많이하는데 이더넷 특유의 작업도 있다.
    - 바로바로 이더넷의 송, 수신 동작을 제어하는 MAC 이라는 회로에 MAC 주소를 설정하는 것이다.
    - LAN 어댑터의 ROM에는 전 세계적으로 중복되지 않도록 일원화해서 관리하는 MAC 주소를 제조할 때 기록하기 때문에 이것을 읽어와서 MAC 회로에 설저하는 것이다.

## 패킷에 3개의 제어용 데이터를 추가한다.

LAN 드라이버는 IP 담당 부분에서패킷을 받으면 LAN 어댑터의 버퍼 메모리에 복사한다.

복사를 마친 후 패킷을 송신하도록 MAC 회로에 명령을 보내면 MAC 회로의 작업이 시작된다.

- MAC 회로는 먼저 송신 패킷을 버퍼 메모리에서 추출해서 아래 데이터를 패킷에 부가한다.
    - 프리앰블, 스타트 프레임 딜리미터를 맨 앞에
        - 프리앰블은 송신하는 패킷을 읽을 때의 타이밍을 잡기 위한 것이다.
    - 프레임 체크 시퀀스라는 오류 검출용 데이터를 맨 뒤에

## 허브를 향해 패킷을 송신한다.

앞에 과정을 거치면 케이블에 송출하는 패킷이 완성된다.

- 이제 신호를 송신하는 동작을 수행하는데 이 동작은 두 가지로 나뉜다.
    - 리피터 허브를 사용했을 때의 반이중 모드
    - 스위칭 허브를 사용한 전이중 모드
- 반이중 모드의 동작은 신호의 충돌을 피하기 위해 아래와 같이 동작한다.
    - 케이블에 다른 기기가 송신한 신호가 흐르고 있는지 조사하고, 신호가 흐르고 있으면 그것이 끝날 때 까지 기다린다.
    - 신호를 송신하고 있는 사이에 수신 신호가 흘러들어오는 경우도 있다.
        - 리피터 허브를 사용한 반이중 모드의 경우 이러한 사태가 되면 서로의 신호가 뒤섞여서 분간할 수 없는 상태가 되는데 이것이 바로 충돌이라는 현상이다.
    - 이렇게 되면 이상 송신을 계속해도 의미가 없기 때문에 송신 동작을 중지한다.
    - 충돌이 일어난 사실을 다른 기기에 알리기 위해 재밍 신호라는 특수한 신호를 잠시 동안 흘린다.

## 돌아온 패킷을 받는다.

- 리피터 허브를 이용한 반이중 동작의 이더넷에서는 1대가 송신한 신호가 리피터 허브에 접속된 케이블 전부에 흘러간다.
