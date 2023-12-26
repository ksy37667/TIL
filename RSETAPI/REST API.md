# REST API

- 정의 - RESTful한 API를 말하며 일련의 특징과 규칙 등을 지키는 API를 말한다.

## REST API 특징

### 1. Uniform-Interface

- API에서 자원들은 각각의 독립적인 인터페이스를 가지며 각각의 자원들이 url 자원식별, 표현을 통한 자원조작, Self-descriptive messages, HATEOAS  구조를 가지는 것을 말합니다.
- 웹 페이지를 변경했다고 웹 브라우저를 업데이트하는 일은 없어야 하고 HTTP 명세나 HTML 명세가 변경되어도 웹페이지는 동작해야 한다.

1. 자원은 url로 식별되어야 한다.
2. 표현을 통한 자원조작
    - url과 HTTP Method 를 통해 자원을 조회, 삭제 등 작업을 설명할 수 있는 정보가 담겨야 한다.
3. self-descriptive messages
    - HTTP Header에 타입을 명시하고 자원들은 MIME types에 맞춰 표현되어야 합니다. 예를 들어 .json를 반환한다면 application/json으로 명시해야 한다.

1. HATEOAS 구조
    - 하이퍼링크에 따라 다른 페이지를 보여줘야 하며 어떤 URL에서 원했는지 명시해주어야 하는 것을 말한다. 보통은 href, links, link, url 속성 중 하나에 해당하는 데이터의 URL을 담아서 표기해야 합니다.

### 2. Stateless

- HTTP 자체가 Stateless이기 떄문에 HTTP를 이용하는 것만으로도 만족된다. 그리고 이는 REST API를 제공해주는 서버는 세션을 해당 서버 쪽에 유지하지 않는다는 의미이다.

### 3. Casheable

- HTTP는 아무런 로직을 구현하지 않아도 자동적으로 캐싱이 된다. 이는 GET 메서드에 한정된다.

### 4. Client-Server 구조

- 클라이언트와 서버가 서로 독립적인 구조를 가져야 한다. 서버는 API를 제공하고 클라이언트는 HTTP로 받는 로직만 잘 처리하면 된다.

### 5. Layered System

- 계층구조로 나눠져 있는 아키텍처를 의미한다.

## REST API의 URI규칙

1. 동작은 HTTP 메소드로만 해야 하고 url에 내용이 들어가면 안된다.
2. .jpg, png 같은 확장자 표현x
3. 동사가 아닌 명사로만 표기해야 한다.
4. 계층적인 내용으 담고 있어야 한다. ‘집/아파트/전세’ 
5. 언더바가 아닌 그냥 -를 쓴다
6. http응답 상태코드를 잘 활용해야 한다.