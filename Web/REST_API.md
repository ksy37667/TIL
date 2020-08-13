# REST API란?
* REST 아키텍쳐(분산 하이퍼미디어 시스템(웹 등)을 위한 아키텍쳐 스타일)의 스타일을 따르는 API
* 아키텍쳐 스타일(제약조건)을 지키는 것

# REST 를 구성하는 스타일
* client-server
* stateless
* caching
* uniform interface
* layered system
* code-on-demand (optional) (예를 들면 javascript)


# client-server
* 서버와 클라이언트 각각의 영역에서 각자의 역할이 확실하게 구분되는 것
* 서버는 API를 제공하고 클라이언트는 사용자와 관련된 것들을 처리하는 역할을 한다.


# stateless
* RESTful 서비스는 서버에서 클라이언트의 상태를 저장해서는 안된다.
* 즉, 서버는 들어온 요청에 대해서만 알맞게 처리하면 되기 때문에 구현이 단순해진다.


# caching
* 클라이언트 자체세 서버의 응답을 저장하는 것으로, 클라이언트가 동일한 리소스에 대해 서버에 요청을 반복 할 필요가 없다.
    * 이미지, css, 자바스크립트와 같은 정적 컨텐츠는 cacheable한 상태로 2~3일간 유지해라.
    * 만료기간을 너무 길게 유지하지 마라.
    * 동적 컨텐츠는 몇시간동안만 유지되야한다.

# uniform interface
* Identification of resources : URI 표준을 사용하여 리소스를 식별합니다.

* Manipulation of resources through these representations : HTTP 표준을 사용하여 메세지에 표현하여 통신한다.
    * 예를들어 GET은 데이터를 URI 식별 리소스에 대한 데이터를 검색. 즉, HTTP메서드와 URI를 사용하여 작업을 설명 할 수 있다.

* Self-descriptive messages : (공부 필요)

* Hypermedia as the engine of application state (HATEOAS) : (공부 필요)


# layered system
* 서버는 다중 계층으로 구성될 수 있으며 특정 계층(로드 밸런싱, 암호화 등)을 추가하여 구조를 변경할 수 있다. 또한 네트워크 기반의 중간매체(Proxy, Gateway)를 사용할 수 있게 해준다. 하지만 클라이언트는 서버와 직접 통신을 하는지, 중간 서버와 통신하는지 알 수 없다.





# reference
* https://stackoverflow.com/questions/25172600/rest-what-exactly-is-meant-by-uniform-interface
* https://www.youtube.com/watch?v=RP_f5dMoHFc&t=678s