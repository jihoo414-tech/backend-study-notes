---
title: SQL SELECT 표현식과 정렬 조건
category: database
type: concept
status: reviewing
created: 2026-08-10
updated: 2026-08-10
sources:
  - "[[2026-08-10 SQL SELECT 연습 메모]]"
  - "https://dev.mysql.com/doc/refman/8.4/en/numeric-functions.html"
  - "https://dev.mysql.com/doc/refman/8.4/en/select.html"
  - "https://dev.mysql.com/doc/refman/8.4/en/expressions.html"
  - "https://dev.mysql.com/doc/refman/8.4/en/flow-control-functions.html"
related: []
tags:
  - backend
  - database
  - sql
  - select
verification: partial
---

# SQL SELECT 표현식과 정렬 조건

## 한눈에 보기

`SELECT` 문제에서 자주 사용하는 네 가지 문법을 정리한다.

| 목적 | 문법 |
| --- | --- |
| 숫자 반올림 | `ROUND(값, 자릿수)` |
| 여러 기준으로 정렬 | `ORDER BY 컬럼1, 컬럼2` |
| 양 끝을 포함하는 범위 검색 | `BETWEEN 시작값 AND 끝값` |
| 조건에 따라 결과값 지정 | `CASE ... WHEN ... THEN ... ELSE ... END` |

수업 메모의 `rountd()`는 오타이며 올바른 함수 이름은 `ROUND()`다.

## 왜 필요한가

이 문법들은 조회 결과를 원하는 형태로 가공하고, 필요한 행만 선택하며, 일정한 순서로 출력하는 데 사용한다. 코딩테스트에서는 정답 행을 찾는 것뿐 아니라 출력값의 자릿수, 정렬 우선순위와 범위 경계를 정확히 맞춰야 한다.

## 핵심 개념

### `ROUND()`로 반올림하기

```sql
SELECT ROUND(price) AS rounded_price
FROM products;
```

두 번째 인자는 남길 소수 자릿수를 뜻한다.

```sql
SELECT ROUND(12.345, 2);  -- 12.35
SELECT ROUND(126, -1);    -- 130
```

- `ROUND(number)`: 정수 자릿수까지 반올림
- `ROUND(number, 2)`: 소수 둘째 자리까지 남김
- `ROUND(number, -1)`: 일의 자리에서 반올림하여 십의 자리까지 남김

MySQL에서는 정확한 수와 부동소수점 근삿값의 반올림 결과가 경계값에서 달라질 수 있다. 코딩테스트에서는 컬럼 자료형과 요구 자릿수를 함께 확인한다.

### `ORDER BY`로 두 컬럼 정렬하기

```sql
SELECT name, price, id
FROM products
ORDER BY price DESC, id ASC;
```

정렬 조건은 왼쪽부터 우선 적용된다.

1. 먼저 `price`가 큰 행부터 정렬한다.
2. `price`가 같은 행끼리만 `id`가 작은 순서로 정렬한다.

`ASC`는 오름차순이며 생략할 수 있고, `DESC`는 내림차순이다. 각 컬럼에 방향을 따로 지정할 수 있다.

```sql
ORDER BY category ASC, price DESC;
```

### `BETWEEN`으로 범위 정하기

```sql
SELECT *
FROM products
WHERE price BETWEEN 10000 AND 20000;
```

`BETWEEN`은 시작값과 끝값을 **모두 포함**한다. 위 조건은 다음과 같다.

```sql
WHERE price >= 10000
  AND price <= 20000
```

날짜·시간 컬럼에서는 끝 경계를 특히 주의한다.

```sql
WHERE created_at >= '2026-08-01'
  AND created_at <  '2026-09-01'
```

시간까지 저장된 컬럼에서 8월 전체를 조회할 때는 위처럼 다음 달 시작 시각 미만으로 표현하면 8월 31일의 시간값을 빠뜨리는 실수를 줄일 수 있다.

### `CASE`로 조건별 값 지정하기

값을 직접 비교할 때는 단순 `CASE`를 사용할 수 있다.

```sql
SELECT name,
       CASE status
           WHEN 'Y' THEN '판매 중'
           WHEN 'N' THEN '판매 종료'
           ELSE '상태 미정'
       END AS status_name
FROM products;
```

범위나 복합 조건을 검사할 때는 검색 `CASE`를 사용한다.

```sql
SELECT name,
       CASE
           WHEN price >= 100000 THEN '고가'
           WHEN price >= 50000  THEN '중가'
           ELSE '저가'
       END AS price_group
FROM products;
```

검색 `CASE`는 위에서부터 조건을 검사하며, 처음 참이 된 `THEN` 값만 반환한다. 따라서 넓은 조건보다 구체적이거나 높은 경계의 조건을 먼저 배치해야 한다.

## 동작 원리

```text
FROM에서 대상 테이블 결정
→ WHERE와 BETWEEN으로 행 필터링
→ SELECT의 ROUND·CASE로 출력값 계산
→ ORDER BY의 첫 번째 기준부터 결과 정렬
→ 앞 기준이 같을 때 다음 정렬 기준 적용
```

이는 SQL을 이해하기 위한 논리적 흐름이며, DBMS의 실제 물리적 실행 순서는 옵티마이저가 실행 계획에 따라 정한다.

## 예제

```sql
SELECT product_name,
       ROUND(price, -2) AS rounded_price,
       CASE
           WHEN price BETWEEN 0 AND 9999 THEN '저가'
           WHEN price BETWEEN 10000 AND 49999 THEN '중가'
           ELSE '고가'
       END AS price_group
FROM products
WHERE price BETWEEN 0 AND 100000
ORDER BY price_group ASC, rounded_price DESC;
```

이 쿼리는 가격 범위를 필터링하고, 가격을 백의 자리로 반올림하며, 조건별 이름을 만든 다음 두 기준으로 정렬한다.

## 주의사항

- 함수 이름은 `ROUND()`다. `ROUNTD()`는 존재하지 않는다.
- `ORDER BY a, b`에서 `b`는 모든 행을 다시 정렬하는 것이 아니라 `a`가 같은 행의 순서를 결정한다.
- `BETWEEN`은 양 끝값을 포함한다.
- 시간값이 있는 날짜 범위는 `BETWEEN '시작일' AND '종료일'`보다 다음 기간 시작점 미만 조건이 안전한 경우가 많다.
- `CASE`에서 여러 조건이 참이면 가장 먼저 만난 조건의 결과를 사용한다.
- `CASE`에서 `ELSE`를 생략하고 어느 조건도 맞지 않으면 일반적으로 `NULL`을 반환하므로 누락된 경우를 의도했는지 확인한다.
- 세부 반올림과 날짜 처리 동작은 DBMS 및 자료형에 따라 차이가 있을 수 있다.

## 관련 개념

- `SELECT` 문의 논리적 처리 순서
- `WHERE` 조건식
- `NULL` 비교와 처리
- 컬럼 별칭

## 검증 필요

- SQL 코딩테스트에서 사용한 DBMS 종류와 버전
- 사용자가 실제로 헷갈렸던 각 문제의 입력값과 작성 쿼리

## 출처

- 사용자 제공 SQL 연습 메모
- [MySQL 8.4 — Numeric Functions and Operators](https://dev.mysql.com/doc/refman/8.4/en/numeric-functions.html)
- [MySQL 8.4 — SELECT Statement](https://dev.mysql.com/doc/refman/8.4/en/select.html)
- [MySQL 8.4 — Expressions](https://dev.mysql.com/doc/refman/8.4/en/expressions.html)
- [MySQL 8.4 — Flow Control Functions](https://dev.mysql.com/doc/refman/8.4/en/flow-control-functions.html)
