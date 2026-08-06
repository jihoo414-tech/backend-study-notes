# 작업 로그

## [2026-08-05] plan | Vault 초기 구조 생성

- 처리한 원본: 없음
- 생성한 문서: 초기 Wiki, Planner, System 문서 및 템플릿
- 수정한 문서: 없음
- 확인이 필요한 내용: 학습 목표와 로드맵 구체화

## [2026-08-05] refactor | 일일 계획 및 정기 리뷰 흐름 정비

- 처리한 원본: 없음
- 생성한 문서: 월간 리뷰 템플릿, 주간·월간 리뷰 디렉터리
- 수정한 문서: AGENTS.md, README.md, 일일 계획 및 주간 리뷰 템플릿
- 확인이 필요한 내용: 없음

## [2026-08-05] ingest | Spring Boot 및 JPA 수업 메모

- 처리한 원본: `raw/inbox/2026-08-05 수업 메모.md`
- 생성한 문서: Spring Boot 프로필별 설정, JPA 일대다 다대일 연관관계, Spring Data JPA 쿼리 메서드
- 수정한 문서: `wiki/index.md`
- 확인이 필요한 내용: 수업에서 구분한 DB 방식과 Java 방식의 정확한 의미, PBL의 연관관계 삭제 정책

## [2026-08-05] plan | 정규화, PBL, Obsidian, Optional

- 처리한 원본: 사용자 제공 일일 계획
- 생성한 문서: `planner/daily/2026-08-05.md`, Optional 초안, 데이터베이스 정규화 초안
- 수정한 문서: `wiki/index.md`
- 확인이 필요한 내용: PBL 미션의 상세 범위와 완료 조건, 정규화 영상 시청 후 예제 보완

## [2026-08-05] ingest | JPA 연관관계 설정 코드

- 처리한 원본: `raw/inbox/2026-08-05 JPA 연관관계 코드.md`
- 생성한 문서: 없음
- 수정한 문서: JPA 일대다 다대일 연관관계, `planner/daily/2026-08-05.md`, AGENTS.md, `.gitignore`
- 확인이 필요한 내용: PBL의 트랜잭션 경계와 cascade 동작, 질문 삭제 시 답변 삭제 요구사항

## [2026-08-05] refactor | 사용자 전용 작성 영역 및 Git 제외 규칙

- 처리한 원본: 없음
- 생성한 문서: 없음
- 수정한 문서: AGENTS.md, `.gitignore`, `planner/daily/2026-08-05.md`
- 확인이 필요한 내용: 없음

## [2026-08-05] refactor | AGENTS 작업별 prompt 분리

- 처리한 원본: 없음
- 생성한 문서: `prompts/wiki-management.md`, `prompts/planning-reviews.md`, `prompts/repository-operations.md`
- 수정한 문서: AGENTS.md, README.md
- 확인이 필요한 내용: 없음

## [2026-08-05] review | 일일 학습 리뷰 도입

- 처리한 원본: 2026-08-05 수업 메모와 JPA 연관관계 코드
- 생성한 문서: `reviews/daily/2026-08-05.md`, `system/templates/daily-review.md`
- 수정한 문서: AGENTS.md, planning/repository prompt, README.md
- 확인이 필요한 내용: 정규화와 Optional 학습 완료 여부

## [2026-08-05] refactor | GitHub 호환 내부 링크

- 처리한 원본: 없음
- 생성한 문서: 없음
- 수정한 문서: Wiki, 일일 리뷰, 일일 계획, AGENTS.md, Wiki/계획 prompt, Obsidian 링크 설정
- 확인이 필요한 내용: YAML frontmatter 링크는 Obsidian 속성 호환을 위해 Wikilink 유지

## [2026-08-05] ingest | 관계형 데이터 모델링과 개념적 모델링

- 처리한 원본: `raw/inbox/2026-08-05 관계형 데이터 모델링 개념적 모델링 메모.md`
- 생성한 문서: `wiki/database/관계형 데이터 모델링.md`
- 수정한 문서: 데이터베이스 정규화, `wiki/index.md`, `planner/daily/2026-08-05.md`, `reviews/daily/2026-08-05.md`
- 확인이 필요한 내용: 첨부 ERD 이미지 미수신, `중복키`의 강의상 정의, 댓글 작성자와 관계 선택성에 관한 추가 업무 규칙

## [2026-08-05] ingest | 관계형 데이터 모델링 ERD 이미지

- 처리한 원본: `raw/assets/2026-08-05 관계형 데이터 모델링 ERD.png`
- 생성한 문서: 없음
- 수정한 문서: 관계형 데이터 모델링, `planner/daily/2026-08-05.md`, `reviews/daily/2026-08-05.md`
- 확인이 필요한 내용: 저자–댓글 관계의 대응 수와 선택성, `휴면자` 엔티티의 독립 관리 필요성

## [2026-08-05] ingest | 관계 유형의 관계형 구현

- 처리한 원본: `raw/inbox/2026-08-05 관계형 데이터 모델링 관계 구현 메모.md`
- 생성한 문서: 없음
- 수정한 문서: 관계형 데이터 모델링, 데이터베이스 정규화, `planner/daily/2026-08-05.md`, `reviews/daily/2026-08-05.md`
- 확인이 필요한 내용: `휴면자` 엔티티의 독립 관리 필요성, 정규화 예제와 생활코딩 후속 강의의 용어 대조

## [2026-08-05] refactor | 검증 출처와 추가 학습 구분

- 처리한 원본: 없음
- 생성한 문서: 없음
- 수정한 문서: 관계형 데이터 모델링, 데이터베이스 정규화
- 확인이 필요한 내용: 없음

## [2026-08-05] plan | 데이터베이스 정규화 연습문제

- 처리한 원본: 없음
- 생성한 문서: `wiki/database/데이터베이스 정규화 연습문제.md`
- 수정한 문서: 데이터베이스 정규화, `wiki/index.md`, `planner/daily/2026-08-05.md`, `reviews/daily/2026-08-05.md`
- 확인이 필요한 내용: 사용자의 풀이 완료 여부

## [2026-08-06] refactor | 일일 계획 1회 이월 및 주말 보충 규칙

- 처리한 원본: 사용자 제공 계획 운영 규칙
- 생성한 문서: 없음
- 수정한 문서: `AGENTS.md`, `prompts/planning-reviews.md`, `system/templates/weekly-review.md`, `planner/daily/2026-08-06.md`
- 확인이 필요한 내용: 2026-08-05 PBL과 2026-08-06 PBL이 같은 미션인지 여부

## [2026-08-06] refactor | 한국어 추가 자료 우선 규칙 보강

- 처리한 원본: 사용자 제공 자료 추천 선호
- 생성한 문서: 없음
- 수정한 문서: `AGENTS.md`, `prompts/planning-reviews.md`, `planner/daily/2026-08-06.md`
- 확인이 필요한 내용: 없음

## [2026-08-06] refactor | 무료 학습 자료 우선 규칙 보강

- 처리한 원본: 사용자 제공 자료 추천 선호
- 생성한 문서: 없음
- 수정한 문서: `AGENTS.md`, `prompts/planning-reviews.md`, `planner/daily/2026-08-06.md`
- 확인이 필요한 내용: 없음

## [2026-08-06] plan | AOP와 프록시 수업 예정 범위 추가

- 처리한 원본: 사용자 제공 수업 링크
- 생성한 문서: 없음
- 수정한 문서: `planner/daily/2026-08-06.md`
- 확인이 필요한 내용: AOP·프록시 수업의 실제 시작 강의와 종료 강의

## [2026-08-06] ingest | 더티체킹과 CascadeType.PERSIST 메모

- 처리한 원본: `raw/inbox/2026-08-06 더티체킹과 CascadeType.PERSIST 메모.md`
- 생성한 문서: `reviews/daily/2026-08-06.md`
- 수정한 문서: JPA 일대다 다대일 연관관계, `planner/daily/2026-08-06.md`
- 확인이 필요한 내용: PBL의 cascade 설정, 연관관계의 주인 설정, 트랜잭션 경계와 실제 SQL 실행 결과
