# NoSQL

- 대량의 분산된 데이터를 저장하고 조회하는 데 특화되었으며 스키마 없이 사용 가능하거나 느슨한 스키마를 제공하는 저장소를 말한다.

## MongoDB

1. 유연한 스키마
2. 중복 허용 (join 회피)
3. scale-out 에 용이함 (그래서 보통 클러스터를 구성하여 사용한다.)
4. ACID의 일부를 포기하고 높은 처리량과 응답을 추구

## Redis

- in-memory key-value database or cache
- data type: strings, lists, sets, hashes, sorted sets (다양한 형태로 사용 가능)
- hash-based sharded cluster
- High Availability