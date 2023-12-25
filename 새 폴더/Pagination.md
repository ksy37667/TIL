# Pagination

## Offset vs Cursor

### 1. Offset

```sql
// 0번째부터 바로 100개를 select
select * from orders where category = 'BOOK' limit 0, 100 

// 50000000번째 데이터를 찾은 후에 100개를 select
select * from orders where category = 'BOOK' limit 50000000, 100 
```

페이징 처리에 Offset을 사용하게 되면 뒤로 갈 수록 속도가 저하된다.

### 2. Cursor

```sql

select * from orders where category = 'BOOK' and id > 0 limit 0, 100

...
...

select * from orders where category = 'BOOK' and id > 967678332 limit 0, 100
```

Cursor 방식은 where절에서 전 Page의 마지막 아이디 값을 이용해 검색 후 limit offset을 사용하기 때문에 속도가 더 빠르다.

**`JdbcCursorItemReader`**

**`HibernateCursorItemReader`**

를 사용해서 Spring에서 CursorItemReader를 구현할 수 있다.