---
title: Java Set과 Map 구현체별 시간 복잡도
category: java
type: comparison
status: reviewing
created: 2026-08-19
updated: 2026-08-19
sources:
  - "[[2026-08-19 세 장 카드 합 K번째 큰 수 풀이]]"
  - "https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/HashSet.html"
  - "https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeSet.html"
  - "https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/HashMap.html"
  - "https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeMap.html"
related:
  - "[[List와 Map 선택 기준]]"
  - "[[전화번호 목록 접두어 탐색]]"
  - "[[세 장 카드 합의 K번째 큰 값]]"
tags:
  - java
  - collections
  - set
  - map
  - time-complexity
verification: partial
---

# Java Set과 Map 구현체별 시간 복잡도

## 한눈에 보기

`Set`과 `Map`은 인터페이스이며 실제 시간 복잡도와 순서 특성은 구현체에 따라 달라진다.

| 필요한 기능 | 우선 검토할 구현체 |
| --- | --- |
| 값의 중복 제거·빠른 존재 확인 | `HashSet` |
| 중복 제거·삽입 순서 유지 | `LinkedHashSet` |
| 중복 제거·정렬·범위 검색 | `TreeSet` |
| 키로 값 또는 빈도 조회 | `HashMap` |
| 키 조회·삽입 순서 유지 | `LinkedHashMap` |
| 키 정렬·범위 검색 | `TreeMap` |

`Hash` 계열의 평균 `O(1)`과 `Tree` 계열의 보장된 `O(log N)`을 단순히 빠르고 느린 것으로만 비교하면 안 된다. 정렬 상태와 범위 검색이 필요한지, 중복만 제거하면 되는지를 먼저 확인해야 한다.

## Set과 Map의 차이

### Set

`Set<E>`는 서로 같은 원소를 중복 저장하지 않는다.

```text
필요한 질문: 이 값이 이미 있는가?
대표 연산: add, contains, remove
```

### Map

`Map<K, V>`는 하나의 키를 하나의 값에 대응시킨다. 키는 중복될 수 없지만 값은 중복될 수 있다.

```text
필요한 질문: 이 키에 대응하는 값은 무엇인가?
대표 연산: put, get, containsKey, remove
```

존재 여부만 필요하면 `Set`, 각 값의 개수나 추가 정보가 필요하면 `Map`이 의도를 더 직접적으로 표현한다.

## Set 구현체 시간 복잡도

### 요약 표

`N`은 저장된 원소 수다.

| 구현체 | `add` | `contains` | `remove` | 전체 순회 | 순서 |
| --- | ---: | ---: | ---: | ---: | --- |
| `HashSet` | 평균 `O(1)` | 평균 `O(1)` | 평균 `O(1)` | `O(N + capacity)` | 보장 없음 |
| `LinkedHashSet` | 평균 `O(1)` | 평균 `O(1)` | 평균 `O(1)` | `O(N)` | 삽입 순서 |
| `TreeSet` | `O(log N)` | `O(log N)` | `O(log N)` | `O(N)` | 정렬 순서 |

### HashSet

`HashSet`은 내부적으로 `HashMap`을 사용한다. 해시가 버킷에 고르게 분산된다는 가정에서 `add`, `remove`, `contains`, `size`는 평균적으로 `O(1)`이다.

```java
Set<String> seen = new HashSet<>();
if (!seen.add(word)) {
    // 이미 등장한 단어
}
```

중요한 점은 순회 비용이다. Oracle 문서는 HashSet 전체 순회가 원소 수 `N`뿐 아니라 내부 버킷 수인 `capacity`에도 비례한다고 설명한다. 초기 용량을 지나치게 크게 잡으면 순회가 불필요하게 느려질 수 있다.

해시 충돌이 심하면 성능이 나빠질 수 있으므로 평균 `O(1)`을 절대적인 최악 시간으로 이해하면 안 된다. Java 구현은 충돌 완화를 위한 내부 최적화를 사용하지만, 컬렉션 선택 단계에서 모든 해시 연산의 최악을 무조건 `O(log N)`이라고 보장해서는 안 된다.

### LinkedHashSet

`LinkedHashSet`은 해시 기반 존재 확인에 연결 정보를 추가해 삽입 순서를 유지한다.

```java
Set<Integer> orderPreserved = new LinkedHashSet<>();
```

기본 연산은 평균 `O(1)`이지만 원소마다 연결 정보를 유지하므로 HashSet보다 메모리 오버헤드가 있다. 순회는 저장 원소 수에 비례하는 `O(N)`이며, 입력에서 처음 등장한 순서대로 중복을 제거해야 할 때 적합하다.

### TreeSet

`TreeSet`은 `TreeMap` 기반의 정렬된 `NavigableSet`이다. Oracle Java 25 문서는 `add`, `remove`, `contains`에 보장된 `O(log N)` 비용을 명시한다.

```java
TreeSet<Integer> descending =
        new TreeSet<>(Collections.reverseOrder());
```

다음 기능이 필요할 때 HashSet보다 TreeSet을 검토한다.

- 항상 정렬된 순회
- 최솟값·최댓값
- `lower`, `floor`, `ceiling`, `higher`
- 특정 범위의 부분 집합

TreeSet의 중복 판단은 정렬 비교 결과가 0인지에 영향을 받는다. Comparator가 `equals()`와 일관되지 않으면 Set의 의미가 예상과 달라질 수 있다.

## Map 구현체 시간 복잡도

### 요약 표

| 구현체 | `put` | `get`·`containsKey` | `remove` | 전체 순회 | 키 순서 |
| --- | ---: | ---: | ---: | ---: | --- |
| `HashMap` | 평균 `O(1)` | 평균 `O(1)` | 평균 `O(1)` | `O(N + capacity)` | 보장 없음 |
| `LinkedHashMap` | 평균 `O(1)` | 평균 `O(1)` | 평균 `O(1)` | `O(N)` | 삽입 또는 접근 순서 |
| `TreeMap` | `O(log N)` | `O(log N)` | `O(log N)` | `O(N)` | 키 정렬 순서 |

### HashMap

Oracle 문서는 해시가 고르게 분산된다는 가정에서 `get`과 `put`을 평균 `O(1)`로 설명한다. 값의 빈도를 계산할 때 대표적으로 사용한다.

```java
Map<String, Integer> counts = new HashMap<>();
counts.put(word, counts.getOrDefault(word, 0) + 1);
```

HashSet과 마찬가지로 순회는 `N + capacity`에 비례하며, 키의 `equals()`와 `hashCode()` 계약이 올바르게 구현되어야 한다.

### LinkedHashMap

`LinkedHashMap`은 HashMap의 키 조회 특성에 순서를 추가한다.

- 기본 설정: 삽입 순서
- 접근 순서 설정: 최근 접근 순서 관리 가능

mission-07 Todo API처럼 ID 조회와 등록 순서 목록 반환을 동시에 요구할 때 유용하다. 기본 연산은 평균 `O(1)`, 순회는 `O(N)`이지만 연결 정보만큼 메모리를 더 사용한다.

### TreeMap

`TreeMap`은 Red-Black Tree 기반으로 키를 정렬한다. Oracle Java 25 문서는 `containsKey`, `get`, `put`, `remove`에 보장된 `O(log N)` 비용을 명시한다.

빈도까지 보존하면서 최솟값과 최댓값을 다뤄야 할 때 `TreeMap<E, Integer>`를 다중집합처럼 사용할 수 있다.

```java
TreeMap<Integer, Integer> counts = new TreeMap<>();
counts.merge(value, 1, Integer::sum);
```

중복 값 하나를 삭제할 때는 빈도를 1 줄이고, 빈도가 0이 되었을 때만 키를 삭제한다. 중복 자체를 버리는 TreeSet과 다른 점이다.

## ArrayList와 비교

| ArrayList 연산 | 시간 복잡도 |
| --- | ---: |
| 인덱스 `get`, `set` | `O(1)` |
| 끝에 `add` | 분할 상환 `O(1)` |
| 값 `contains` | `O(N)` |
| 중간 삽입·삭제 | `O(N)` |
| 정렬 | `O(N log N)` |

ArrayList는 위치 접근과 순회가 중심일 때 적합하다. 값의 존재 여부를 반복해서 확인하면 매번 선형 탐색이 필요하므로 Set을 함께 사용하거나 자료구조를 바꿀지 검토한다.

## 선택 순서

```text
1. 중복을 제거해야 하는가?
   └─ 예: Set 후보

2. 값마다 개수나 추가 정보를 저장해야 하는가?
   └─ 예: Map 후보

3. 정렬된 상태가 항상 필요한가?
   └─ 예: TreeSet 또는 TreeMap

4. 처음 등장하거나 삽입된 순서를 유지해야 하는가?
   └─ 예: LinkedHashSet 또는 LinkedHashMap

5. 순서가 필요 없고 빠른 존재·키 조회가 중심인가?
   └─ 예: HashSet 또는 HashMap
```

정렬이 마지막 한 번만 필요하면 HashSet·HashMap에 모은 뒤 정렬하는 방법도 비교한다. Tree 구조는 모든 삽입에서 정렬 상태를 유지하는 비용을 낸다.

## 프로그래머스 연습문제

### Set 중심

| 문제 | 연습할 핵심 |
| --- | --- |
| [폰켓몬](https://school.programmers.co.kr/learn/courses/30/lessons/1845) | 종류 번호의 중복 제거와 선택 가능한 종류 수 비교 |
| [영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981) | 이전 등장 단어를 HashSet으로 검사 |
| [연속 부분 수열 합의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/131701) | 생성한 합의 중복 제거, Set 크기를 정답으로 사용 |

### Map 중심

| 문제 | 연습할 핵심 |
| --- | --- |
| [완주하지 못한 선수](https://school.programmers.co.kr/learn/courses/30/lessons/42576) | 동명이인을 포함한 이름별 빈도 증감 |
| [의상](https://school.programmers.co.kr/learn/courses/30/lessons/42578) | 종류별 의상 개수 집계 |
| [할인 행사](https://school.programmers.co.kr/learn/courses/30/lessons/131127) | 고정 길이 구간의 상품 빈도 비교 |
| [신고 결과 받기](https://school.programmers.co.kr/learn/courses/30/lessons/92334) | 중복 신고 제거와 사용자별 관계를 `Map`과 `Set`으로 표현 |

### 정렬 Map 응용

- [이중우선순위큐](https://school.programmers.co.kr/learn/courses/30/lessons/42628) — 최솟값·최댓값과 중복 개수를 함께 관리한다. 난도가 높으므로 TreeMap과 우선순위 큐를 학습한 뒤 진행한다.

TreeSet 사용 자체를 강제하는 프로그래머스 문제는 드물다. 문제의 요구가 `중복 제거 + 항상 정렬된 순회 또는 범위 탐색`일 때 TreeSet을 선택하는 연습이 중요하다.

## 주의사항

- `Set`은 같은 값을 몇 번 넣었는지 기억하지 않는다. 빈도가 필요하면 `Map<E, Integer>`를 사용한다.
- HashSet과 HashMap은 순서를 보장하지 않는다.
- TreeSet과 TreeMap은 정렬 비교가 가능한 요소·키가 필요하다.
- 평균 `O(1)`은 해시 분산이 적절하다는 가정이다.
- Tree 구조의 `O(log N)`은 정렬 유지 비용이며, 정렬이 필요 없다면 Hash 계열이 단순하다.
- 일반 구현체는 동기화되어 있지 않으므로 여러 스레드가 동시에 구조를 변경하는 상황은 별도로 다뤄야 한다.

## 관련 개념

- [List와 Map 선택 기준](List%EC%99%80%20Map%20%EC%84%A0%ED%83%9D%20%EA%B8%B0%EC%A4%80.md) — 프로젝트 요구사항의 주요 연산을 기준으로 컬렉션을 고르는 사례
- [전화번호 목록 접두어 탐색](../algorithm/%EC%A0%84%ED%99%94%EB%B2%88%ED%98%B8%20%EB%AA%A9%EB%A1%9D%20%EC%A0%91%EB%91%90%EC%96%B4%20%ED%83%90%EC%83%89.md) — HashMap과 HashSet을 존재 확인에 적용한 알고리즘 사례
- [세 장 카드 합의 K번째 큰 값](../algorithm/%EC%84%B8%20%EC%9E%A5%20%EC%B9%B4%EB%93%9C%20%ED%95%A9%EC%9D%98%20K%EB%B2%88%EC%A7%B8%20%ED%81%B0%20%EA%B0%92.md) — TreeSet으로 서로 다른 합을 내림차순 관리한 사례

## 검증 필요

- 사용자가 각 추천 문제를 실제로 푼 결과와 선택한 구현체
- 실제 제출 Java 버전에 따른 컬렉션 구현 세부 차이

## 출처

- [Oracle Java 25 — HashSet](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/HashSet.html)
- [Oracle Java 25 — TreeSet](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeSet.html)
- [Oracle Java 25 — HashMap](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/HashMap.html)
- [Oracle Java 25 — TreeMap](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/TreeMap.html)
- 위 연습문제 표에 연결한 각 프로그래머스 공식 문제 페이지
