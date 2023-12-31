## Story_06 UDP 프로토콜을 이용한 송_수신 동작.
### TCP UDP

데이터를 확실히 보내는 가장 단순한 방법은 패킷 하나만 사라져도 다시 전부 보내는 것.<br>
TCP는 전송되지 않은 패킷을 확인해 효율적으로  도착하지 않은 패킷만 다시 보내준다.<br>
TCP 가 복잡하지만 데이터를 확실히 전달하고 도착한 것을 확인할 수 있다.

UDP는 TCP 와는 다르게 하나의 패킷이 손실되거나, 오류로 받지 못할경우 전부들 다 보내도 1개의 패킷이기 떄문에 낭비라고 볼 수 없다.

### 제어용 짧은 데이터

DNS 서버에 대한 조회<br>
제어용으로 실행하는 정보 교환은 한개의 패킷으로 끝나는 경우가 많기 때문에 UDP 를 사용한다.

UDP는 수신 확인, 윈도우가 없다. 연결끊기 단계도 없다.

오류가 생기면 회답이 돌아오지 않기 때문에 애플리케이션은 그 사실을 알아차리고 다시 데이터를 보낸다.

UDP 헤더
- 송신처 포트 번호
- 수신처 포트 번호
- 데이터 길이
- 체크섬

### 영상, 음악 음성 데이터도 UDP 를 사용한다.
결정된 시간 안에 데이터를 건대주어야 하므로, 데이터 도착이 지연되면 재생 타이밍이 맞지않아 끊기거나 멈춘다.<br>
TCP는 UDP 보다 절차가 더 길기 때문에 오류가 나서 다시 전송한다 해도 타이밍이 맞지 않을 수 있다.
