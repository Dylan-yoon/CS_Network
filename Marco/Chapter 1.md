
### 01 웹 브라우저가 메시지를 만든다

_ 웹 브라우저의 내부 탐험

01. HTTP 리퀘스트 메시지를 작성한다.
    1. 탐험 여행은 URL 입력부터 시작한다.
        
        - URL (Uniform Resource Locator) : 웹 서버에 액세스하는 방법
            
        - 액세스의 대상에 따라 다른 사용법
            
            - 웹 서버 또는 FTP 서버 접근시 서버 도메인 명이나 액세스 파일 경로 작성
            - 메일의 경우 보내는 상대의 메일 주소 포함
        - 필요에 따라 사용자 명이나 패스워드 서버측 포트번호 등 작성
            
        - URL 맨 앞 문자열 (`http:` `ftp:` `file:` `mailto:`) : 액세스 방법
            
            → 액세스할 때의 프로토콜 종류
            
    2. 브라우저는 먼저 URL을 해독한다.
        - URL 해독
        - `http://www.naver.com/dir1/file1.html`
            - `http:` - 데이터 출처에 액세스하는 방법 (프로토콜)
            - `//` - 다음 문자열이 서버의 이름임을 나타냄
            - [`www.naver.com`](http://www.naver.com) - 웹 서버명
            - `/디렉토리명 …` - 디렉토리 경로
            - `… /파일명` - 파일명
    3. 파일명을 생략한 경우
        - 끝이 `/` 로 끝난 것은 파일명을 쓰지 않고 생략
            
            → `index.html` 또는 `default.htm` 등의 기본 파일
            
        
        > 브라우저가 가장 먼저 하는 일은 URL을 해독하는 것
        
    4. HTTP의 기본 개념
        - HTTP 프로토콜은 클라이언트와 서버가 주고 받는 메시지의 내용이나 순서를 정한것
        - 먼저 클라이언트에서 서버를 향해 리퀘스트 메시지 보냄
        - 리퀘스트 메시지의 안에는 `무엇을` `어떻게해서` 하겠다
            - URI : `무엇을`
            - 메소드 : `어떻게해서`
        - 응답메시지의 맨 앞부분에는 실행 결과가 정상 종료되었는지 또는 이상이 발생했는지를 나타내는 `Status Code`
        - 메소드
            - GET : URL 지정한 정보 도출 (파일 내용 되돌려보냄)
            - POST : 크라이언트에서 서버로 데이터를 송신
            - HEAD : HTTP 메시지 헤더만 반송하고 데이터 돌려보내지 않음 (최종 갱신등)
            - OPTION : 통신 옵션을 통지하거나 조사할 때 사용
            - PUT : URI로 지정한 서버의 파일을 치환, 파일 없을 경우 새 파일로 작성
            - DELETE : 파일 삭제
            - TRACE : 서버측에서 받은 리퀘스트 라인과 헤더를 그대로 클라이언트에 반송
            - CONNECT : 암호화된 메시지를 프록시로 전송할 때 이용하는 메서드
    5. HTTP 리퀘스트 메시지를 만든다
        - 첫번째 행 : 리퀘스트 라인 - 웹 브라우저는 웹 서버에 어떻게 할 것인지를 전달
            
            <메소드><공백><URI><공백><HTTP> 버전
            
        - 두번째 행 : 메시지 헤더 - 부가적인 자세한 정보를 써두는 역할
            
        - 공백행 후 네번째 행 : 메시지 본문 - 메시지의 실제내용 (GET은 없음)
            
    6. 리퀘스트 메시지를 보내면 응답이 되돌아온다
        > 리퀘스트 메시지에 쓰는 URI는 하나 뿐으로, 복수의 파일을 읽을 때는 웹서버에 별도의 리퀘스트 메시지 보냄
        
02. 웹서버의 IP주소를 DNS 서버에 조회한다.
    1. IP 주소의 기본
        - URL 안에 쓰여있는 서버의 도메인명에서 IP주소를 조사
        - 서브넷이라는 작은 네트워크를 라우터로 접속해서 전체 네트워크가 만들어짐
        - IP주소
            - 동에 해당하는 번호를 **네트워크 번호**
            - 번지에 해당하는 번호를 **호스트 번호**
            - 라우터 : 패킷을 중계하는 장치의 일종.
            - 허브 : 패킷을 중계하는 장치의 일종, 리피터 허브와 스위칭 허브
            - 호스트 번호 부분의 비트가 모두 1인 것은 서브넷, 전체에 대한 브로드캐스트
            - 넷마스크
            - 모두 0 : 서브넷 자체
    2. 도메인명과 IP 주소를 구분하여 사용하는 이유
    3. Socket 라이브러리가 IP 주소를 찾는 기능을 제공한다.
        - DNS의 원리를 사용해서 IP 주소를 조사하는 것을 네임 리졸루션(Name Resoluton, 이름 확인), 리졸루션을 실행하는 것이 리졸버(resolver)
        - Socket 라이브러리 : 네트워크의 기능을 호출하기 위한 프로그램의 부품집
    4. 리졸버를 이용해서 DNS 서버를 조회한다.
    5. 리졸버 내부의 작동
        - 메시지 송신 동작은 리졸버가 스스로 실행하는 것이 아니라 OS의 내부에 포함된 **프로토콜 스택**을 호출해서 실행을 의뢰
        - 서로역할 분담. 상위 계층에서 무엇인가 의뢰 했을때 그 일을 스스로 전부 실행하지 않고 하위 계층에 실행을 의뢰하면서 처리를 진행
03. 전 세계의 DNS 서버가 연대한다.
    1. DNS 서버의 기본 동작
        - 클래스 - 지금은 인터넷 이외의 네트워크는 소멸되었으므로 클래스는 항상 인터넷을 나타내는 ‘IN’ 이라는 값
        
        > DNS 서버는 서버에 등록된 도메인 명과 IP 주소의 대응표를 조사해서 IP 주소를 확답
        
        리소스 레코드
        
    2. 도메인의 계층
        - 정보를 분산시켜서 다수의 DNS 서버에 등록하고, 다수의 DNS 서버가 연대해서 어디에 정보가 등록되어 있는지 찾아내는 구조
        - 인터넷의 도메인은 모두 이렇게 해서 도메인의 하위 도메인을 만들고 국가나 회사 및 단체 등에 할당한 것
    3. 담당 DNS 서버를 찾아 IP 주소를 가져온다.
        
    4. DNS 서버는 캐시 기능으로 빠르게 회답할 수 있다.
        - 현실에는 상위와 하위의 도메인을 같은 DNS 서버에 등록하는 경우도 있음

04. 프로토콜 스택에 메시지 송신을 의뢰한다.
    5. 데이터 송수진 동작의 개요
        - OS의 내부에 있는 프로토콜 스택에 의뢰
        
        > OS 내부의 프로토콜 스택에 메시지 송신 동작을 의뢰할 때 Socket 라이브러리 프로그램 부품을 결정된 순번대로 호출
        
        - Socket : 송수신 동작하기전 송수진하는 양자 사이를 파이프로 연결하는 데이터의 양 출입구
        - 데이터를 전부 보내고 나면 연결했던 파이프 분리
        - OS 내부의 프로토콜 스택
            1. 소켓 만듬 (소켓 작성단계)
            2. 서버측의 소캣에 파이프 연결 (접속단계)
            3. 데이터를 송수신 (송수신단계)
            4. 파이프를 분리하고 소켓을 말소 (연결 끊기단계)
    6. 소켓의 작성 단계
        - 소켓이 생기면 디스크립터 (Discripter) 라는 것이 돌아오므로 애플리케이션은 이것을 받아서 메모리에 기록
        - 디스크립터 : 소켓을 식별하기 위해 사용
    
    1. 파이프를 연결하는 접속단계
        
        - IP주소 : 네트워크에 존재하는 각 컴퓨터를 식별하기 위해 각가에 서로 다른 값을 할당
        - 포트번호 : 소켓지정 중간과정
        - IP 주소와 포트번호의 두가지를 지정해야 어느 컴퓨터의 어느 소켓과 접속할지를 분명히 지정 가능
        - 디스크립터는 컴퓨터 한대의 내부에서 소켓을 식별하기 위해 사용하지만, 포트 번호는 접속 상대측에서 소켓을 식별하기 위해 사용
        - 서버측의 포트 번호는 애플리케이션의 종류에 따라 미리 결정된 값을 사용하는 규칙
            - 웹은 89
            - 매일 25
        - 상대와 연결되면 프로토콜 스택은 연결된 상대의 IP 주소나 포트 번호 등의 정보를 소켓에 기록
    8. 메시지를 주고 받는 송/수신 단계
        
        - 수신 버퍼 : 수신한 응답 메시지를 저장하기 위한 메모리 영역을 지정
    9. 연결 끊기 단계에서 송/수신이 종료된다.
        
        - 하나의 웹페이지에 영상이 많이 포함되어 있으면 접속, 송/수신, 연결 끊기 동작을 여러 번 반복