---
title: mission-03 Attendance Manager
category: project
type: project
status: reviewing
created: 2026-08-13
updated: 2026-08-14
sources:
  - "[[2026-08-13 PBL mission-03 완료 메모]]"
  - "https://github.com/devcos-pbl/devcos-pbl-backend-14-jihoo414-tech/tree/main/missions/mission-03-attendance-manager"
related:
  - "[[List와 Map 선택 기준]]"
tags:
  - backend
  - pbl
  - java
  - collection
  - file-io
verification: partial
---

# mission-03 Attendance Manager

## 목표

날짜, 회원 이메일, 출석 여부로 이루어진 출석 기록을 메모리에서 관리하고 텍스트 파일로 저장·불러오는 `AttendanceManager`를 구현한다. 요구사항과 전체 소스는 [공개 GitHub 저장소](https://github.com/devcos-pbl/devcos-pbl-backend-14-jihoo414-tech/tree/main/missions/mission-03-attendance-manager)에서도 확인할 수 있다.

## 요구사항

- `date + memberEmail` 조합으로 출석 여부를 기록하고 조회한다.
- 같은 조합을 다시 기록하면 기존 출석 여부를 덮어쓴다.
- 저장할 때 현재 메모리 데이터를 파일 전체에 덮어쓴다.
- 파일이 없으면 빈 상태로 초기화한다.
- 파일의 한 줄이라도 형식이 잘못되면 `IllegalArgumentException`을 발생시킨다.
- 정상 파일을 불러오면 기존 메모리 상태를 파일 내용으로 교체한다.
- 외부 라이브러리와 Spring을 사용하지 않는다.

## 설계

```text
Map<AttendanceKey, Boolean>
AttendanceKey = (LocalDate date, String memberEmail)
Boolean       = present
```

`AttendanceKey`는 Java `record`로 날짜와 이메일에 대한 값 동등성과 해시 코드를 제공한다. 파일을 읽을 때는 임시 Map에서 모든 줄을 검증한 뒤 기존 상태를 교체하므로 잘못된 파일이 메모리 상태를 부분적으로 바꾸지 않는다.

자세한 선택 기준은 [List와 Map 선택 기준](../java/List%EC%99%80%20Map%20%EC%84%A0%ED%83%9D%20%EA%B8%B0%EC%A4%80.md)에 정리했다.

## 구현 코드

```java
import java.time.LocalDate;

public record AttendanceKey(
        LocalDate date,
        String memberEmail
) {
}
```

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.time.LocalDate;
import java.time.format.DateTimeParseException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class AttendanceManager {
    private final Map<AttendanceKey, Boolean> attendanceMap = new HashMap<>();

    public void markAttendance(LocalDate date, String memberEmail, boolean present) {
        validateInput(date, memberEmail);
        attendanceMap.put(new AttendanceKey(date, memberEmail), present);
    }

    private void validateInput(LocalDate date, String memberEmail) {
        if (date == null) {
            throw new IllegalArgumentException("date는 null일 수 없습니다.");
        }
        if (memberEmail == null || memberEmail.isBlank()) {
            throw new IllegalArgumentException("memberEmail은 비어있을 수 없습니다.");
        }
    }

    public boolean isPresent(LocalDate date, String memberEmail) {
        validateInput(date, memberEmail);
        return attendanceMap.getOrDefault(new AttendanceKey(date, memberEmail), false);
    }

    public void save(Path filePath) {
        if (filePath == null) {
            throw new IllegalArgumentException("filePath는 null일 수 없습니다.");
        }
        try {
            Path parent = filePath.getParent();
            if (parent != null) {
                Files.createDirectories(parent);
            }
            List<String> lines = attendanceMap.entrySet().stream()
                    .map(entry -> entry.getKey().date() + ","
                            + entry.getKey().memberEmail() + ","
                            + entry.getValue())
                    .toList();
            Files.write(filePath, lines);
        } catch (IOException e) {
            throw new RuntimeException("파일 저장에 실패했습니다.", e);
        }
    }

    public void load(Path filePath) {
        if (filePath == null) {
            throw new IllegalArgumentException("filePath는 null일 수 없습니다.");
        }
        if (!Files.exists(filePath)) {
            attendanceMap.clear();
            return;
        }

        Map<AttendanceKey, Boolean> loadedMap = new HashMap<>();
        try {
            for (String line : Files.readAllLines(filePath)) {
                String[] parts = line.split(",", -1);
                if (parts.length != 3) {
                    throw new IllegalArgumentException("잘못된 파일 형식입니다.");
                }

                LocalDate date;
                try {
                    date = LocalDate.parse(parts[0]);
                } catch (DateTimeParseException e) {
                    throw new IllegalArgumentException("잘못된 날짜 형식입니다.");
                }

                String email = parts[1];
                if (email.isBlank()) {
                    throw new IllegalArgumentException("이메일은 비어있을 수 없습니다.");
                }

                boolean present;
                if (parts[2].equals("true")) {
                    present = true;
                } else if (parts[2].equals("false")) {
                    present = false;
                } else {
                    throw new IllegalArgumentException("present는 true 또는 false여야 합니다.");
                }
                loadedMap.put(new AttendanceKey(date, email), present);
            }

            attendanceMap.clear();
            attendanceMap.putAll(loadedMap);
        } catch (IOException e) {
            throw new RuntimeException("파일 읽기에 실패했습니다", e);
        }
    }
}
```

위 코드는 확인한 구현의 동작을 유지하면서 사용하지 않는 import와 불필요한 공백을 정리한 공개 문서용 표현이다.

## 문제와 해결

### `ArrayList`에서 `Map`으로 전환

`ArrayList`에서는 같은 날짜와 이메일을 매번 선형 검색하고 중복 방지 분기를 작성해야 한다. 복합 키 기반 조회와 덮어쓰기가 핵심인 이 미션에서는 `Map`이 요구사항을 더 직접적으로 표현한다.

### 잘못된 파일로부터 상태 보호

임시 `loadedMap`에서 전체 파일을 파싱한 뒤 기존 Map을 교체한다. 따라서 중간 줄이 잘못되어도 기존 상태가 일부 데이터로 덮이지 않는다.

## 테스트와 검증

- 2026-08-14에 `starter`에서 `gradlew.bat test --console=plain`을 실행해 `BUILD SUCCESSFUL`을 확인했다.
- 현재 저장소의 공개 테스트 코드는 `date == null`과 `memberEmail == null` 예외만 직접 검사한다.
- `TEST_SPEC.md`에는 기록·조회·덮어쓰기·저장·불러오기·잘못된 파일 등 더 넓은 검증 범위가 적혀 있지만, 그 전체를 수행하는 공개 테스트 코드는 현재 저장소에서 확인되지 않는다.
- 숨겨진 테스트 결과는 확인할 수 없으므로 검증 상태를 `partial`로 유지한다.

## 개선할 점

- 원본 구현의 사용하지 않는 `BufferedWriter`, `StandardCharsets` import를 제거할 수 있다.
- `Files.write`와 `Files.readAllLines`는 별도 charset을 지정하지 않는다. 요구사항이 UTF-8을 명시하므로 `StandardCharsets.UTF_8`을 명시하면 의도가 더 분명하다.
- `HashMap` 순회 순서는 보장되지 않는다. 출력 재현성이 필요하면 저장 전에 키를 정렬한다.
