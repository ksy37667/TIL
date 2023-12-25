# Hash Table

- key, value를 빠르게 저장하고 읽을 수 있는 자료구조
- Hash Function과 Table(배열)로 구성
    - **Hash Function** - 임의의 길이를 갖는 임의의 데이터를 고정된 길이의 데이터로 매핑하는 단방향 함수

### Hash Funtion의 문제점

- 2개 이상의 다른 input값을 Hash Funtion에 적용했는데 같은 결과값이 나올 수도 있다.

### Hash Funtion의 문제점 해결

1. Chianing with Linked List
    - 배열을 쓰는게 아니라 Linked List 배열을 만들어서 Hash Collision 이 생기면 그 Index에 있는 Linked List 에 데이터 추가
    - Chaning with Binary Tree 를 사용할 수도 있다. (Linked list보다 더 빨리 찾을 수 있음)
    
2. Probing
    1. Linear Probing - 이미 값이 있으면 다음 자리에 추가하기
    2. Quadratic Probing - 다음 자리를 찾을 때 바로 다음 자리가 아닌 i^2 후에 위치한 자리에 추가하기
    

### 시간복잡도

- Hash Collision이 없다고 가정하면
    - 데이터 추가 - O(1)
    - 데이터 읽기 - O(1)

- Hash Collision이 항상 일어난다면 - O(n)이 될 수도 있다.

### 공간 복잡도

- O(n)