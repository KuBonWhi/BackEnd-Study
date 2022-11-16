# 스프링을 배우기 전 알아두면 좋은 개념

## Web Application Server vs Web Server

- web server
  - 단순한 정적인 리소스를 화면에 뿌려줄 수 있음. 대표적으로 Apache, Nginx 등이 있음
- WAS
  - 단순한 정적인 리소스를 화면에 뿌려주는 것은 물로, 동적으로 사용자에게 맞춰서 데이터를 뿌려 줄 수 있음.
  - 즉, 프로그래밍이 가능해 짐. 자바 진영에서는 서블릿 컨테이너를 가지게 된다면 WAS의 기능을 수행할 수 있음

## 웹 통신(HTTP)

- HTTP의 경우 L5 응용계층에서 사용하는 프로토콜
- L4(transport layer)에서 오는 TCP/IP 소켓에 대한 연경에 대한 처리가 필요
  - 패킷을 받는 경우 unpack, 패킷을 보내는 경우 pack 수행
- 소켓연결을 마치면, HTTP 요청 메세지를 파싱하고 헤더와 바디로 나누어 각각의 데이터가 어떤 행위를 포함하고 어떤 데이터를 보내려는 지를 parsing해야함
- 이후 들어온 요청에 의해 비지니스 로직을 마치면 응답메세지를 헤더와 바디로 나눠 작성하고 다시 TCP/IP 소켓에 대한 연결을 진행해서 L4계층으로 보내는 일까지 진행
- 서블릿은 비지니스 로직을 실행하는 일을 제외한 '수고스러운 일들을 모두 처리해주는 역할'

## 서블릿

- res, req를 통해 편리하게 HTTP 스펙을 사용할 수 있게 해줌
- WAS 클라이언트가 HTTP 요청을 보냄. 그러면 WAS는 해당 HTTP 요청을 파싱해서 request 객체와 response 객체를 생성
- 생성된 객체들을 기반으로 서블릿 객체를 호출하여 비지니스 로직을 수행
- 서블릿 호출이 종료될 경우 생성된 response 객체를 기반으로 HTTP 요청 응답을 생성하고 클라이언트에 반환

## Multi Thread

- 서블릿의 경우 Multi Thread 방식을 사용
- 요청이 들어올 때마다 쓰레드를 생성하여 요청을 수행. 단, 이러한 방식은 Context-Switching 이 일어나는 단점 발생
  - 쓰레드를 생성할때마다 비용이 너무 많이 발생
- Thread-Pool 이라는 곳에 지정된 수만큼의 쓰레드를 미리 만들어 놓고 '요청이 오면 해당 풀에서 쓰레드를 꺼내 사용하도록 하는 방식'

## MVC의 등장 배경

- 자바의 JSP와 서블릿만을 이용해서 웹서비스를 개발하면 코드가 복잡하게 얽혀 있음. 더한 문제는 '변경 생명 주기가 서로 다른 요소들이 결합(뷰+비즈니스 로직)'이 되어 있다는 점.
- UI가 변경되어야 하는 시점에 비지니스 로직도 함께 변경 됨.
- 하나의 파일에 너무 많은 역할을 부여하여 해당 파일을 유지보수하기 굉장히 어려움
- 이를 타파하고자 생성된 개념이 MVC 패턴

## MVC 패턴이란?

- view-logic과 business-logic을 아래의 세가지 요소를 통해 나누어 관리하는 기능을 수행
  - Model
    - 뷰에 출력할 데이터를 담아둠. 뷰가 필요한 데이터를 모두 모델에 담아서 전달해주기 때문에 뷰는 렌더링하는 일에만 책임을 가지게 됨.
  - View
    - 모델에 담겨있는 데이터를 받고 해당 데이터를 활용해서 사용자에게 뿌려질 화면을 그리는 일에 집중. 주로 HTML을 생성하는 일을 함.
  - Controller
    - HTTP 요청을 받아서 파라미터를 검증하고,비지니스 로직을 실행. 뷰에 전달할 결과 데이터를 모델에 담아 줌.

## MVC 패턴의 한계

- Controller가 가지는 공통된 부분을 매번 코드를 작성하고 호출하는 것은 비효율적
- 공통된 부분은 한번에 처리해주는 역할이 있으면 좋음
- 이를 위해 스프링의 MVC는 Dispatcher Servlet을 사용함

---

# 서블릿 파헤치기

- 어떠한 불편함 때문에 스프링 프레임워크가 나왔을까? 여기에 관점을 두고 서블릿에 대해 알아본다.

## Servlet Request

- Request-Header
  - HTTP 요청이 들어올 경우 어떻게 해당 요청이 파싱되어 최종적으로 개발자가 이용 할 수 있는지 보자.
  - 헤더와 바디로 나뉨
  - 헤더의 경우, 요청이 어떤 동작을 취하는지 또 어떤 곳으로 보내지는지 등 해당 리퀘스트에 대한 정보를 가짐
  - 바디 부분은, 클라이언트가 서버에게 보내는 다양한 데이터들이 담겨 있음.
  - Request 요청이 구동한 WAS에 도달하면 서블릿에서 해당 메세지를 파싱하여 HttpServletRequest를 통해 request 객체를 만들어 줌
  - 도착한 request의 다양한 정보들을 활용하여 로직을 구현
- Request-Body

  - GET
    - HTTP 요청의 바디에 아무것도 넣지않고 데이터를 전송하는 방식
  - POST
    - 데이터 전달
    - 서버에서는 쿼리 파라미터 조회 메서드를 사용해도 됨
    - request.getParameter()
  - HTTP Body에 직접 데이터 넣기

    - Rest API라는 구조를 채택할 경우 사용
    - 주로 DTO 형식으로 클라이언트단에서 필요한 정보를 담아 서버에서 비지니스 로직을 처리.
      - DTO : 계층 간 데이터 교환을 하기 위해 사용하는 객체. 로직을 가지지 않기에 순순한 데이터 객체이다.

    ```
        @Data
        public class HelloData {
            private String username;
            private int age;
        }

        @WebServlet(name = "requestBodyJsonServlet", urlPatterns = "/request-body-json")
        public class RequestBodyJsonServlet extends HttpServlet {

            //JSON DATA를 Object로 바꿔주는 객체 from Jackson Library
            //Springboot에는 자체 내장 되어있음
            private ObjectMapper objectMapper = new ObjectMapper();

            @Override
            protected void service(HttpServletRequest request, HttpServletResponse response) throws             ServletException, IOException {
                ServletInputStream inputStream = request.getInputStream();

                String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

                System.out.println("messageBody = " + messageBody);
                HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);

                System.out.println("helloData.toString() = " + helloData.toString());

                response.getWriter().write("OK");
            }
        }
    ```

    - ServletInputStream의 경우 들어온 메세지 바디의 데이터를 바이트단위로 변형해서 전달받는 역할.
    - objectMapper의 경우, 들어온 데이터를 Json형태로 변경시켜주는 라이브러리의 객체
    - messagebody의 경우 string 형태로 존재하기에 해당 객체를 지정한 HelloData 클래스의 Json 형식으로 변경하기 위한 도구
    - ObjectMapper의 경우 복잡하거나 가독성이 떨어지는 api응답을 reform하여 클라이언트단으로 넘겨줄때 사용

## Servlet Response

- Response-Header
  - Request와 비슷한 구조
  - request 요청이 들어올때 생성된 HttpServletResponse 객체를 사용
  - 응답의 상태코드 및 content를 직접 지정해 줄 수 있음
  - 사용자가 만든 임의의 헤더를 response 헤더내에 위치시킬수도 있음
  - 캐시에 대한 사용여부 또한 직접 조정가능
- Response-Body
  - 텍스트/HTML 응답과 Json형식 데이터 응답으로 나뉠 수 있음
  - Json형식의 데이터는 ObjectMapper를 사용.
    - contentType과 encoding 타입을 지정해주고 원하는 데이터 형식의 객체를 생성
    - ObjectMapper를 이용해서 JSON형태인 DTO 데이터를 String으로 response에 넣어주면 됨

## DAO, DTO, VO

- DAO(Data Access Object)
  - 데이터베이스의 data에 접근하기 위한 객체
  - 데이터베이스에 접근하기 위한 로직, 비지니스 로직을 분리하기 위해 사용
- DTO(Data Transfer Object)
  - 계층 간 데이터 교환을 하기 위해 사용하는 객체. 로직을 가지지 않는 순수한 데이터 객체
  - 유저가 입력한 데이터를 DB에 넣는 과정
    - 유저가 자신의 브라우저에서 데이터를 입력하여 form에 있는 데이터를 DTO에 넣어서 전송.
    - 해당 DTO를 받은 서버가 DAO를 사용하여 데이터베이스로 넣음.
- VO(Value Object)
  - 값을 위해 쓰임. read-only 특징을 가짐
  - DTO와 유사하지만 DTO는 setter를 가지고 있어 값이 변할 수 있음

---

# REST

## REST(Representational State Transfer)란?

### REST의 정의

- 자원을 이름으로 구분해 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미
- 즉, 자원의 표현에 의한 상태 전달

  - 자원 : 해당 소프트웨어가 관리하는 모든 것( 문서, 그림, 데이터 등 )
  - 표현 : 그 자원을 표현하기 위한 이름( ex. DB의 학생 정보 : students )
  - 상태 전달 : 데이터가 요청되는 시점에 자원의 상태를 전달( JSON 혹은 XML을 통해 데이터를 주고 받는 것 )

- REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에, 웹의 장점을 최대한 활용 아키텍처
- REST는 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나

### REST의 개념

- 어떤 자원에 대해 CRUD 연산을 수행하기 위하여 URI(Resource)로 GET, POST 등의 방식(Method)을 사용하여 요청을 보냄.
- 요청을 위한 자원은 특정한 형태로 표현
- URL와 URI의 차이점
  - URL은 Uniform Resource Locator로 인터넷 상 자원의 위치
  - URI는 Uniform Resource Identifier로 인터넷 상의 자원을 식별하기 위한 문자열의 구성
  - URI는 URL을 포함하게 됨. URI가 URL보다 포괄적인 범위라고 할 수 있음

### REST의 구성 요소

- 1. 자원(Resource) - URI
  - 모든 자원에는 고유 ID가 존재하고, 이 자원은 Server에 존재한다.
  - 자원을 구별하는 ID는 '/exgroups/:exgroup_id'와 같은 HTTP URI이다.
  - Client는 URI를 이용해 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청
- 2. 행위(Verb) - Method

  - HTTP 프로토콜의 Method 사용.
  - HTTP 프로토콜은 GET, POST, PUT, PATCH, DELETE의 Method를 제공

    | Method | 설명                                                           |
    | :----: | :------------------------------------------------------------- |
    |  GET   | Read : 정보 요청, URI가 가진 정보를 검색하기 위해 서버에 요청  |
    |  POST  | Create : 정보 입력, 클라이언트에서 서버로 전달하려는 정보 보냄 |
    |  PUT   | Update : 정보 업데이트, 내용 갱신, 전체 바꿀 때                |
    | PATCH  | Update : 정보 업데이트, 내용 갱신, 부분 바꿀 때                |
    | DELETE | Delete : 정보 삭제                                             |

- 3. 표현
  - Client와 server가 데이터를 주고받는 형태로 JSON, XML 등이 있음

### REST의 특징

- 1. Server - Client 구조
- 2. Stateless(무상태)
  - HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성
  - Client의 context를 Server에 저장하지 않음
    - 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순
  - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리
    - 각 API 서버는 Client의 요청만을 단순 처리
    - 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안됨
    - Server의 처리 방식에 일관성을 부여하기 때문에 서비스의 자유도가 높아짐
- 3. Cacheable(캐시 처리 기능)
  - 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용
    - 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용
  - 대량의 요청을 효율적으로 처리
- 4. Layered System (계층 구조)
  - Client는 REST API Server만 호출
  - REST Server는 다중 계층으로 구성할 수 있음
    - 보안, 로드 밸런싱, 암호화 등을 위한 계층을 추가하여 구조를 변경
    - Proxy, Gateway와 같은 네트워크 기반의 중간매체를 사용
    - Client는 Server와 직접 통신하는지, 중간 서버와 통신하는지는 알 수 없음
- 5. Uniform Interface (인터페이스 일관성)
  - URI로 지정한 Resource에 대한 요청을 통일되고, 한정적으로 수행하는 아키텍처 스타일
  - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하며, Loosely Coupling(느슨한 결함) 형태
    - 즉, 특정 언어나 기술에 종속되지 않음
- 6. Self-Descriptiveness (자체 표현)
  - 요청 메시지만 보고도 쉽게 이해할 수 있는 자체 표현 구조

### REST API 란?

- 정의
  - REST의 특징을 기반으로 서비스 API를 구현
  - 최근 OpenAPI, 마이크로 서비스 등을 제공하는 기업 대부분은 REST API를 제공
- 특징
  - 각 요청이 어떤 동작이나 정보를 위한 것인지를 그 요청의 모습 자체로 추론이 가능
- 디자인 가이드
  - URI는 정보의 자원을 표현
  - 자원에 대한 행위는 HTTP Method(GET, POST, PUT, PATCH, DELETE)로 표현
- 설계 규칙
  - URI는 명사를 사용
  - 슬래시( / )로 계층 관계를 표현
  - URI 마지막 문자로 슬래시 ( / )를 포함하지 않음
  - 밑줄( \_ )을 사용하지 않고, 하이픈( - )을 사용
  - URI는 소문자로만 구성
  - HTTP 응답 상태 코드 사용
  - 파일확장자는 URI에 포함하지 않음

### REST API와 RESTful API의 차이?

- RESTful은 REST의 설계 규칙을 잘 지켜서 설계된 API를 RESTful한 API라고 함
- REST의 원리를 잘 따르는 시스템을 RESTful이란 용어로 지칭

### REST API 요약

- URI는 정보의 자원만 표현해야 하며, 자원의 행위는 HTTP Method에 명시한다
