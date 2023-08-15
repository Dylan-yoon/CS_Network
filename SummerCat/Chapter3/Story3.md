# 라우터의 패킷 중계 동작
리피터 허브/스위칭 허브를 경유한 패킷이 라우터에 도착

## 라우터
중계 부분과 포트 부분으로 구성
  - 중계 부분: 패킷의 중계 대상 판단
  - 포트 부분: 패킷 송수신. 포트 부분의 통신 기술 규칙(이더넷, 무선 LAN, ADSL, FFTH, ...)에 따라 동작

### 라우터와 스위칭 허브의 관계
- 라우터: IP 기반
  - 통신 상대까지 패킷을 전달하는 전체 동작 담당
- 스위칭 허브: 이더넷 기반
  - 다음 라우터까지 패킷 운반 담당
- 라우터는 패킷을 운반하는 일을 스위칭 허브에 의뢰
  - = IP는 이더넷에 다음 목적지까지 패킷 운반을 의뢰

### 라우터의 패킷 중계 동작
1. 포트 부분에서 패킷 수신
2. 중계 부분에서 받은 패킷의 IP 패킷에 기록된 수신처 IP 주소와 라우팅 테이블(경로표)를 대조해 중계 대상 판단
3. 중계 대상 측 포트로 패킷을 옮기고, 포트 부분의 하드웨어 규칙에 따라 패킷 송신
4. (여기부터 수신) 신호가 커넥터에 도착하면 라우터 내부 PHY/MAU 회로와 MAC 회로에서 디지털 신호로 변환
5. 패킷 끝의 FCS를 점검하여 오류 유무 점검
6. MAC 헤더의 수신처 MAC 주소와 자신의 MAC 주소 일치 여부 확인
7. 패킷 수신 동작이 끝나면 MAC 헤더 폐기
8. (여기서부터 송신) IP 패킷의 맨 앞에 MAC 헤더를 추가해서 이더넷 패킷으로 만듦
   - 수신처 MAC 주소 필드에 값 설정
     - 경로표의 게이트웨이 항목의 IP 주소
     - 만약 이 값이 비어 있다면 IP 헤더의 IP 주소
     - IP 주소가 결정되면 ARP로 IP 주소에서 MAC 주소를 조사
   - 송신처 MAC 주소 필드에 출력 측 포트에 할당된 MAC 주소 설정
9. 만들어진 송신 패킷을 전기 신호로 변환하여 포트에서 송신

### 경로표의 정보
- 수신처
- 넷마스크
- 게이트웨이
- 인터페이스
- 메트릭: 수신처 IP 주소에 기록된 목적지까지의 거리를 수로 표현
  - 라우터 하나를 거칠 떄마다 1씩 감소 → 클 수록 멀다

게이트웨이와 인터페이스: 패킷의 중계 대상
  - 인터페이스에 등록된 인터페이스(포트)에서 게이트웨이 항목에 등록된 IP 주소를 가진 라우터에 패킷을 중계

경로표 맨 마지막 행의 넷마스크는 `0.0.0.0`, 게이트웨이에 원하는 기본 게이트웨이 값 설정
  - 수신처 IP 주소에 해당하는 행이 경로표에 없을 경우 여기(기본 게이트웨이)로 보낸다.
  - 이것을 **기본 경로**라고 함

## 패킷 유효 기간
패킷 IP 헤더의 TTL 필드에 해킷의 유효 기간을 설정
  - 값은 보통 64 또는 128
  - 라우터 하나를 경유할 때마다 1씩 줄어든다.
  - 0이 되면 패킷 폐기
  - 패킷이 계속 순환하는 것을 막기 위함