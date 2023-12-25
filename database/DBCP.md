# DBCP

- DB Connection Pool 이란?웹 컨테이너(WAS)가 실행되면서 DB와 미리 연결해놓은 객체들을 Pool에 저장해두었다가 클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 다시 connection을 반납받아 pool에 저장하는 방식

- 어플리케이션이 DB를 사용하기 위해서는 Connection이 필요한데, 이 생성 및 소멸 비용이 크기 때문에, 커넥션 풀을 미리 생성하고 애플리케이션이 시작하는 시점에 커넥션을 미리 다 생성하고 이것을 재활용하며 사용하게 됩니다.


<br><br>
## DBCP 설정 (HikariCP)

### minimumidle

- pool에서 유지하는 최소한의 idle connection 수

### maximumPoolSize

- pool이 가질 수 있는 최대 connection 수
- idle과 active connetion 합쳐서 최대 수

※ **minimumidle 의 값과 maximumPoolSize의 값을 동일하게 설정하는 것을 권장한다.**


<br><br>

## maxLifetime

- pool엣더 connection의 최대 수명
- maxLifetime을 넘기면 idle일 경우 pool에서 바로 제거, active인 경우 pool로 반환된 후 제거
- DB의 connection time limit(wait time)보다 약간 짧게 설정해야 한다.

### connectdionTimeout
- pool에서 connection을 받기 위한 대기 시간

<br><br>

## Database 설정

### max_connections

- client와 맺을 수 있는 최대 connection 수

### wait_timeout

- connection이 inactive 할 때 다시 요청이 오기까지 얼마의 시간을 기다린 뒤에 close 할 것인지를 결정
- 만약 시간 내에 요청이 들어오면 0으로 초기화 후 다시 시간이 카운팅된다.