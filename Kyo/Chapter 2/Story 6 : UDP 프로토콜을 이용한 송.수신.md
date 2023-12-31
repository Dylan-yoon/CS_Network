# 06. UDP 프로토콜을 이용한 송.수신

### 01. 수정 송신이 필요없는 데이터의 송신은 UDP가 효율적이다.

---

대부분의 애플리케이션은 TCP 프로토콜을 사용하여 데이터 송.수신을 실행한다.

하지만 UDP 프로토콜을 사용하는 경우도 있다.

- 예시 : DNS 서버에 IP 주소를 조회할때 UDP를 사용한다.

**왜사용할까?**

- TCP는 복잡하다. 패킷이 한개가 없어지면 어디까지 받았는지 수신하고 또 송신해야한다.
- TCP는 데이터를 확실하면서 효율적으로 전달하기 위해, 도착을 확인한다.
- 그러면 패킷이 유실되면 싹다 다시보내면 되지않을까? → 비효율적이다.

### 02. 제어용 짧은 데이터

---

DNS 서버에 대한조회등 제어용으로 실행하는 정보는 한개의 패킷으로 끝나는 경우가 많다.

→ TCP가 아니라 UDP를 사용한다. 

UDP는 TCP와 달리 수신확인이나 윈도우가 없어서 데이터 송.수신전에 제어 정보를 주고 받을 필요가 없고, 접속이나 연결 끊기 단계가 없다.

- 만약 오류가 발생하여 패킷이 없어진다면 ? → 모른척한다.

**오류가 발생하면 회답이 없기 때문에 애플리케이션이 데이터 전체를 한번 더 다시 보내면 그만이다.**

### 03. 음성 및 동영상 데이터

---

음성이나 영상의 데이터를 보낼때도 UDP를 사용한다.

- 음성, 영상의 경우 데이터를 결정된 시간에 맞춰서 데이터를 보내야한다.
- TCP 사용하면 너무 느리다.
- 데이터가 유실되도, 잠깐 지직이나 영상이 멈추거나 화질이 안좋아지는게 전부다.

→ UDP 가 더 효율적이다.

### 정리

---

### TCP 특징

- 연결지향 - TCP 3 way handshake (가상연결)
    - 연결 확인 후 메세지 보낸다.
    - 얘 때문에 조금 느리다.
- 데이터 전달 보증
- 순서 보장

### UDP 특징 (기능이 거의 없다)

- 연결지향X
- 데이터전달보증 X, 순서보장 X
- 단순하고 빠름
- IP와 거의 같다 하지만 PORT + 체크섬 정도 추가
- 애플리케이션에서 추가작업 필요
- 요새 뜨기 시작.
