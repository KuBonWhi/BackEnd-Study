# URI와 웹 브라우저 요청 흐름

## URI, URL, URN

- URI는 URL(locator), URN(name)을 포함함
- URL은 우리가 흔히 웹 브라우저에서 사용하는 주소
- URN은 이름을 부여하는 것. 이름만 가지고는 주소를 찾아갈 수 없기에 실제로 사용하기 힘듬

## URI

- Uniform
  - 리소스를 식별한느 통일된 방식
- Resource
  - 자원, URI로 식별할 수 있는 모든 것
- Identifier
  - 다른 항목과 구분하는데 필요한 정보

## URL, URN

- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않음

## URL 분석

- scheme://[userinfo@]host[:port][/port][/path][?query][#fragment]
- 예시 : `https://google.com/search?q=hello&hl=ko`
- 프로토콜 : https
- 호스트명: google.com
- 포트번호: 443
- 패스: /search
- 쿼리 파라미터: q=hello&hl=ko

## 웹 브라우저의 요청 흐름

- 1. DNS 조회해서 IP 정보를 가져오고, 포트 정보도 확인(생략시 기본 포트)
- 2. 웹 브라우저가 HTTP 요청 메세지 생성
- 3. SOCKET 라이브러리를 통해 전달
  - 3-1. TCP/IP 연결 : 3 way handshake
  - 3-2. 데이터 전달
- 4. TCP/IP 패킷 생성. HTTP 메세지도 포함
- 5. 요청 패킷을 서버에 전달
- 6. 서버에서는 HTTP 메세지를 해석해서 HTTP 응답 메세지를 클라이언트에 전달
- 7. 클라이언트에서는 받은 응답 메세지로 브라우저에 렌더링

### 3 way handshake가 발생하는 시점

- 애플리케이션 계층에서 Socket 라이브러리를 통해 TCP 계층으로 데이터를 전달할 때,
  Socket 라이브러리가 커넥션을 TCP/IP로 맺으라고 전달하면
  그 하부에서(TCP 계층) 3 way handshake를 실행하고 연결이 됨.Socket 라이브러리는 TCP 계층에서 3 way handshake 작업이 일어나도록
  실행을 해주는 역할을 한다고 생각하면 된다.
- 3 way handshake도 마찬가지로 이를 위한 신호 정보가 패킷에 담겨서
  물리 계층을 통해 전달이 됨.
  HTTP 메시지가 포함된 패킷이 전달되기 전에
  연결을 위한 패킷이 먼저 왔다 갔다 하고, 연결이 확인되면 요청 패킷이 전달됨
