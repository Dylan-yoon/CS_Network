# 소켓을 작성한다

OS에 내장된 네트워크 제어용 소프트웨어(프로토콜 스택), 네트워크용 하드웨어(LAN 어댑터)가 브라우저에서 받은 메시지를 서버에 송출하는 동작

<img width="478" alt="image" src="https://github.com/Dylan-yoon/CS_Network/assets/98260324/d96a6c50-e772-4569-af67-a07ea561eaac">

1. 애플리케이션에서 Socket 라이브러리의 리졸버를 호출해 DNS 서버에 IP 주소를 조회하는 동작을 실행
2. 애플리케이션이 TCP 또는 UDP를 사용한 데이터 송수신 동작을 프로토콜 스택에 의뢰
3. IP 프로토콜을 사용해 패킷 송수신 동작 제어
   -  IP: 패킷을 통신 상대까지 운반하는 것이 주 역할
4. LAN 드라이버가 LAN 어댑터를 제어하고, LAN 어댑터가 실제 케이블에 신호를 송수신하는 동작 실행


## 소켓
프로토콜 스택 내부에 있는 통신 동작을 제어하기 위한 제어 정보를 기록하는 메모리
  - 제어 정보: 상대 IP 주소, 포트 번호, 통신 동작의 진행 상태 등
  - 소켓은 개념적인 것으로 실체가 존재하지 않음(제어 정보 또는 제어 정보가 기록된 메모리 영역이 소켓의 실체)
  - 프로토콜 스택은 소켓에 기록된 제어 정보를 참조하면서 동작한다.

소켓 작성 단계
  1. 애플리케이션이 `socket`을 호출해 프로토콜 스택에 소켓 작성 의뢰
  2. 프로토콜 스택이 1개의 소켓 생성
     - 소켓 1개 분량의 메모리 영역 확보
     - 송수신 동작이 시작되지 않은 초기 상태임을 나타내는 제어 정보 기록
  3. 소켓을 나타내는 디스크립터를 애플리케이션에 알려준다.
  4. 애플리케이션이 프로토콜 스택에 데이터 송수신 동작을 의뢰할 때 디스크립터 통지
  5. 프로토콜 스택에서 디스크립터를 통해 소켓을 찾아 제어 정보를 얻는다.
