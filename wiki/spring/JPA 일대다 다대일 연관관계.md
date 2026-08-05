---
title: JPA 일대다 다대일 연관관계
category: spring
type: concept
status: reviewing
created: 2026-08-05
updated: 2026-08-05
sources:
  - "[[2026-08-05 수업 메모]]"
  - "[[2026-08-05 JPA 연관관계 코드]]"
  - "https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2"
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

## Optional과 함께 보기

수업 예제의 `findById(5).get()`은 값이 없으면 `NoSuchElementException`을 발생시킨다. 조회 실패의 의미를 드러내려면 [Optional](../java/Optional.md)에서 다루는 `orElseThrow` 사용을 검토한다.

## 검증 필요

- 현재 PBL의 트랜잭션 경계와 실제 엔티티 상태를 확인해 `addAnswer` 호출만으로 INSERT가 발생하는 조건을 검증해야 한다.
- 질문 삭제 시 답변을 함께 삭제하는 것이 요구사항에 맞는지 확인한 뒤 `CascadeType.REMOVE`를 확정해야 한다.

## 출처
