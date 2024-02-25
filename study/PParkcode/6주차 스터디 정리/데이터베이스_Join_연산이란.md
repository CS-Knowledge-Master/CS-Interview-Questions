# 데이터베이스 - Join연산


## Join 연산

- `Join` 연산은 관계형 데이터베이스에서 2개 이상의 테이블 간에 데이터를 결합하는 연산이다.

- 이를 통해 데이터베이스에서 복수의 테이블에서 가져온 데이터를 합쳐 하나의 결과로 반환할 수 있다.

- `Join`은 데이터의 무결성을 유지하고 효율적으로 데이터를 가져오는데 사용된다.

- Join 연산의 종류에는 `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`이 있다.


## INNER JOIN

- `INNER JOIN`은 두 테이블 간에 공통된 값이 있는 경우네만 결과를 포함 시킨다.

- 테이블 조인에 대한 조건은 `ON`, 데이터 필터링에 대한 조건은 `WHERE`을 사용한다.

```sql
SELECT * FROM table1
INNER JOIN table2 ON table1.column = table2.column;
```


## OUTER JOIN

- 외부 조인은 공통 데이터 외에 어느 한 테이블에만 존재한는 데이터도 함께 결합하여 조회하고자 할 때 사용한다.

- `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`이 있고, 데이터가 한 쪽에만 있을 때 어느 테이블을 기준으로 삼을지에 따라 나뉜다.

- 내부 조인은 공통된 컬럼을 기주으로 테이블을 묶기 때문에 순서가 상관없었지만, 외부 조인에서는 순서가 중요하다.

- 어떤 테이블을 먼저 접근하냐에 따라 쿼리 성능에 영향을 미치므로, 더 적은 데이터를 추출하는 테이블을 드라이빙 테이블로 삼는 것이 좋다.

![](https://velog.velcdn.com/images/newdana01/post/ff2ac35d-0ee9-494b-b5f0-f4f070c4197e/image.png)


## LEFT JOIN

- 왼쪽 테이블을 기준으로 조인을 한다.

- 즉, 왼쪽 테이블의 모든 행을 결과에 포함시키고, 오른쪽 테이블에서는 조인 조건을 만족하는 행이 있으면 해당 행을, 없다면 `NULL` 값을 포함시킨다.

```sql
SELECT * FROM table1
LEFT JOIN table2 ON table1.column = table2.column;
```


## RIGHT JOIN

- `RIGHT JOIN`은 반대로 오른쪽 테이블을 기준으로 조인을 한다.

- 즉, 오른쪽 테이블의 모든 행을 결과에 포함시키고, 왼족 테이블에서는 조인 조건을 만족하는 행이 있으면 해당 행을, 없다면 `NULL` 값을 포함시킨다.

```sql
SELECT * FROM table1
RIGHT JOIN table2 ON table1.column = table2.column;
```



## FULL JOIN

- `FULL JOIN`은 두 테이블 중 어느 한쪽에든 조건을 만족하는 행을 결과에 포함시킨다.

- 양쪽 테이블에서 매칭되는 행이 없을 경우 `NULL` 값을 포함시킨다.

```sql
SELECT * FROM table1
FULL JOIN table2 ON table1.column = table2.column;
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbWiaHo%2FbtrknXYcRbO%2FKFG8M40GKJKpsVJPz4D141%2Fimg.png)