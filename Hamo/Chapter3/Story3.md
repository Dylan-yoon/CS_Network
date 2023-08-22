# 라우터의 패킷 중계 동작
리피터 허브나 스위칭 허브를 경유한 패킷은 결국 라우터에 도착하고. 라우터에서 다음 라우터로 중계될 것이다.

- 중계 대상을 등록한 표를 보고 패킷을 어디로 중계해야 할지 판단하는 부분이 스위칭 허브와 고통이다.
    - 차이점은 라우터는 IP 라는 개념이 바탕이고 스위칭 허브는 이더넷이 바탕이다.
- 라우터의 내부 구조는 중계 부분과 포트 부분이라는 두 부분으로 구성된다.
    - 중계 부분: 패킷의 중계 대상을 판단하는 동작을 담당한다.
    - 포트 부분: 패킷을 송, 수신하는 동작을 담당한다.
- 라우터의 각 포트에는 MAC 주소와 IP 주소가 할당되어 있다.
- 라우터의 테이블은 라우팅 테이블, 경로표라고 부른다.
    - IP 주소로 중계 대상을 판단한다.
- 라우터는 호스트 번호는 무시하고 네트워크 번호 부분만 조사한다.
- 주소집약
    - 몇 개의 서브넷을 모아서 한 개의 서브넷으로 간주한 후 묶음 서브넷을 경로표에 등록할 수 있다.
    - 경로표를등록하는 건수를 줄일 수 있다.
- 라우터의 경로표에서 넷마스크 항목은 경로표의 수신처와 패킷의 수신처 주소를 대조할 때 비트 수를 나타낸다.
- 경로표에서 게이트웨이, 인터페이스 항목은 패킷의 중계대상을 나타낸다.
- 메트릭은 수신처 IP 주소에 기록되어 있는 목적지가 가까운지, 먼지를 나타낸다.
    - 수가 작으면 가까이에 있는 것이고 크면 먼 것을 나타낸다.
- 라우터의 경로표에 경로 정보를 등록하는 방법은 아래 두 가지로 분류할 수 있다.
    - 사람이 수동으로 경로 정보를 등록/갱신
    - 라우팅 프로토콜이라는 구조를 사용하여 라우터들끼리 경로 정보를 교환하고 라우터가 자체에서 경로표에 등록
- 라우터의 포트에는 MAC 주소가 할당되어 있으며, 라우터는 자신의 주소에 해당하는 패킷만 수신하고 해당하지 않는 패킷을 폐기한다.

## 경로표를 검색하여 출력 포트를 발견한다.

- 라우터는 패킷 수신 동작이 끝나면 맨 앞의 MAC 헤더를 폐기한다.
    - MAC 헤더의 역할은 이 라우터에 패킷을 건네주는 것이다.