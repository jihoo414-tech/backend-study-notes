---
title: JPA 일대다 다대일 연관관계
category: spring
type: concept
status: reviewing
created: 2026-08-05
updated: 2026-08-06
sources:
  - "[[2026-08-05 수업 메모]]"
  - "[[2026-08-05 JPA 연관관계 코드]]"
  - "[[2026-08-06 더티체킹과 CascadeType.PERSIST 메모]]"
  - "https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2"
  - "https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html"
  - "https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/tx-decl-explained.html"
related:
  - "[[Spring Data JPA 쿼리 메서드]]"
  - "[[데이터베이스 정규화]]"
tags:
  - backend
  - jpa
  - relationship
verification: partial
---

# JPA 일대다 다대일 연관관계

## 한눈에 보기

질문 하나에 여러 답변이 달리는 모델에서는 `Question`과 `Answer`가 일대다 관계다. 관계형 DB의 외래 키는 보통 다 쪽인 답변 테이블에 있으므로 `Answer.question`이 연관관계의 주인이 된다.

```java
@Entity
class Answer {
    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    @JoinColumn(name = "question_id", nullable = false)
    private Question question;
}
```

```java
@Entity
class Question {
    @OneToMany(mappedBy = "question")
    private List<Answer> answers = new ArrayList<>();
}
```

`mappedBy = "question"`의 값은 DB 컬럼명이 아니라 `Answer` 엔티티의 필드 이름이다. DB 관계 변경은 외래 키를 관리하는 `@ManyToOne` 쪽의 값으로 결정된다.

## 주요 파라미터

| 파라미터 | 의미 | 기본값 또는 주의점 |
| --- | --- | --- |
| `mappedBy` | 반대편에서 관계를 소유한 필드 | 양방향 `@OneToMany`에서 지정 |
| `fetch` | 연관 엔티티 로딩 시점 | `OneToMany`는 `LAZY`, `ManyToOne`은 `EAGER`가 명세 기본값이므로 명시적 `LAZY`를 검토 |
| `cascade` | 부모 작업을 자식에게 전파 | 기본값은 없음. 생명주기가 같을 때만 선택 |
| `orphanRemoval` | 컬렉션에서 빠진 자식 삭제 | 기본값은 `false`; 부모가 자식을 독점 소유할 때 검토 |
| `optional` | 다대일 관계의 `null` 허용 여부 | `ManyToOne`의 기본값은 `true` |
| `targetEntity` | 대상 엔티티 타입 | 제네릭으로 타입을 알 수 있으면 보통 생략 |

## DB 반영 중심 방식

```java
Question question = questionRepository.findById(5L).orElseThrow();

Answer answer = new Answer();
answer.setContent("답변 내용");
answer.setQuestion(question);
answerRepository.save(answer);
```

`Answer.question`은 외래 키를 관리하는 연관관계의 주인이다. 이 값을 설정하고 `Answer`를 직접 저장하면 DB 관계는 반영된다. 다만 현재 Java 메모리의 `question.getAnswers()` 컬렉션에는 새 답변이 자동으로 추가되지 않으므로 같은 객체 그래프를 계속 사용하는 코드에서는 양쪽 상태가 어긋날 수 있다.

## Java 객체 관계를 함께 맞추는 방식

첫 번째 방법은 호출하는 쪽에서 양쪽을 직접 설정한다.

```java
Answer answer = new Answer();
answer.setContent("답변 내용");
answer.setQuestion(question);
question.getAnswers().add(answer);
```

DB의 외래 키와 Java 컬렉션이 같은 관계를 표현하지만, 호출자가 두 작업 중 하나를 빠뜨릴 수 있고 엔티티 내부 컬렉션이 외부에 노출된다.

두 번째 방법은 관계 설정 규칙을 도메인 메서드로 캡슐화한다.

```java
public void addAnswer(String content) {
    Answer answer = new Answer();
    answer.setContent(content);
    answer.setQuestion(this);
    answers.add(answer);
}
```

호출자는 `question.addAnswer(content)`만 사용하므로 양쪽 관계 설정을 빠뜨리기 어렵다. 관계를 만드는 책임도 `Question`에 모인다. 세 방식 중 객체 일관성과 캡슐화 측면에서 가장 안전한 형태다.

## 영속화 조건

```java
@OneToMany(
    mappedBy = "question",
    cascade = {CascadeType.PERSIST, CascadeType.REMOVE}
)
private List<Answer> answers = new ArrayList<>();
```

- `PERSIST`는 `Question`의 영속화 작업을 새 `Answer`에 전파한다.
- `REMOVE`는 `Question`을 삭제할 때 연결된 `Answer` 삭제를 전파한다.
- Java 컬렉션에 추가했다는 사실만으로 모든 상황에서 DB에 저장되는 것은 아니다. 엔티티가 영속 상태인지, 작업이 트랜잭션 안에서 수행되는지, cascade가 적용되는지를 함께 확인한다.
- 컬렉션에서 답변 하나를 제거할 때 DB에서도 자동 삭제하려면 `REMOVE`와 별개로 `orphanRemoval`의 의미를 검토해야 한다.
- `cascade` 옵션은 질문과 답변의 실제 생명주기에 맞춰 선택한다.

## 더티 체킹과 CascadeType.PERSIST 구분

### 더티 체킹: 관리 중인 엔티티의 변경을 UPDATE로 동기화

더티 체킹은 영속성 컨텍스트가 **관리 상태인 엔티티**의 영속 필드·프로퍼티 변경을 감지하는 기능이다. 변경 내용은 곧바로 SQL이 되는 것이 아니라 flush 시점에 데이터베이스와 동기화된다. 일반적인 Spring JPA 흐름에서는 트랜잭션 커밋 전에 flush가 일어나며, 실제 변경이 발견되면 `UPDATE`가 실행된다.

```java
@Transactional
public void changeQuestion(Long id, String subject) {
    Question question = questionRepository.findById(id).orElseThrow();
    question.setSubject(subject);
    // 관리 상태이므로 별도의 save 호출 없이 커밋 전 flush에서 UPDATE 가능
}
```

따라서 `더티 체킹 = 자동 UPDATE`는 기억을 돕는 요약이지만 다음 조건을 함께 기억해야 정확하다.

- 엔티티가 현재 영속성 컨텍스트의 관리 상태여야 한다.
- 변경한 값이 영속 대상 필드·프로퍼티여야 한다.
- flush가 실행되어야 데이터베이스와 동기화된다.
- 트랜잭션이 롤백되면 해당 작업은 커밋되지 않는다.
- 분리(detached) 상태의 엔티티를 단순히 수정하는 것만으로는 더티 체킹 대상이 되지 않는다.

### CascadeType.PERSIST: persist 작업을 자식에게 전파

`CascadeType.PERSIST`는 변경 감지 기능이 아니라 **영속화 생명주기 작업의 전파 규칙**이다. 부모를 `persist`할 때 연관관계가 `cascade = PERSIST` 또는 `ALL`이면 새 자식에도 `persist`가 적용된다. 관리 상태인 부모가 새 자식을 참조하는 경우에도 JPA flush 규칙에 따라 자식으로 persist가 전파될 수 있으며, 실제 `INSERT`는 flush 과정에서 실행된다.

```java
@Transactional
public void addAnswer(Long questionId, String content) {
    Question question = questionRepository.findById(questionId).orElseThrow();
    question.addAnswer(content); // 자식 생성, 양쪽 관계 설정, 컬렉션 추가
    // cascade=PERSIST라면 flush 과정에서 새 Answer INSERT 가능
}
```

즉, 컬렉션 변경을 감지하는 과정과 새 자식을 영속 상태로 만드는 cascade는 협력할 수 있지만 같은 기능은 아니다.

| 구분 | 더티 체킹 | `CascadeType.PERSIST` |
| --- | --- | --- |
| 대상 | 관리 상태 엔티티의 변경 | 부모에서 참조하는 새 자식 엔티티 |
| 역할 | 변경 상태를 flush 때 동기화 | `persist` 작업을 연관 엔티티에 전파 |
| 대표 SQL | 주로 `UPDATE` | 새 엔티티의 `INSERT` |
| 핵심 조건 | 관리 상태와 flush | cascade 설정과 올바른 연관관계 설정 |

### OneToMany 컬렉션에 추가할 때의 주의점

양방향 관계에서 `Question.answers`가 `mappedBy`로 지정된 역방향이라면 컬렉션에 자식을 추가하는 것만으로 외래 키의 값이 결정되지 않는다. DB 관계는 연관관계의 주인인 `Answer.question`을 기준으로 반영되므로 두 방향을 함께 설정해야 한다.

```java
public void addAnswer(String content) {
    Answer answer = new Answer();
    answer.setContent(content);
    answer.setQuestion(this); // 연관관계의 주인 설정
    answers.add(answer);      // 객체 그래프 일관성 유지
}
```

`CascadeType.PERSIST`가 없으면 새 `Answer`를 직접 `answerRepository.save(answer)` 또는 `EntityManager.persist(answer)`로 영속화해야 한다. 반대로 cascade가 있어도 연관관계의 주인 설정을 빠뜨리면 기대한 외래 키 관계가 반영되지 않을 수 있다.

## @Transactional과 flush 흐름

Spring의 선언적 트랜잭션은 일반적으로 AOP 프록시를 통해 메서드 실행 전후에 트랜잭션 경계를 만든다.

1. 프록시가 트랜잭션을 시작한다.
2. 조회된 엔티티가 영속성 컨텍스트의 관리 상태가 된다.
3. 애플리케이션 코드가 관리 엔티티 또는 연관관계를 변경한다.
4. 커밋 전에 영속성 컨텍스트가 flush된다.
5. 더티 체킹 결과는 `UPDATE`로, cascade persist가 적용된 새 엔티티는 `INSERT`로 동기화된다.
6. 예외와 롤백 규칙에 따라 트랜잭션이 커밋되거나 롤백된다.

`@Transactional` 자체가 더티 체킹을 수행하는 것은 아니다. 트랜잭션 안에서 JPA 영속성 컨텍스트가 변경을 추적하고 flush할 수 있는 실행 범위를 제공하는 역할로 구분한다. 또한 프록시를 거치지 않는 내부 호출에서는 기대한 트랜잭션 경계가 만들어지지 않을 수 있으므로 AOP 프록시 동작과 함께 확인해야 한다.

## Optional과 함께 보기

수업 예제의 `findById(5).get()`은 값이 없으면 `NoSuchElementException`을 발생시킨다. 조회 실패의 의미를 드러내려면 [Optional](../java/Optional.md)에서 다루는 `orElseThrow` 사용을 검토한다.

## 검증 필요

- 현재 PBL의 트랜잭션 경계, 연관관계의 주인 설정과 실제 엔티티 상태를 확인해 `addAnswer` 호출만으로 INSERT가 발생하는 조건을 검증해야 한다.
- 질문 삭제 시 답변을 함께 삭제하는 것이 요구사항에 맞는지 확인한 뒤 `CascadeType.REMOVE`를 확정해야 한다.

## 출처

- [2026-08-06 더티체킹과 CascadeType.PERSIST 메모](../../raw/inbox/2026-08-06%20%EB%8D%94%ED%8B%B0%EC%B2%B4%ED%82%B9%EA%B3%BC%20CascadeType.PERSIST%20%EB%A9%94%EB%AA%A8.md)
- [Jakarta Persistence 3.2 Specification](https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2)
- [Hibernate ORM User Guide — managed state와 flushing](https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html)
- [Spring Framework — 선언적 트랜잭션 구현](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/tx-decl-explained.html)
