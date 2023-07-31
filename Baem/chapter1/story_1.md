## Story_01 HTTP 리퀘스트 메세지를 작성한다.
- URL: Uniform Resource Locator
- URL의 형식은 여러가지가 있다.
  - http://
  - ftp:
    - (FTP)File Transfer Protocol
  - file:
  - mailto:
  - etc.

위와같이 여러 형식이 있는 이유. 각 특징이 있음<br>
웹 서버를 요청하는 것 뿐만아닌, 데이터의 down/up load, 메일의 전송 등 여려거지 기능이 있음.

아래의 주소가 있을 때. <br>
`http://user:password@www.cyber.co.kr:80/dir/file1.htm`
- http:// -> `URL` 
- user -> 사용자명(생략가능)
- password -> 패스워드(생략가능)
- www.cyber.co.kr -> 웹 서버의 도메인명
- 80 -> 포트번호
- dir/file1.htm -> 파일경로

아래 형식을 프로토콜이라고 한다<br>
`[URL][사용자명][패스워드][웹 서버의 도메인명]:[포트번호]/[파일경로]`

### 브라우저의 URL해독
브라우저는 URL을 입력받아 어떤 요소로 되어있는지 분해한다.<br>
어떤 경로에 접근 할 것인지 프로토콜에 알맞게 분해 한다.<br>

우리가 기본적으로 `www.naver.com` 을 입력한다면, 파일명, 디렉토리 모두 생략한 것이다.<br>
하지만 서버에서 디폴트 값을 지정해 주었기 때문에, 접근이 가능 한 것. 

대부분의 서버가 아래경로로 엑세스 한다
- /dir/index.html
- /dir/default.htm

아래와 같이 url이 있다고 하자. <br>
`naver.com/sports`<br>
그렇다면 파일명인지, 디렉토리인지 알 수 없을 것이다.<br>
하지만 관례적으로 파일명, 디렉토리 명은 중복 될 수 없기 때문에, 적절하게 판단해준다.

### HTTP
URL을 해독해서 어디에 Access해야 할 지 판명된다.<br>
그 후 브라우저가 HTTP 프로토콜을 사용해 웹 서버에 엑세스한다.

#### 클라이언트 -> `[메소드 + URI]` -> 서버 -> [스테이터스 코드](#StatusCode) -> 클라이언트

주요 [메소드](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- GET: 경로를 보내 서버에 저장되어있는 해당 파일 및 디렉토리의 데이터를 가져온다.
- POST: `form(폼)` 에 데이터를 사용해서 웹 서버에 송신하는 경우 사용. 
- HEAD: 
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

보안상의 이유로 GET, POST 외에 기능을 숨겨둔다.

### MAKE REQUEST
#### 리퀘스트 메세지
- 리퀘스트 라인
  - 한 행으로 리퀘스트 내용을 대략적으로 안다.
- 메세지 헤더
  - 한 행에 한개의 헤더필드.
- 메세지 본문
  - POST Method로 웹 서버에 보낼 데이터가 들어간다.

#### 응답 메세지
- 스테이터스 라인
  - 스테이터스 코드 내용
- 메세지 헤더
- 메세지 본문
  - 데이터가 전달된다
  - 바이너리 데이터로 취급한다.

### Return Message
#### StatusCode
[Status Code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- 1xx: 처리의 경과 상황 등을 통지
- 2xx: 정상 종료
- 3xx: 무언가 다른 조치 필요
- 4xx: 클라이언트 측 오류
- 5xx: 서버 측 오류
