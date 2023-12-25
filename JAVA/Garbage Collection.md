# Garbage Collection

- 어플리케이션이 동적으로 할당했던 메모리 영역 중 더이상 사용하지 않는 영역을 정리하는 기능
- Heap 메모리에서 활용하며, JVM에서 GC의 스케줄링을 담당하며 개발자가 직접 관여하지 않아도 더이상 사용하지 않는 점유된 메모리를 제거해주는 역할

### STOP THE WORLD

- GC를 구행하기 위해 JVM이 멈추는 현상을 의미
- GC가 작동하는 동안 GC관련 Thread를 제외한 모든 Thread는 멈춤

![Untitled](Garbage%20Collection%2066b85441a34543d592b7a3c914cc3c1b/Untitled.png)

### Young 영역

- 새롭게 생성된 객체가 할당되는 영역
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션을 Minor GC라고 부른다.

### Old 영역

- Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
- Young 영역보다 크게 할당
- Old 영역에 대한 가비지 컬렉션을 Major GC 또는 Full GC라고 부른다.

Old 영역이 더 크게 할당되는 이유는 Young 영역은 수명이 짧은 객체들이 할당되기 때문에 큰 공간이 필요하지 않음

---

### Eden

- new를 통해 새로 생성된 객체가 위치.
- 살아남은 객체들은 Survivor 영역으로 보냄

### Survivor 0 / Survivor 1

- 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역

# GC 알고리즘

## 1. Reference Counting Algorithm

- 각 객체마다 Reference Count를 관리하며, 이 카운트가 0이 되면 GC를 수행
- 카운트가 0이 되면 메모리에서 제거
- 순환 참조 구조에서 카운팅이 0이 되지 않는 문제가 발생하며 Memory Leak이 발생할 수 있음

## 2. Mark-and-Sweep Algorithm

- 1번의 단점을 극복하기 위해 나옴
- root가 참조하고 있는 메모리를 순차적으로 접근해서 마크한 후 아무에게도 접근되지 않는 메모리는 마크되어 있지 않아 삭제된다.