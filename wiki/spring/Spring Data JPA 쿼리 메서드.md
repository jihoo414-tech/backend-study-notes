---
title: Spring Data JPA 쿼리 메서드
category: spring
type: concept
status: reviewing
created: 2026-08-05
updated: 2026-08-05
sources:
  - "[[2026-08-05 수업 메모]]"
  - "https://docs.spring.io/spring-data/jpa/reference/repositories/query-methods-details.html"
related:
  - "[[JPA 일대다 다대일 연관관계]]"
  - "[[Optional]]"
tags:
  - backend
  - spring-data-jpa
  - repository
verification: partial
---

# Spring Data JPA 쿼리 메서드

## 한눈에 보기

`JpaRepository`를 상속하면 `save`, `findById`, `findAll`, `delete` 같은 기본 CRUD 메서드를 직접 선언하지 않아도 된다. 도메인 속성을 조건으로 조회하려면 Repository 인터페이스에 규칙에 맞는 메서드를 선언할 수 있다. 이를 파생 쿼리 메서드라고 한다.

```java
public interface QuestionRepository extends JpaRepository<Question, Long> {
    List<Question> findBySubjectContaining(String keyword);

    Optional<Question> findByIdAndAuthor(Long id, User author);
}
```

Spring Data는 첫 `By` 뒤의 조건을 엔티티 속성 이름으로 해석한다. `And`, `Or`, `Between`, `LessThan`, `Containing`, `OrderBy` 같은 키워드를 조합할 수 있다.

## 주의할 점

- 메서드의 속성 이름은 엔티티 필드와 일치해야 한다.
- 조건이 복잡해져 이름이 지나치게 길어지면 `@Query`, Specification, Querydsl 같은 대안을 검토한다.
- 결과가 없을 수 있는 단건 조회는 [Optional](../java/Optional.md)을 반환해 부재 가능성을 표현할 수 있다.
- 연관관계 속성을 따라가는 쿼리는 생성 SQL과 조회 횟수도 확인한다.

## 출처
