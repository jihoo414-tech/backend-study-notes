---
title: List와 Map 선택 기준
category: java
type: comparison
status: reviewing
created: 2026-08-13
updated: 2026-08-18
sources:
  - "[[2026-08-13 PBL mission-03 완료 메모]]"
  - "[[2026-08-14 PBL mission-04 완료 메모]]"
  - "[[2026-08-18 PBL mission-07 Todo API 완료 메모]]"
  - "https://github.com/devcos-pbl/devcos-pbl-backend-14-jihoo414-tech/tree/main/missions/mission-03-attendance-manager"
  - "https://github.com/devcos-pbl/devcos-pbl-backend-14-jihoo414-tech/tree/main/missions/mission-04-library-rental"
related:
  - "[[mission-03 Attendance Manager]]"
tags:
  - java
  - collection
  - list
  - map
verification: partial
---

# List와 Map 선택 기준

## 한눈에 보기

`List`와 `Map`은 데이터가 단순한지보다 프로그램이 데이터를 **어떻게 식별하고 조회·갱신하는지**를 기준으로 선택한다.

- 순서대로 저장하고 순회하거나 중복 항목 자체가 의미 있으면 `List`가 자연스럽다.
- 고유한 키로 값을 자주 조회하거나 같은 키의 값을 교체해야 하면 `Map`이 자연스럽다.

## 왜 필요한가

자료구조는 단순히 데이터를 담는 그릇이 아니라 요구사항의 규칙을 코드로 표현한다. 맞지 않는 자료구조를 선택하면 중복 검사, 검색, 갱신 로직을 매번 직접 작성해야 하고 구현 실수도 늘어난다.

## 핵심 개념

### `List`가 자연스러운 경우

- 입력 또는 등록 순서가 중요하다.
- 같은 값이 여러 번 등장할 수 있다.
- 대부분의 작업이 전체 순회 또는 위치 기반 접근이다.
- 데이터 수가 작고 선형 검색 비용이 실제 요구사항에서 문제가 되지 않는다.

`ArrayList`에도 고유성 검증을 추가할 수 있으므로 “중복이 없어야 하면 List를 사용할 수 없다”는 뜻은 아니다. 다만 특정 식별자로 기존 항목을 자주 찾고 교체하려면 매번 `O(n)` 선형 검색이 필요하고, 고유성 규칙도 별도 코드로 유지해야 한다.

### `Map`이 자연스러운 경우

- 각 값을 식별하는 고유 키가 명확하다.
- 키로 조회·등록·갱신하는 작업이 핵심이다.
- 같은 키의 새 값으로 기존 값을 덮어쓰는 의미가 요구사항과 맞는다.
- 일반적인 `HashMap`에서 평균 `O(1)` 조회와 갱신이 유용하다.

`Map`은 키의 중복을 허용하지 않지만 값은 중복될 수 있다. 또한 `HashMap`은 저장 순서를 보장하지 않으므로 출력 순서가 요구되면 `LinkedHashMap`, `TreeMap`, 별도 정렬 등을 검토해야 한다.

## mission-03에서의 판단

미션의 핵심 규칙은 같은 `date + memberEmail` 조합이 이미 존재하면 `present` 값을 덮어쓰는 것이다. 따라서 다음 대응이 요구사항을 직접 표현한다.

```text
key   = AttendanceKey(date, memberEmail)
value = present
```

`Map<AttendanceKey, Boolean>`에서 `put(key, present)`를 호출하면 신규 기록과 기존 기록 갱신을 같은 연산으로 처리할 수 있다. `AttendanceKey`가 Java `record`이므로 구성 요소인 날짜와 이메일을 기준으로 `equals()`와 `hashCode()`가 생성되어 복합 키로 사용할 수 있다.

반면 `ArrayList`를 사용한다면 기록할 때마다 같은 날짜와 이메일을 가진 항목을 먼저 찾아야 한다. 찾으면 값을 바꾸고, 없으면 추가해야 하며, 중복 항목이 생기지 않도록 모든 입력 경로에서 이 규칙을 지켜야 한다. 구현은 가능하지만 이 미션의 중심 연산에는 `Map`보다 덜 직접적이다.

## mission-04에서의 판단

도서 대출 미션은 `id`로 단건을 찾는 기능도 있지만 `getAllBooks()`가 **등록 순서대로** 전체 도서를 반환해야 한다. 사용자는 `Map`과 `ArrayList`를 비교한 뒤 등록 순서를 자연스럽게 유지하는 `ArrayList`를 선택했다.

현재 구현은 도서를 등록할 때 리스트 끝에 추가하고 다음과 같이 복사본을 반환한다.

```java
public List<Book> getAllBooks() {
    return new ArrayList<>(bookList);
}
```

이 선택은 요구사항을 만족하는 유효한 방법이다. 반환된 목록에 항목을 추가하거나 삭제해도 서비스 내부 목록 구조가 바로 바뀌지 않는다는 장점도 있다. 다만 `id` 조회는 스트림으로 목록을 순회하므로 `O(n)`이며, 도서 수가 커지고 단건 조회가 많아지면 비용이 커질 수 있다.

순서 요구만으로 `Map`을 배제해야 하는 것은 아니다. `LinkedHashMap<Long, Book>`도 키 조회와 등록 순서 보존을 함께 제공할 수 있다. 따라서 이 미션에서 `ArrayList`가 적절한 이유는 순서 요구뿐 아니라 작은 규모, 단순한 구현, 전체 목록 반환이라는 주요 연산을 함께 고려한 결과로 보는 편이 정확하다.

## 선택 질문

자료구조를 정하기 전에 다음을 확인한다.

1. 한 항목을 고유하게 식별하는 값 또는 값의 조합이 있는가?
2. 주된 연산이 전체 순회인가, 키를 통한 조회·갱신인가?
3. 중복은 허용되는가? 중복 금지는 값 전체에 적용되는가, 키에만 적용되는가?
4. 입력·등록·정렬 순서를 보존해야 하는가?
5. 데이터 규모에서 선형 검색 비용이 허용되는가?

## mission-07에서의 판단

Todo API의 메모리 저장소는 `Map<Long, Todo>` 인터페이스에 `LinkedHashMap` 구현체를 사용했다. API 요구사항에는 ID 기반 단건 조회와 등록 순서 목록 반환이 함께 존재한다.

- `get(id)`를 이용한 단건 조회는 키 기반 접근으로 표현한다.
- `put(id, todo)`는 생성된 ID와 Todo를 연결한다.
- `values()`는 `LinkedHashMap`의 삽입 순서를 따라 Todo를 반환한다.

따라서 이 선택은 일반 `HashMap`의 키 조회와 `ArrayList`의 등록 순서 보존 요구를 하나의 자료구조로 함께 만족한다. `findAll()`은 `List.copyOf(todoStore.values())`를 반환해 저장소 내부 컬렉션을 호출부가 직접 수정하지 못하게 한다.

완료 처리에서는 기존 `record Todo`의 필드를 변경하는 대신 `completed=true`인 새 Todo를 만들어 같은 ID에 다시 `put`한다. `LinkedHashMap`에서 기존 키의 값을 교체해도 최초 삽입 순서는 유지되므로 목록 순서 요구와 충돌하지 않는다.

## 주의사항

- “데이터와 검증 규칙이 단순하면 `ArrayList`”는 충분한 기준이 아니다. 단순한 데이터라도 키 조회와 덮어쓰기가 중심이면 `Map`이 더 적합할 수 있다.
- `Map`을 쓴다고 모든 중복이 사라지는 것은 아니다. 키만 고유하고 값은 중복될 수 있다.
- 복합 키로 변경 가능한 객체를 사용하면 저장 후 해시값이 바뀌어 조회가 실패할 수 있다. 불변인 `record`는 이 용도에 적합하다.
- 자료구조 선택과 입력 검증은 별개의 책임이다. `Map`이 `null`, blank, 파일 형식 오류를 대신 검증하지 않는다.

## 관련 개념

- [mission-03 Attendance Manager](../projects/mission-03%20Attendance%20Manager.md) — 복합 키 기반 출석 기록에 `Map`을 적용한 사례
- 복합 키의 `equals()`와 `hashCode()`
- `HashMap`, `LinkedHashMap`, `TreeMap`의 순서 차이

## 검증 필요

- 사용자가 처음 작성한 `ArrayList` 구현 코드는 현재 미션 폴더에서 확인되지 않아 구체적으로 실패한 지점은 검증하지 못했다.
- mission-07은 로컬 공개 테스트 2개가 직접 통과했지만, `TEST_SPEC.md`에 나열된 전체 범위와 hidden test 결과는 확인하지 못했다.

## 출처

- mission-03의 `PROBLEM.md`, `REQUIREMENTS.md`, `TEST_SPEC.md`
- 현재 `AttendanceKey.java`, `AttendanceManager.java`
- [2026-08-13 PBL mission-03 완료 메모](../../raw/inbox/2026-08-13%20PBL%20mission-03%20%EC%99%84%EB%A3%8C%20%EB%A9%94%EB%AA%A8.md)
