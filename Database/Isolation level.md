# Isolation level

- 데이터 처리량 vs 데이터 일관성의 trade off
- 격리수준(isolation level)이란 트랜잭션끼리 **얼마나 서로 고립되어 있는지를 나타내는 수준**입니다.
- 즉, 한 트랜잭션이 **다른 트랜잭션이 변경한 데이터**에 대한 접근 강도를 의미합니다.
- 레벨이 높아질수록 트랜잭션간 고립정도가 높아지며, 성능저하도 야기됩니다.

### dirty read

- commit 되지 않은 변화를 읽는것

### Non-repeatable read (fuzzy read)

- 같은 데이터를 하나의 트랜잭션 안에서 여러번 읽었을 때 값이 달라짐

### Phantom read

- 읽어온 결과의 행이 새로 생기거나 없어지는 현상이다.

| Isolation level | Dirty read | Non-repeatable read | Phantom read |
| --- | --- | --- | --- |
| Read uncommiitted | o | o | o |
| Read committed | x | o | o |
| Repeatable read | x | x | o |
| serializable | x | x | x |