# 01. 소켓 작성

OS에 내장된 네트워크 제어용 SW(프로토콜 스택)와 네트워크용 하드웨어(LAN 어댑터)가 브라우저에서 받은 메세지를 서버에 송출하는 동작을 알아본다.

### 프로토콜 스택 내부 구성

---

- 맨위에 있는 네트워크 애플리케이션(브라우저, 메일러, 웹 서버)이 아래로 향하여 데이터 송수신등의 일을 의뢰한다.


![스크린샷 2023-08-05 오후 9 26 56](https://github.com/AlgoMango/Season2/assets/59204352/704ccf3c-2d3f-4a87-901b-9383211459fd)

- OS 내부에는 프로토콜 스택이 있고, 프로토콜 스택의 윗부분에는 **TCP, UDP 프로토콜**을 사용하여 데이터 송.수신을 담당하는 부분이 있다.
- 그 다음은 **IP 프로토콜**을 사용하여 패킷 송.수신을 제어하는 부분이 있다.
    - 인터넷에서는 데이터를 운반할 때, 패킷이라는 형태로 운반한다.
    - 데이터는 수십 바이트에서 수천 바이트 정도의 작은 덩어리로 분할되어 운반되는데, 이 덩어리가 **패킷**이다.
- IP 아래의 **LAN 드라이버**는 LAN 어댑터의 하드웨어를 제어한다.
- 그리고 **LAN 어댑터**가 실제 송.수신 동작을 수행한다.(케이블에 대해 신호를 송.수신)

### 소켓의 실체는 통신 제어용 제어정보

- 프로토콜 스택은 내부에 제어정보를 기록하는 메모리 영역을 가지고 있다.
- 여기에 통신 동작을 제어하기 위한 제어정보를 기록한다.
    - IP주소, 포트번호, 통신 동작의 진행상태 등을 기록한다.
- 프로토콜 스택은 이 정보들을 참조하여 동작한다.
- **이러한 제어 정보들을 기록한 메모리 영역이 곧 소켓이다. (사실 소켓은 실체가 없다)**
- 프로토콜 스택은 소켓을 참조하면서 다음에 무엇을 해야할지 판단한다.

![스크린샷 2023-08-05 오후 9 16 56](https://github.com/AlgoMango/Season2/assets/59204352/bacbbab2-b326-48cf-92cb-415ca66a2ba1)

- 한 행이 하나의 소켓이다.
- 소켓을 만든다 == 새로 한 행의 제어정보를 추가하고, 상태를 기록하고 통신을 준비하는 작업.

(Foreign Address가 상태측이다. 0.0.0.0인 경우 아직 통신 시작안되거나 연결 안된 것이다.)

### Socket 만드는 과정

---

애플리케이션에서 socket 호출하면 프로토콜 스택한테 소켓 만들어줘라고 의뢰한다.

- 프로토콜 스택은 소캣 한개 분량의 메모리 영역 확보 후, 초기 제어 정보를 기록한다.
- 소켓만들면 **디스크립터**가 반환된다. (소켓 식별자)
- 애플리케이션은 프로토콜 스택에 데이터 송수신을 의뢰할때 이제 디스크립터를 통지한다. 
(디스크립터로 어느 소켓인지를 알 수 있으니까)
