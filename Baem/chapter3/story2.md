## Story_02 스위칭 허브의 패킷 중계 동작
#### 리피터와는 별개로 스위칭 허브는 목적지를 향해 중계하도록 만들어져 있다.

스위칭 허브의 패킷 중계동작을 간단하게 보면 아래와 같다<br>
신호 -> 커넥터 -> PHY(MAU) -> 메모리 -> 스위치 회로 -> [송출]

스위칭 허브는 
1. 신호를 받아들인 후
2. FCS 대조 후 이상 없으면 버퍼 메모리에 저장한다.
3. 다음 수신처의 MAC 주소가 `주소표`에 등록 되어있는지 조사
4. 주소표에 있다면 스위치 회로를 경유해 송신측 포트에 보냄

LAN 어댑터와 달리 스위칭 허브는 MAC 주소가 없기 때문에 MAC 주소를 검사하지 않고 메모리에 저장한다.

스위치 회로 특징 요약
- 신호선이 격자 모양으로 배치, 교점에 스위치가 있음
- 복수의 신호를 동시에 흘릴 수 있음
- 교점의 스위치가 각각 독립하여 움직임

### MAC 주소 테이블 등록 & 갱신

갱신 동작
1. 패킷을 수신했을 때 MAC 주소 조사
   1. 한번이라도 패킷을 송신하면 해당 기기의 MAC 주소가 MAC 주소표에 등록된다.
2. 몇 분 마다 오래된 MAC 주소 삭제
   1. 예시) 노트북을 다른 장소로 이동하여 연결 할 때


#### 예외동작
[PC1, PC2 -> 리피터허브 -> 스위칭 허브] 로 구성된 경우<br>
PC1에서 받은 패킷을 리피터 허브를 거치게 된다.<br>
이 때, 리피터 허브는 모든 곳에 패킷을 전달하게 되고, <br>
스위칭 허브로 도착했을 때 다시 리피터 허브로 돌아와야 하는데, <br>
이는 정상적으로 작동 되지 않으므로 스위칭 허브는 패킷을 중계하지 않고 폐기한다.

전이중, 반이중 모드
- 전이중 : 송수신 동시에 -> 스위치 허브
- 반이중 : 송 수신 중 하나만 가능 -> 리피터 허브

스위치 허브에서는 PHY 에서 송신과 수신의 충돌검사를 진행한다.

#### 최적의 전송 속도를 위한 `자동 조정 기능`

링크펄스 : 펄스형 신호로 데이터가 흐르지 않을때 이 신호를 흘려 지속적으로 신호를 유지시킨다.
아래와 같은 사항들을 확인 할 수 있다.
- 상대가 올바르게 작동하는가.
- 케이블 단선이 있는가

** 리피터 허브와는 다르게 스위칭 허브는 복수의 중계 동작을 동시에 실행한다.