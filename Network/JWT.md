# JWT

- Json 포맷을 이용하여 사용자에 대한 속성을 저장하고 인증 수단으로 사용 할 수 있는 Claim 기반의 Web Token이다.

## Header

- 헤더는 토큰의 타입이나 서명 생성에 어떤 알고리즘이 사용되었는지 저장한다.

## payload

- Claim이라는 사용자에 대한 정보를 key-value 형태로 저장한다.
- payload에는 민감한 정보를 담으면 안되고 식별하기 위한 최소한의 정보만을 담아야 한다. (이메일, 만료시간 등)

## Signature

- header를 디코딩한 값, payload를 디코딩한 값을 합쳐서 서버가 가지고 있는 개인키를 가지고 암호화되어있는 상태
- 서버에 있는 개인키로만 복호화 할 수 있다.
- 만약 클라이언트가 payload에 담긴 식별자가 변조된 JWT로 요청을 하더라도 서버가 발급했던 Signature 안의 payload와 다르기 떄문에 인증이 불가능

## 장점

- 토큰 자체가 인증된 정보이기 떄문에 세션 저장소와 같은 별도의 인증 저장소가 필수는 아니다
- 클라이언트의 상태를 서버가 저장해 두지 않아도 된다.
- signature를 개인키 암호화를 통해 막아두었기 떄문에 데이터에 대한 보안

## 단점

- 더 많은 필드가 추가되면 토큰이 커질 수 있다.
- 데이터 트래픽 크기에 영향을 미칠 수 있다.
- 토큰은 클라이언트에 저장되어 있기 떄문에 데이터베이스에서 사용자가 정보를 조작하더라도 토큰에 적용할 수 없다.
- 탈취됐을 경우 대처가 어렵다 (그래서 access token의 만료시간을 짧게 두고 refresh token을 같이 사용하는 방법이 있다.)