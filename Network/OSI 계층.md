# OSI 계층

## OSI model (7 layer)

- 범용적인 네크워크 구조

1. Application layer
    - 애플리케이션 목적에 맞는 통신 방법 제공 (http, dns, smtp, ftp)
    
2. presentation layer
    - 애플리케이션 간의 통신에서 메시지 포맷 관리 (디코딩, 압축풀기 등)

1. session layer
    - 애플리케이션 간의 통신에서 세션을 관리 (RPC)

1. transport layer
    - 애플리케이션 간의 통신 담당
    - 목적지 애플리케이션으로 데이터 전송
    - 안정적이고 신뢰할 수 있는 데이터 전송 보장 (TCP)
    - 필수 기능만 제공 (UDP)

1. Network layer
    - 호스트 간의 통신 담당 (IP)
    - 목적지 호스트로 데이터 전송
    - 네트워크 간의 최적의 경로 결정

1. Data link layer
    - 직접 연결된 노드 간의 통신 담당
    - MAC 주소 기반 통신 (ARP로 IP에서 MAC 주소로 변환)

1. Physical layer
    - bits 단위로 데이터 전송