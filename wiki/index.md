# Wiki Index

## Java

- [List와 Map 선택 기준](java/List%EC%99%80%20Map%20%EC%84%A0%ED%83%9D%20%EA%B8%B0%EC%A4%80.md) — 순회·순서와 키 기반 조회·갱신 요구사항에 따라 Java 컬렉션을 선택하는 기준
- [Java 문자열 공백 처리](java/Java%20%EB%AC%B8%EC%9E%90%EC%97%B4%20%EA%B3%B5%EB%B0%B1%20%EC%B2%98%EB%A6%AC.md) — `replaceAll("\\s+", ...)`와 `isBlank()`·`isEmpty()`의 차이
- [Optional](java/Optional.md) — 값의 부재를 명시적으로 표현하고 처리하는 Java 컨테이너

## Spring

- [Spring Bean Validation](spring/Spring%20Bean%20Validation.md) — 요청 DTO의 제약 조건 선언, `@Valid`·`@Validated` 검증 실행과 오류 처리
- [Spring Boot 프로필별 설정](spring/Spring%20Boot%20%ED%94%84%EB%A1%9C%ED%95%84%EB%B3%84%20%EC%84%A4%EC%A0%95.md) — 개발·테스트 환경의 설정 파일 분리와 활성화
- [JPA 일대다 다대일 연관관계](spring/JPA%20%EC%9D%BC%EB%8C%80%EB%8B%A4%20%EB%8B%A4%EB%8C%80%EC%9D%BC%20%EC%97%B0%EA%B4%80%EA%B4%80%EA%B3%84.md) — 질문·답변 도메인의 양방향 연관관계와 주요 옵션
- [Spring Data JPA 쿼리 메서드](spring/Spring%20Data%20JPA%20%EC%BF%BC%EB%A6%AC%20%EB%A9%94%EC%84%9C%EB%93%9C.md) — 도메인 속성으로 Repository 조회 조건을 선언하는 규칙
- [Spring Bean 등록과 의존성 주입](spring/Spring%20Bean%20%EB%93%B1%EB%A1%9D%EA%B3%BC%20%EC%9D%98%EC%A1%B4%EC%84%B1%20%EC%A3%BC%EC%9E%85.md) — `@Bean`과 컴포넌트 스캔, 필드·생성자 주입, `self`·`@Lazy` 비교
- [ApplicationRunner](spring/ApplicationRunner.md) — Spring Boot 컨텍스트 준비 후 실행하는 시작 콜백과 Runner 호출 흐름

## Database

- [SQL SELECT 표현식과 정렬 조건](database/SQL%20SELECT%20%ED%91%9C%ED%98%84%EC%8B%9D%EA%B3%BC%20%EC%A0%95%EB%A0%AC%20%EC%A1%B0%EA%B1%B4.md) — `ROUND()`, 복수 컬럼 정렬, 범위 조건과 조건별 출력값 지정
- [관계형 데이터 모델링](database/%EA%B4%80%EA%B3%84%ED%98%95%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EB%AA%A8%EB%8D%B8%EB%A7%81.md) — 업무 규칙을 ERD와 관계형 구조로 옮기는 단계, 식별자, 대응 수와 선택성
- [데이터베이스 정규화](database/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EC%A0%95%EA%B7%9C%ED%99%94.md) — 데이터 이상과 중복을 줄이는 1NF부터 3NF까지의 과정
- [데이터베이스 정규화 연습문제](database/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EC%A0%95%EA%B7%9C%ED%99%94%20%EC%97%B0%EC%8A%B5%EB%AC%B8%EC%A0%9C.md) — 수강 신청 데이터를 1NF부터 3NF까지 직접 분해하는 문제

## CS

### Operating System

### Network

- [HTTP 멱등성](cs/network/HTTP%20%EB%A9%B1%EB%93%B1%EC%84%B1.md) — 동일 요청을 반복해도 서버에 대한 의도된 효과가 같게 유지되는 HTTP 메서드의 성질

### Computer Architecture

### Data Structure

## Algorithm

- [전화번호 목록 접두어 탐색](algorithm/%EC%A0%84%ED%99%94%EB%B2%88%ED%98%B8%20%EB%AA%A9%EB%A1%9D%20%EC%A0%91%EB%91%90%EC%96%B4%20%ED%83%90%EC%83%89.md) — 모든 문자열 쌍 비교를 각 문자열의 접두어 생성과 해시 조회로 바꾸는 탐색 전략
- [고정 길이 슬라이딩 윈도우](algorithm/%EA%B3%A0%EC%A0%95%20%EA%B8%B8%EC%9D%B4%20%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%94%A9%20%EC%9C%88%EB%8F%84%EC%9A%B0.md) — 겹치는 연속 구간에서 빠지는 값과 들어오는 값만 반영해 `O(N)`에 집계하는 방법
- [정렬된 두 배열 병합과 투 포인터](algorithm/%EC%A0%95%EB%A0%AC%EB%90%9C%20%EB%91%90%20%EB%B0%B0%EC%97%B4%20%EB%B3%91%ED%95%A9%EA%B3%BC%20%ED%88%AC%20%ED%8F%AC%EC%9D%B8%ED%84%B0.md) — 정렬된 두 입력의 현재 최솟값을 비교해 `O(N + M)`에 병합하는 발상과 불변식
- [2차원 배열의 학생 관계 비교](algorithm/2%EC%B0%A8%EC%9B%90%20%EB%B0%B0%EC%97%B4%EC%9D%98%20%ED%95%99%EC%83%9D%20%EA%B4%80%EA%B3%84%20%EB%B9%84%EA%B5%90.md) — 학생 쌍 완전 탐색에서 배열 축, 존재·전체 조건, 중복 집계와 순위 역매핑을 구분하는 방법
- [문자열 순회와 변환 패턴](algorithm/%EB%AC%B8%EC%9E%90%EC%97%B4%20%EC%88%9C%ED%9A%8C%EC%99%80%20%EB%B3%80%ED%99%98%20%ED%8C%A8%ED%84%B4.md) — 양방향 거리 갱신, 연속 구간 세기, 고정 길이 분할과 진법 변환
- [에라토스테네스의 체](algorithm/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98%20%EC%B2%B4.md) — 배열에 소수의 배수를 표시해 범위 내 소수를 찾는 알고리즘

## Architecture

## Testing

## Troubleshooting

## Projects

- [mission-01 Java String Utils](projects/mission-01%20Java%20String%20Utils.md) — Java 문자열 유틸리티를 연습하고 공백 치환·판별 차이를 학습한 PBL 미션
- [mission-03 Attendance Manager](projects/mission-03%20Attendance%20Manager.md) — 복합 키 Map과 파일 입출력으로 출석 기록의 조회·덮어쓰기·전체 교체를 구현한 PBL 미션

## Interview
