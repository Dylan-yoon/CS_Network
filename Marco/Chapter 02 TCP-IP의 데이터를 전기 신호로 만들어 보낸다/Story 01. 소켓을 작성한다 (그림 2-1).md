### 1. 프로토콜 스택의 내부 구성
- 네트워크 애플리케이션 : 브라우저, 메일러, 웹 서버, 메일 서버등의 프로그램이 여기 층에 해당
- Socket 라이브러리 이씅며, 그 안에는 리졸버가 내장 → DNS 서버 조회하는 동작 실행
- OS : 프로토콜 스택
- TCP 프로토콜, UDP 프로토콜
- IP 프로토콜
- LAN 드라이버 : LAN 어댑터의 하드웨어를 제어

### 2. 소켓의 실체는 통신 제어용 제어 정보
- 프로토콜 스택은 내부에 제어 정보를 기록하는 메모리 영역을 가지고 통신 동작을 제어하기 위한 제어 정보를 기록 → 소켓의 실체 (제어 정보)
- 프로토콜 스택은 소켓에 기록된 제어 정보를 참조하며 움직임 (그림2-2)
- ‘Socket’ Socket 라이브러리 / ‘socket’ Socket 라이브러리 안에 프로그램 / ‘소켓’ 양끝에 있는 출입구

### 3. Socket을 호출했을 때의 동작 (그림 2-3)
- 소켓을 만들 때 한 개의 메모리 영역을 확보하고 초기 상태라는 것을 이 영역에 기록
- 디스크립터 : 프로토콜 스택의 내부에 있는 다수의 소켓 중 어느것을 가리키는 지 나타내는 정보
- 디스크립터를 받은 애플리케이션은 이후 프로토콜 스택에 데이터 송/수신 동작을 의뢰할 때 디스크립터에 통지 → 통신 상대 정보를 일일이 통지 받을 필요가 없음