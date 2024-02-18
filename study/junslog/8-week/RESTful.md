# [ RESTful이란 무엇이며, 이것에 대해 아는대로 설명해주세요 ]

## REST ( Representational State Transfer )

### REST - 정의

- 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템에서 **정보를 교환**하기 위한
  **소프트웨어 아키텍처**
- 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나이다.
- **자원을 이름(자원의 표현)으로 구분**하여,
  해당 **자원의 상태(정보)**를 주고 받는 모든 것을 의미한다.
- 자원(Resource)의 표현(Representation)에 의한 상태 전달

1. **자원의 표현 ( Resource Representation )**
    - 자원 : 해당 SW가 관리하는 모든 것

      ex) 문서, 그림, 데이터, 해당 소프트웨어 자체 등

    - 자원의 표현 : 해당 자원을 표현하기 위한 이름

      ex) DB의 학생 정보가 자원일 때, `**‘students’**`를 자원의 표현으로 정한다.

2. **상태(정보) 전달 ( State Transfer )**
    - 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달
    - JSON 혹은 XML을 통해 데이터를 주고받는다.

### REST - 구체적인 개념

- HTTP URI ( Uniform Resource Identifier )를 통해 자원(Resource)을 식별, 명시하고
  HTTP Method ( POST, GET, PUT, DELETE )를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것

- 자원 기반의 구조(ROA, Resource Oriented Architecture)이다.
  설계의 중심에 Resource가 있고, HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍처

- 웹 사이트의 이미지, 텍스트, DB 내용 등 모든 자원에 고유한 ID인 HTTP URI를 부여한다.

- 고유한 자원들에 대해 HTTP Method를 통해 CRUD Operation을 적용한다.
  POST / GET / PUT / PATCH / DELETE / HEAD 메서드 등이 있다.

### REST - 장단점

[ 장점 ]

- HTTP Protocol의 인프라를 그대로 사용하기에,
  REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
- HTTP Protocol의 표준을 최대한 활용하여,
  여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
- REST API 메시지 자체가 의도하는 바를 명확하게 나타내서,
  의도하는 바를 쉽게 파악할 수 있다.
- 서버와 클라이언트의 역할을 명확히 분리한다.

[ 단점 ]

- 표준이 존재하지 않는다.
- HTTP Method 형태가 제한적이다.

### REST의 필요성

- 어플리케이션 분리 및 통합
- 다양한 클라이언트 등장에 따라 통신 규약의 통일 ( 멀티 플랫폼에 대한 지원 )
- 서비스 자원에 대한 아키텍처를 세우고, 해당 자원의 상태에 관한 요청을 통해
  어플리케이션 운영

### REST 구성요소

- 자원(Resource) : URI
    - 모든 자원에 고유한 ID가 존재, 이 자원은 Server에 존재함.
    - 자원을 구별하는 ID는 HTTP URI 이다.
    - Client는 URI를 이용해서 자원을 지정하고, 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
- 행위(Verb) : HTTP Method
    - HTTP Protocol의 Method를 사용한다.
    - HTTP Protocol은 GET, POST, PUT, DELETE와 같은 메서드를 제공함.
- 표현(Representation of Resource)
    - 클라이언트가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
    - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
    - JSON or XML을 통해 데이터를 주고 받는 것이 일반적

### REST의 특징

**1) Server-Client (서버-클라이언트 구조)**

- 자원이 있는 쪽이 Sever, 자원을 요청하는 쪽이 Client이다.
    - REST Server : API를 제공하고, 비즈니스 로직 처리 및 저장을 책임진다.
    - Client : 사용자 인증이나 Context( 세션, 로그인 정보 )등을 직접 관리하고 책임진다.
- 이 구조를 통해 서로 간 의존성이 줄어든다.

**2) Stateless (무상태)**

- HTTP Protocol은 Stateless Protocol 이므로, REST 역시 무상태성을 갖는다.
- Client의 Context를 Server에 저장하지 않는다.
    - 즉, 세션과 쿠키와 같은 Context 정보를 신경쓰지 않아도 되어, 구현이 단순해진다.
- Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
    - 각 API 서버는 Client의 요청만을 단순 처리한다.
    - 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
    - 이전 요청이 DB를 수정하여, DB에 의해 요청의 처리가 바뀌는 것은 허용한다.
    - Server의 처리 방식에 일관성을 부여하여 개발 부담이 줄고, 서비스의 자유도가 높아진다.

3) Cacheable (캐시 처리 가능)

- 웹 표준 HTTP Protocol을 그대로 사용하여, 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
    - HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
    - HTTP Protocol 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
- 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
- 캐시 사용을 통해 응답시간이 빨라지고, REST Server 트랜잭션이 발생하지 않아
  전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있음.

4) Layered System (계층화)

- Client는 REST API Server만 호출
- REST Server는 다중 계층으로 구성 가능
    - API Server는 순수 비즈니스 로직을 수행,
      그 앞단에 **보안, 로드밸런싱, 암호화, 사용자 인증** 등을 추가하여
      구조상의 유연성을 줄 수 있음.
    - 또한, 로드밸런싱, 공유 캐시 등을 통해 확장성 및 보안성을 향상시킬 수 있음.
- Proxy, Gateway와 같은 네트워크 기반의 중간 매체를 사용할 수 있다.

5) Code-On-Demand

- Server로부터 스크립트를 받아서 Client에서 실행
- 반드시 충족할 필요는 없음 (Optional)

6) Uniform Interface ( 인터페이스 일관성 )

- URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행
- HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능
    - 특정 언어나 기술에 종속되지 않음

### REST API - 정의

- API ( Application Programming Interface )
    - 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진,
      서로 정보를 교환가능 하도록 하는 것

- REST API의 정의
    - REST 기반으로 서비스 API를 구현한 것
    - OpenAPI, MicroService 에서 대부분 REST API를 제공한다.

### RESST API - 특징

- 사내 시스템을 REST 기반으로 시스템을 분산해,
  확장성과 재사용성을 높여서 유지보수 및 운용을 편리하게 할 수 있다.

- REST는 HTTP 표준을 기반으로 구현하므로,
  HTTP를 지원하는 프로그램 언어라면 클라이언트, 서버를 구현할 수 있다.

- 즉, 서버에서 REST API를 제작하면
  다양한 클라이언트 ( 자바, C#, 웹, AOS, IOS .. )등을 이용해 클라이언트를 제작할 수 있다.

### REST API - 설계 규칙 ( 컨벤션 )

1. URI는 정보의 자원을 표현해야 한다.
    1. resource는 동사보다는 명사를, 대문자보다는 소문자를 사용
    2. resource의 도큐먼트 ( 객체 인스턴스, DB 레코드와 비슷한 개념 ) 이름으로는 단수 명사를 사용한다.
    3. resource의 컬렉션 ( 서버에서 관리하는 디렉터리와 같은 리소스 ) 이름으로는 복수 명사를 사용해야 한다.
    4. resource의 스토어 ( 클라이언트에서 관리하는 리소스 저장소 ) 일므으로는 복수 명사를 사용해야 한다.

       ex) GET /Member/1 (x) → GET /members/1

2. 자원에 대한 행위는 HTTP Method로 표현한다.
    1. URI에 HTTP Method가 들어가면 안된다.
    2. URI에 행위에 대한 동사 표현이 들어가면 안된다. ( CRUD 기능을 나타내는 용어는 URI에 사용하지 않는다 )
    3. 경로 부분 중 변하는 부분은 유일한 값으로 대체한다.

3. 슬래시 구분자(/) 는 계층 관계를 나타내는데 사용한다.

1. URI 마지막 문자로 슬래시(/)를 포함하지 않는다. ( 혼동울 주지 않기 위함 )

1. 하이픈 ( - )은 URI 가독성을 높이는데 사용한다.

1. 언더바 ( _ )는 URI에 사용하지 않는다.

1. URI 경로에는 소문자가 적합하다.

1. 파일확장자는 URI에 포함하지 않는다.
    - 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
    - HTTP Header의 Accept 를 사용한다.
    - ex) http://restapi.example.com/members/soccer/345/photo.jpg (X)
      ex) GET / members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (O)

2. 리소스 안에 연관 관계가 있는 경우 나타내는 방법
- /리소스명/리소스 ID/관계가 있는 다른 리소스명
    - ex) GET : /users/{userId}/devices ( has 관계 )

### RESTful - 개념

- RESTful은 일반적으로 **REST 아키텍처를 구현하는 웹 서비스**를 나타내기 위해 사용되는 용어
    - ‘REST API’를 제공하는 웹 서비스를 ‘RESTful’ 하다 할 수 있다.
- RESTful은 REST를 REST 답게 쓰기 위한 방법, 공식적인 발표나 표준이 있는 것은 아니다.
    - REST의 원리를 따르는 시스템을 RESTful이란 용어로 지칭한다.

### RESTful - 목적

- 이해하기 쉽고, 사용하기 쉬운 REST API를 만드는 것
- 성능 향상에 목적이 있다기 보단,
  **일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것**
- 성능이 1순위인 상황에서는 굳이 REST 아키텍처를 따르지 않아도 될 수 있다.