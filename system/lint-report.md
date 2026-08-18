# Wiki Lint Report

## 검사 현황

- 검사일: 2026-08-15
- 검사 범위: `wiki/` Markdown 23개, 공개 대상 `reviews/`, frontmatter, Index, 로컬·외부 링크, Git 추적 상태, Graphify 연결 정보
- Wiki 문서 수: Index를 제외한 22개
- Index에 등록된 문서: 21개
- 로컬 Markdown 링크 오류: 0개
- 중복 제목: 0개
- 공개 문서의 로컬 절대경로: 0개
- 외부 URL: 고유 링크 31개 중 28개 접근 가능, PBL GitHub 링크 3개는 비로그인 기준 `404`
- 검증 상태: `partial` 19개, `required` 2개, `completed` 0개, frontmatter 없음 1개

## 요약

문서 본문의 상대 경로와 제목 중복은 전반적으로 양호하다. Spring, Spring Data JPA, Jakarta Persistence, Java Optional, HTTP 멱등성의 주요 설명을 현재 공식 문서와 표본 대조했으며 직접 충돌하는 설명은 발견하지 않았다.

가장 큰 문제는 **로컬에서는 정상인 링크가 공개 GitHub에서는 깨지는 구조**다. `raw/`는 Git에서 제외되지만 Wiki와 일일 리뷰가 `raw/` 파일을 직접 링크하고 있다. 또한 프로젝트 Wiki가 공개 저장소라고 설명하는 PBL URL 세 개는 공개 접근 시 `404`를 반환한다. 공개 포트폴리오로 사용하기 전에 우선 수정해야 한다.

## 높은 우선순위

### 1. Git에서 제외된 `raw/`를 공개 문서가 직접 링크함

- 발생 건수: 21개 링크, 9개 문서
- Wiki 문서: 4개
  - `wiki/algorithm/2차원 배열의 학생 관계 비교.md`
  - `wiki/database/관계형 데이터 모델링.md`
  - `wiki/java/List와 Map 선택 기준.md`
  - `wiki/spring/JPA 일대다 다대일 연관관계.md`
- 일일 리뷰: 5개
  - `reviews/daily/2026-08-06.md`
  - `reviews/daily/2026-08-10.md`
  - `reviews/daily/2026-08-13.md`
  - `reviews/daily/2026-08-14.md`
  - `reviews/daily/2026-08-15.md`

`raw/`에는 `.gitkeep`만 추적할 수 있으므로 위 링크는 로컬 Obsidian에서는 열리지만 공개 GitHub에서는 대상 파일이 존재하지 않는다. 특히 `관계형 데이터 모델링.md`의 ERD 이미지도 `raw/assets/`에 있어 공개 화면에서 렌더링되지 않는다.

권장 처리:

1. 공개 문서 이해에 필요한 핵심 요구사항·코드·검증 결과는 현재처럼 Wiki 본문에 유지한다.
2. 공개 문서 본문의 `raw/` 링크는 링크가 아닌 로컬 출처 표기로 바꾸거나 제거한다.
3. 공개 가치와 재배포 권한이 있는 ERD만 별도의 Git 추적 공개 자산 경로로 복사하고 Wiki 링크를 교체한다.
4. frontmatter의 로컬 `sources` Wikilink는 Obsidian 출처 추적용이라는 점을 문서 정책에 명확히 표시한다.

### 2. 공개 저장소라고 설명한 PBL URL 세 개가 `404`

- mission-01: `.../mission-01-java-string-utils`
- mission-03: `.../mission-03-attendance-manager`
- mission-04: `.../mission-04-library-rental`

영향 문서:

- `wiki/projects/mission-01 Java String Utils.md`
- `wiki/projects/mission-03 Attendance Manager.md`
- `wiki/java/List와 Map 선택 기준.md`

mission-01과 mission-03 문서는 본문에서 해당 URL을 **공개 GitHub 저장소**라고 설명하지만 2026-08-15 비로그인 요청은 `404`를 반환했다. 저장소가 비공개이거나 경로가 달라졌다면 공개 독자는 구현 근거를 확인할 수 없다.

권장 처리:

- 실제 공개 URL이 생기기 전까지 `공개 GitHub 저장소` 표현과 링크를 제거한다.
- 문서 안에 이미 포함한 요구사항·핵심 코드·테스트 결과만으로 이해 가능하도록 유지한다.
- 향후 공개 미러를 만들면 그때 URL을 다시 연결한다.

### 3. Index가 가리키는 Wiki 문서 12개가 아직 Git 미추적 상태

현재 작업 트리 기준으로 Wiki Markdown 23개 중 11개만 Git이 추적하며 다음 12개는 미추적 상태다.

- 알고리즘 문서 5개 전부
- `wiki/cs/network/HTTP 멱등성.md`
- `wiki/database/SQL SELECT 표현식과 정렬 조건.md`
- `wiki/java/Java 문자열 공백 처리.md`
- `wiki/java/List와 Map 선택 기준.md`
- 프로젝트 문서 2개
- `wiki/spring/Spring Bean Validation.md`

Index 자체는 이 문서들을 가리키므로 로컬에서는 정상이나, 커밋 전 공개 저장소에서는 Index 링크가 깨진다. 이번 lint는 커밋이나 staging을 수행하지 않았다.

## 중간 우선순위

### 4. `wiki/overview.md`가 문서 메타데이터와 Index에서 제외됨

`wiki/overview.md`는 frontmatter가 없고 `wiki/index.md`에도 항목이 없다. 현재 내용은 Wiki 안내 문서로 유효하지만 다른 Wiki 문서와 관리 규칙이 다르다.

선택지는 다음 중 하나다.

- 공식 Wiki 문서로 관리한다면 `type: summary` frontmatter와 Index 항목을 추가한다.
- 단순 진입 페이지로 예외 처리한다면 AGENTS 또는 Wiki 관리 규칙에 예외를 명시한다.

### 5. `데이터베이스 정규화 연습문제`의 출처가 비어 있음

`wiki/database/데이터베이스 정규화 연습문제.md`는 `sources: []`, `verification: required`다. 문제 자체가 AI가 만든 연습문제인지, 수업 자료에서 변형한 것인지 출처와 작성 주체를 확인할 수 없다.

권장 처리:

- 자체 제작 문제라면 `AI 생성 연습문제 — 데이터베이스 정규화 문서 기반`처럼 생성 근거를 기록한다.
- 수업 자료에서 변형했다면 원본을 재배포하지 않는 범위에서 출처와 변형 사실을 기록한다.

### 6. 모든 지식 문서가 아직 검증 완료 전 단계

- `verification: partial`: 19개
- `verification: required`: 2개
  - `wiki/java/Optional.md`
  - `wiki/database/데이터베이스 정규화 연습문제.md`
- `verification: completed`: 0개

2026-08-05 이후 작성된 문서뿐이므로 30일 이상 방치된 오래된 검증 항목은 아직 없다. 다만 공개 포트폴리오의 신뢰도를 위해 다음 순서로 일부 문서를 `completed`까지 올리는 것이 좋다.

1. 공식 문서 출처가 있고 설명 범위가 작은 `Optional`, `ApplicationRunner`, `HTTP 멱등성`
2. 직접 실행 결과가 있는 알고리즘 문서
3. 공개 테스트 범위가 제한된 PBL 문서는 숨김 테스트를 추정하지 않고 계속 `partial` 유지

## 연결 품질 개선 후보

### 7. Index 외 본문에서 들어오는 링크가 없는 문서 9개

다음 문서는 Index에서는 찾을 수 있지만 다른 Wiki 본문에서 직접 연결되지 않는다.

- `wiki/algorithm/2차원 배열의 학생 관계 비교.md`
- `wiki/algorithm/고정 길이 슬라이딩 윈도우.md`
- `wiki/algorithm/문자열 순회와 변환 패턴.md`
- `wiki/cs/network/HTTP 멱등성.md`
- `wiki/database/SQL SELECT 표현식과 정렬 조건.md`
- `wiki/spring/ApplicationRunner.md`
- `wiki/spring/Spring Bean 등록과 의존성 주입.md`
- `wiki/spring/Spring Boot 프로필별 설정.md`
- `wiki/spring/Spring Data JPA 쿼리 메서드.md`

고립 문서라고 해서 모두 문제가 되는 것은 아니다. 현재 학습 사례와 관계가 명확할 때만 본문 링크를 추가하고, 같은 분류라는 이유만으로 억지로 연결하지 않는다.

Graphify는 28개의 연결 1개 이하 개념 노드를 보고한다. 이는 문서 단위 고립 9개와 다른 지표이며, 세부 개념 노드까지 포함한 보강 후보로만 취급한다.

### 8. Graphify 보고서의 Corpus Check가 증분 입력만 표시함

현재 `GRAPH_REPORT.md`는 그래프 전체가 106개 노드·146개 연결임에도 Corpus Check를 `2 files`로 표시한다. 마지막 증분 갱신에서 바뀐 파일 수가 전체 코퍼스 수처럼 기록된 결과다.

그래프 자체에는 기존 노드가 유지되어 있지만, 보고서의 파일 수와 단어 수는 전체 Wiki 규모를 나타내지 않는다. Graphify 업데이트 절차에서 보고서 생성 시 `all_files` 기준 통계를 사용하도록 개선할 후보로 남긴다.

또한 `Algorithms Text SQL and Validation` 커뮤니티의 cohesion은 `0.1286549707602339`로 낮다. 알고리즘·SQL·Validation 문서가 실제로 하나의 주제라서가 아니라 `Wiki Index` 허브와 소수의 교차 연결이 군집화에 강하게 작용한 결과로 보인다. 이 수치만 근거로 Wiki 문서를 자동 분할하지 않는다.

## 정확성 표본 대조

다음 현재 공식 문서와 주요 설명을 대조했으며 직접 충돌은 발견하지 않았다.

- [Spring Boot — SpringApplication과 Runner](https://docs.spring.io/spring-boot/reference/features/spring-application.html)
- [Spring Framework — AOP 프록시와 자기 호출](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html)
- [Spring Boot — 외부 설정과 프로필별 파일](https://docs.spring.io/spring-boot/reference/features/external-config.html)
- [Spring Data JPA — Query Methods](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html)
- [Jakarta Persistence — ManyToOne](https://jakarta.ee/specifications/persistence/3.2/apidocs/jakarta.persistence/jakarta/persistence/manytoone)
- [Java SE — Optional](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Optional.html)
- [RFC 9110 — Idempotent Methods](https://www.rfc-editor.org/rfc/rfc9110.html#name-idempotent-methods)

확인한 범위:

- Runner 실행과 `Ordered`·`@Order`
- 프록시 기반 AOP의 자기 호출 제한과 self injection의 최후 수단 성격
- 프로필별 설정의 우선순위
- 메서드 이름 기반 JPA 쿼리 파싱
- `ManyToOne.optional` 기본값과 cascade 기본값
- `Optional`의 반환 타입 중심 용도
- HTTP 멱등성의 정의와 `PUT`·`DELETE` 의미

이는 모든 문장의 완전한 외부 검증을 뜻하지 않는다. 각 문서의 사용자 경험·실행 결과·프로젝트별 설정은 계속 별도 증거가 필요하다.

## 정상 확인 항목

- 현재 로컬 파일 기준 깨진 상대 Markdown 링크 없음
- 중복된 Wiki 제목 없음
- 공개 Wiki와 README·템플릿에 사용자 홈 절대경로 없음
- `overview.md`를 제외한 모든 Wiki 문서에 필수 frontmatter 필드 존재
- frontmatter의 `sources`·`related` Wikilink 대상은 로컬에서 모두 확인됨
- 프로젝트 문서의 `category: project`는 `system/templates/project.md`와 일치함
- 확인한 공식 기술 문서와 직접 충돌하는 설명 없음

## 권장 처리 순서

1. 공개 문서의 `raw/` 본문 링크 21개와 ERD 이미지 처리
2. `404`인 PBL GitHub 링크 및 `공개 저장소` 표현 정정
3. 공개할 Wiki 문서 12개의 Git 추적 여부 검토
4. `overview.md`를 정식 문서 또는 명시적 예외로 통일
5. 정규화 연습문제의 생성 출처 기록
6. 작은 핵심 문서부터 검증 상태를 단계적으로 올림
7. 실제 학습 연결이 생길 때 고립 문서의 본문 링크 보강

이번 lint에서는 보고서 작성 외에 Wiki 문서, 링크, frontmatter, Git 추적 상태를 자동 변경하지 않았다.
