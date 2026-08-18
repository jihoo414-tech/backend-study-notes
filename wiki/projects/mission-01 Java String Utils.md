---
title: mission-01 Java String Utils
category: project
type: project
status: reviewing
created: 2026-08-10
updated: 2026-08-14
sources:
  - "[[2026-08-10 PBL mission-01 Java String Utils 완료 메모]]"
  - "https://github.com/devcos-pbl/devcos-pbl-backend-14-jihoo414-tech/tree/main/missions/mission-01-java-string-utils"
related:
  - "[[Java 문자열 공백 처리]]"
tags:
  - backend
  - pbl
  - java
  - string
verification: partial
---

# mission-01 Java String Utils

## 목표

외부 라이브러리 없이 문자열 정규화, 역순 변환, 회문 판별, 단어 수 계산을 구현하는 PBL 미션이다. 구현과 요구사항은 [공개 GitHub 저장소](https://github.com/devcos-pbl/devcos-pbl-backend-14-jihoo414-tech/tree/main/missions/mission-01-java-string-utils)에서도 확인할 수 있다.

## 요구사항

- `StringUtils`는 상속할 수 없는 유틸리티 클래스이며 생성자는 외부에서 호출할 수 없어야 한다.
- `normalizeSpaces`는 앞뒤 공백을 제거하고 연속된 공백 문자를 한 칸으로 바꾼다.
- `reverse`는 문자열의 순서를 뒤집는다.
- `isPalindrome`은 대소문자와 공백을 무시해 회문 여부를 판단하며 blank 입력은 `true`다.
- `countWords`는 공백 문자를 구분자로 단어 수를 계산하며 blank 입력은 `0`이다.
- 네 메서드는 모두 `null` 입력에 `IllegalArgumentException`을 발생시킨다.

## 구현 코드

```java
public final class StringUtils {

    private StringUtils() {
    }

    public static String normalizeSpaces(String input) {
        if (input == null) {
            throw new IllegalArgumentException();
        }
        input = input.trim().replaceAll("\\s+", " ");
        if (input.isEmpty()) {
            return "";
        }
        return input;
    }

    public static String reverse(String input) {
        if (input == null) {
            throw new IllegalArgumentException();
        }
        if (input.isEmpty()) {
            return "";
        }
        return new StringBuilder(input).reverse().toString();
    }

    public static boolean isPalindrome(String input) {
        if (input == null) {
            throw new IllegalArgumentException();
        }
        input = input.trim().replaceAll("\\s+", "");
        if (input.isEmpty()) {
            return true;
        }
        String reverse = new StringBuilder(input).reverse().toString();
        return reverse.equalsIgnoreCase(input);
    }

    public static int countWords(String input) {
        if (input == null) {
            throw new IllegalArgumentException();
        }
        input = input.trim();
        if (input.isBlank()) {
            return 0;
        }
        String[] words = input.split("\\s+");
        return words.length;
    }
}
```

## 학습한 내용

- `replaceAll("\\s+", ...)`로 연속된 정규식 공백 문자 묶음을 치환한다.
- `isEmpty()`는 길이 `0`만 판별하고 `isBlank()`는 빈 문자열과 공백 전용 문자열을 판별한다.
- `StringBuilder.reverse()`와 `equalsIgnoreCase()`를 조합해 대소문자를 무시한 회문을 판별할 수 있다.

자세한 개념은 [Java 문자열 공백 처리](../java/Java%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EA%B3%B5%EB%B0%B1%20%EC%B2%98%EB%A6%AC.md)에 정리했다.

## 테스트와 검증

- 공개 테스트는 클래스 구조, 네 메서드의 대표 입력, `null`, 빈 문자열과 여러 종류의 공백을 다룬다.
- 2026-08-14에 `starter`에서 `gradlew.bat test --console=plain`을 실행했고 종료 코드 `0`을 확인했다.
- 숨겨진 테스트의 구체적인 내용과 결과는 확인할 수 없으므로 전체 요구사항 통과는 검증 완료로 올리지 않는다.

## 개선할 점

- 조건문과 공백을 일관된 Java 스타일로 정리하면 가독성이 좋아진다.
- `isPalindrome`의 마지막 조건문은 비교 결과를 바로 반환해 분기를 줄일 수 있다. 위 코드는 이 동작을 보존하면서 간결하게 정리한 형태다.
