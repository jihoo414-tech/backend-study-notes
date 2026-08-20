---
title: 세 장 카드 합의 K번째 큰 값
category: algorithm
type: concept
status: reviewing
created: 2026-08-19
updated: 2026-08-19
sources:
  - "[[2026-08-19 세 장 카드 합 K번째 큰 수 풀이]]"
related:
  - "[[Java Set과 Map 구현체별 시간 복잡도]]"
  - "[[List와 Map 선택 기준]]"
tags:
  - algorithm
  - brute-force
  - combination
  - tree-set
verification: partial
---

# 세 장 카드 합의 K번째 큰 값

## 한눈에 보기

서로 다른 카드 인덱스 세 개를 모두 선택해 합을 만들고, 중복된 합을 제거한 상태에서 K번째로 큰 값을 구한다.

```text
세 카드 인덱스 조합을 한 번씩 생성
→ 합을 TreeSet에 삽입하며 중복 제거·내림차순 유지
→ 앞에서부터 K번째 원소 반환
```

원문 제출의 문제 이름은 `최소 직사각형`, 플랫폼은 `프로그래머스`라고 적혀 있지만 실제 지문과 코드는 `K번째 큰 수` 문제다. 문제 URL과 공식 출처가 제공되지 않아 Wiki에서는 제목을 내용에 맞게 정정하되, 원본 메타데이터의 충돌은 검증 필요로 남긴다.

## 왜 TreeSet인가

이 문제의 결과 후보에는 두 요구가 동시에 존재한다.

1. 같은 합은 한 번만 센다.
2. 서로 다른 합을 큰 값부터 순회한다.

`TreeSet<Integer>`는 중복을 허용하지 않는 `Set`이면서 정렬 순서를 유지한다. 생성 시 역순 비교자를 전달하면 반복문도 큰 값부터 진행한다.

```java
TreeSet<Integer> sums = new TreeSet<>(Collections.reverseOrder());
```

`HashSet`도 중복 제거는 가능하지만 순서를 보장하지 않는다. `ArrayList`는 정렬할 수 있지만 중복을 직접 제거해야 한다. 따라서 이 문제에서는 TreeSet이 두 요구를 한 자료구조로 직접 표현한다.

다만 TreeSet이 언제나 가장 빠르다는 뜻은 아니다. `HashSet`에 합을 모은 뒤 서로 다른 값만 정렬하는 방법도 가능하며, 입력 크기와 필요한 연산에 따라 비교해야 한다.

## 조합을 한 번씩 만드는 반복문

```java
for (int i = 0; i < n; i++) {
    for (int j = i + 1; j < n; j++) {
        for (int l = j + 1; l < n; l++) {
            sums.add(arr[i] + arr[j] + arr[l]);
        }
    }
}
```

인덱스에 `i < j < l` 순서를 강제하면 다음이 보장된다.

- 같은 카드 인덱스를 두 번 선택하지 않는다.
- `(0, 1, 2)`와 `(2, 0, 1)`처럼 순서만 다른 같은 조합을 다시 만들지 않는다.
- 카드 값이 같더라도 카드 인덱스가 다르면 서로 다른 카드를 선택할 수 있다.

중복 **카드 조합**은 반복문에서 제거하고, 서로 다른 카드 조합이 우연히 만든 중복 **합계 값**은 TreeSet에서 제거한다. 두 종류의 중복을 구분해야 한다.

## 시간 복잡도

카드 `N`장 중 세 장을 고르는 조합 수를 `T`라 하면 다음과 같다.

```text
T = C(N, 3) = N(N-1)(N-2) / 6 = O(N³)
```

서로 다른 합의 개수를 `M`이라 하면 TreeSet 삽입 한 번은 `O(log M)`이다.

```text
조합 생성과 삽입: O(T log M)
K번째까지 순회: O(K)
전체: O(C(N,3) log M + K)
```

`M <= T`이므로 일반적인 N 기준 상한은 `O(N³ log N)`으로 단순화할 수 있다. 원본의 `O(N)` 분석은 삼중 반복문과 TreeSet 삽입 비용을 반영하지 않아 정확하지 않다.

이 문제의 카드 값이 1부터 100이라면 세 장의 합은 3부터 300 사이이므로 서로 다른 합은 최대 298개다. 값의 범위를 문제처럼 고정하면 `log M`은 상수로 제한되므로 N에 대한 실제 상한은 `O(N³)`으로 볼 수 있다. 그래도 삼중 반복문이 있으므로 원본에 적힌 `O(N)`은 아니다. 카드 값 범위를 일반화하면 `O(N³ log N)`까지 고려한다.

추가 공간은 서로 다른 합을 저장하는 `O(M)`이다.

## ArrayList·HashSet 풀이와 비교

### ArrayList에 모든 합 저장

```text
T개 합 생성: O(T)
정렬: O(T log T)
중복 제거·K번째 탐색: O(T)
공간: O(T)
```

동작은 가능하지만 같은 합을 여러 번 저장하고 나중에 제거한다.

### HashSet에 합 저장 후 정렬

```text
평균 삽입: O(T)
M개 값을 배열로 변환하고 정렬: O(M log M)
공간: O(M)
```

정렬된 상태가 마지막에만 필요하면 충분히 좋은 대안이다.

### TreeSet에 합 저장

```text
삽입할 때마다 중복 제거와 정렬 유지: O(T log M)
앞에서 K번째까지 순회: O(K)
공간: O(M)
```

중복 제거와 정렬된 순회가 항상 함께 필요하다는 요구를 가장 직접적으로 표현한다.

## 예제 코드

사용자의 알고리즘은 문제 요구를 올바르게 구현한다. 변수명과 반복문 블록만 다듬으면 다음처럼 표현할 수 있다.

```java
import java.util.Collections;
import java.util.TreeSet;

public class Main {
    public static int solution(int n, int k, int[] cards) {
        TreeSet<Integer> sums = new TreeSet<>(Collections.reverseOrder());

        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int l = j + 1; l < n; l++) {
                    sums.add(cards[i] + cards[j] + cards[l]);
                }
            }
        }

        int rank = 0;
        for (int sum : sums) {
            if (++rank == k) {
                return sum;
            }
        }
        return -1;
    }
}
```

## 직접 확인한 결과

Java 25.0.2 JShell에서 다음을 확인했다.

- 제공 예제: `N=10`, `K=3` → `143`
- `[1, 1, 1]`, `K=2` → 서로 다른 합이 `3` 하나뿐이므로 `-1`

이는 공개 예제와 직접 만든 경계 사례에 대한 확인일 뿐, 플랫폼 전체 테스트 통과를 의미하지 않는다.

## 주의사항

- K번째는 중복을 제거한 **서로 다른 합**을 기준으로 센다.
- `i < j < l`은 값이 아니라 카드 인덱스의 중복 선택을 막는다.
- TreeSet은 요소의 자연 순서 또는 Comparator가 필요하다.
- Comparator 기준으로 비교 결과가 0이면 TreeSet에서는 같은 원소로 취급한다. 정렬 기준이 `equals()`와 일관되는지 주의한다.
- `TreeSet<Integer>`에 `null`을 넣는 것은 자연 순서 비교에서 허용되지 않는다.
- 서로 다른 합의 개수가 K보다 작으면 `-1`을 반환한다.

## 관련 개념

- [Java Set과 Map 구현체별 시간 복잡도](../java/Java%20Set%EA%B3%BC%20Map%20%EA%B5%AC%ED%98%84%EC%B2%B4%EB%B3%84%20%EC%8B%9C%EA%B0%84%20%EB%B3%B5%EC%9E%A1%EB%8F%84.md) — HashSet·LinkedHashSet·TreeSet과 Map 구현체의 순서 및 연산 비용
- [List와 Map 선택 기준](../java/List%EC%99%80%20Map%20%EC%84%A0%ED%83%9D%20%EA%B8%B0%EC%A4%80.md) — 요구사항의 주요 연산을 기준으로 컬렉션을 선택하는 방법
- 조합과 순열의 차이
- 중복 후보와 중복 결과의 구분

## 검증 필요

- 원문의 실제 문제 이름, 플랫폼, 문제 번호와 URL
- 사용자의 실제 제출 결과와 제출에 사용한 Java 세부 버전

## 출처

- [2026-08-19 세 장 카드 합 K번째 큰 수 풀이](../../raw/inbox/2026-08-19%20%EC%84%B8%20%EC%9E%A5%20%EC%B9%B4%EB%93%9C%20%ED%95%A9%20K%EB%B2%88%EC%A7%B8%20%ED%81%B0%20%EC%88%98%20%ED%92%80%EC%9D%B4.md)
