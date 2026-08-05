---
title: Optional
category: java
type: concept
status: draft
created: 2026-08-05
updated: 2026-08-05
sources:
  - "https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Optional.html"
related:
  - "[[Spring Data JPA 쿼리 메서드]]"
tags:
  - backend
  - java
verification: required
---

# Optional

## 한눈에 보기

`Optional<T>`는 값이 있거나 없을 수 있음을 나타내는 컨테이너다. 주로 결과가 없을 수 있는 메서드의 반환 타입으로 사용해 `null` 반환 가능성을 호출자에게 드러낸다.

## 오늘 확인할 내용

- `of`, `ofNullable`, `empty`의 차이
- `orElse`와 `orElseGet`의 평가 시점 차이
- `map`과 `flatMap`의 반환 구조 차이
- `filter`, `ifPresent`, `orElseThrow` 사용 시점
- `isPresent()` 확인 후 `get()`만 반복하는 코드가 좋지 않은 이유
- Spring Data JPA의 `findById` 결과 처리

## 비교 예제

```java
Question question = questionRepository.findById(id)
    .orElseThrow(() -> new EntityNotFoundException("question not found"));
```

## 주의할 점

- `Optional` 자체에 `null`을 대입하지 않는다.
- 단순히 모든 `null`을 기계적으로 `Optional`로 바꾸지 않는다.
- `orElse(createDefault())`는 값이 있어도 인자를 먼저 평가한다. 기본값 생성 비용이 있으면 `orElseGet(this::createDefault)`을 비교한다.

## 검증 필요

- 실제 PBL의 Repository와 Service 코드에서 `Optional`을 어떻게 처리하는지 확인한다.

## 출처

- [Java SE 17 `Optional` API](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Optional.html)
