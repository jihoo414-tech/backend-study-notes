---
title: Java 문자열 공백 처리
category: java
type: comparison
status: reviewing
created: 2026-08-10
updated: 2026-08-10
sources:
  - "[[2026-08-10 PBL mission-01 Java String Utils 완료 메모]]"
  - "https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/String.html"
  - "https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/regex/Pattern.html"
related:
  - "[[mission-01 Java String Utils]]"
  - "[[Spring Bean Validation]]"
tags:
  - java
  - string
  - regex
  - validation
verification: partial
---

# Java 문자열 공백 처리

## 한눈에 보기

```java
text.replaceAll("\\s+", " "); // 연속된 정규식 공백을 하나의 공백으로 치환
text.isBlank();                 // 비어 있거나 공백 문자로만 이루어졌는지
text.isEmpty();                 // 길이가 정확히 0인지
```

`isBlank()`와 `isEmpty()`는 모두 빈 문자열 `""`에 대해 `true`지만, 공백만 있는 문자열에서는 결과가 다르다.

| 입력 | `isEmpty()` | `isBlank()` |
| --- | --- | --- |
| `""` | `true` | `true` |
| `" "` | `false` | `true` |
| `"\n\t"` | `false` | `true` |
| `"a"` | `false` | `false` |

## 왜 필요한가

사용자 입력을 검사할 때 “문자가 하나도 없음”과 “공백만 입력함”은 서로 다른 상태다. 단순 자료구조 관점에서는 `" "`도 길이가 있는 문자열이지만, 이름이나 제목 같은 입력값에서는 내용이 없는 것으로 취급하는 경우가 많다. 요구사항에 따라 두 기준을 구분해야 한다.

## 핵심 개념

### `replaceAll()`과 `"\\s+"`

`String.replaceAll(regex, replacement)`은 첫 번째 인자를 일반 문자열이 아니라 **정규 표현식**으로 해석하고, 일치하는 모든 부분을 두 번째 인자로 치환한 새 문자열을 반환한다. `String`은 불변 객체이므로 원본 문자열 자체가 바뀌지는 않는다.

```java
String normalized = "hello\t\n  world"
        .replaceAll("\\s+", " ");

// "hello world"
```

두 단계의 해석을 구분해야 한다.

1. Java 문자열 리터럴 `"\\s+"`가 정규식 엔진에 `\s+`로 전달된다.
2. 정규식 `\s`는 공백 문자 하나, `+`는 앞 표현식이 한 번 이상 반복됨을 뜻한다.

따라서 `"\\s+"`는 여러 칸의 공백, 탭과 줄바꿈처럼 **연속된 정규식 공백 문자 묶음**을 한 번에 찾는다.

```java
text.replaceAll("\\s+", "");  // 일치하는 공백 묶음 제거
text.replaceAll("\\s+", " "); // 일치하는 공백 묶음을 한 칸으로 정규화
```

### `isEmpty()`

`isEmpty()`는 문자열 길이가 `0`인지 확인한다.

```java
"".isEmpty();  // true
" ".isEmpty(); // false
```

공백, 탭 또는 줄바꿈도 문자이므로 하나라도 있으면 길이가 `0`이 아니다.

### `isBlank()`

`isBlank()`는 문자열이 비어 있거나, 모든 코드 포인트가 Java에서 정의한 공백 문자이면 `true`를 반환한다. Java 11부터 제공된다.

```java
"".isBlank();    // true
"  ".isBlank();  // true
"\n\t".isBlank(); // true
" a ".isBlank(); // false
```

폼 입력에서 공백만 입력한 값을 내용이 없는 값으로 처리하려면 `isBlank()` 쪽이 요구사항에 더 가까울 수 있다.

## 동작 원리

```text
길이가 0인가?
├─ 예 → isEmpty() = true, isBlank() = true
└─ 아니오
   ├─ 모든 문자가 공백인가? → isEmpty() = false, isBlank() = true
   └─ 내용 문자가 있는가?   → isEmpty() = false, isBlank() = false
```

`replaceAll()`은 판별 메서드가 아니라 정규식에 일치하는 부분을 치환하는 변환 메서드다. 입력 검증 전에 공백을 정규화할 수 있지만, 원본 입력을 변경해도 되는지는 별도의 요구사항이다.

## 예제

```java
public boolean hasContent(String value) {
    return value != null && !value.isBlank();
}
```

```java
public String normalizeSpaces(String value) {
    return value.strip().replaceAll("\\s+", " ");
}
```

첫 예제는 `null`, 빈 문자열과 공백 전용 문자열을 모두 거부한다. 두 번째 예제는 앞뒤 공백을 제거한 뒤 내부의 연속된 정규식 공백을 한 칸으로 바꾼다.

## 주의사항

- `replaceAll()`의 첫 번째 인자는 정규식이다. 문자 그대로 바꾸려면 `replace()`가 더 명확할 수 있다.
- 기본 Java 정규식에서 `\s`는 주로 스페이스, 탭, 줄바꿈, 캐리지 리턴, 폼 피드와 수직 탭을 뜻한다. **모든 유니코드 공백을 무조건 포함한다고 단정하면 안 된다.**
- 유니코드 기준의 정규식 공백이 필요하면 `UNICODE_CHARACTER_CLASS` 플래그 또는 요구사항에 맞는 문자 클래스를 검토한다.
- `isBlank()`와 `isEmpty()`는 `null`을 처리하지 않는다. `null`에서 호출하면 `NullPointerException`이 발생하므로 별도로 검사한다.
- 데이터 유효성 검사에서는 빈 값 판별뿐 아니라 길이, 형식과 비즈니스 규칙도 함께 고려한다.
- 입력을 먼저 치환하면 사용자가 입력한 원본과 의미가 달라질 수 있다. 검증과 정규화 중 어떤 동작이 필요한지 구분한다.

## 관련 개념

- [Spring Bean Validation](../spring/Spring%20Bean%20Validation.md)
- [mission-01 Java String Utils](../projects/mission-01%20Java%20String%20Utils.md)
- 정규 표현식과 수량자
- 불변 객체인 `String`
- `strip()`과 `trim()`의 차이

## 검증 필요

- PBL에서 `replaceAll("\\s+", ...)`의 실제 치환 문자열과 기대 결과
- 프로젝트가 처리해야 하는 공백의 범위가 ASCII 중심인지 유니코드 전체인지

## 출처

- 사용자 제공 PBL 완료 메모
- [Java SE 25 — String](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/String.html)
- [Java SE 25 — Pattern](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/regex/Pattern.html)
