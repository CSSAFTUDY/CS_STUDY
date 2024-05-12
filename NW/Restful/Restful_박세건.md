#REST
>
Rest 정의 :  분산 하이퍼미디엇 시스템을 위한 소프트웨어 아키텍처의 한 형식
_아키텍처 : 구조의 설계
즉, 네트워크에서 **Client와 Server** 사이의 **통신 방식**(규칙)
- HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용
- 자원의 **이름으로 구분**하여 자원의 상태를 주고받는 것을 의미

## REST 구성요소
- **자원**을 나타내는 URL
  - URL을 통해서 자원을 나타냄
  - 고유한 이름을 갖음
  ex) member/1 : ID가 1 인 학생을 의미
- **행위**를 나타내는 Method
  - URL에 의해 정해진 자원을 **어떤 방식으로 가공할지**를 나타내는 Method
  - HTTP Method를 사용
  ex) get, post, put ...
- **표현**
  - 서버가 보내주는 **응답 자원의 상태를 의미**
  ex) JSON, XML 등
  
## REST를 사용하는 이유
- 이해하기 쉬운 API를 제작 가능
  - 일관적이기에 다른 사람이 보고 이해하기가 쉬움
- **정해진 컨벤션으로 API를 쉽게 이해하고 호환성을 높이는 것**이 목적

## REST 특징
> REST는 HTTP를 사용하기 때문에 HTTP의 장점을 소유할 수 있음

- Uniform(통일화)
  - URI로 지정된 **요청이 통일**되고 지정된 인터페이스로만 수행 가능
- **Stateless**
  - 요청에 대한 상태정보를 따로 저장하지 않는다
  - 들어오는 요청만 처리
- **Cacheable**
  - HTTP의 캐시 기능 적용 가능
  >HTTP 캐시 : HTTP응답을 저장해놓고 이전과 동일한 요청이 들어오면 응답 재활용
- Self-Descriptiveness(자기 기술성)
  - 클라이언트가 API 메시지를 보고 명확하게 이해 가능
- Client-Server
  - 클라이언트와 서버의 역할이 확실하게 구분
  - 의존성이 줄어듬
- 계층형 구조
  - REST 서버는 다중 계층으로 구성될 수 있음
  
  
# RESTful
> RESTful, REST 원리를 따르는 시스템

## RESTful 특징
- 확장성과 재사용성이 높아 유지보수 및 운영이 편리
- HTTP를 기반으로 하기때문에 HTTP를 지원하는 프로그래밍언어를 사용해서 구축가능

## RESTful 규칙
ex) (GET 방식) /book/{id} : id번째 book의 정보를 알려줘
ex) (PUT 방식) /book/{id} : id번째 book의 정보를 수정할게
ex) (DELTE 방식) /book/{id} : id번째 book의 정보를 삭제할게
>
[RESTful 규칙](https://dev-cool.tistory.com/32)을 확인해보자

## HTTP Method
- GET : 조회
- POST : 추가
- PUT : 수정
- DELETE : 삭제
- PATCH : 부분적 수정
  - PUT 방식과 차이
  > PUT 방식으로 일부분을 보내면 나머지는 default 값으로 수정되기 때문에 주의

_POST를 제외한 Method 모두 멱등성을 띈다_
> **멱등성** : 데이터가 변경되지 않는 한, 동일한 호출에 동일한 결과를 얻음
