# Raw 자료 제출 템플릿

채팅이나 파일로 학습 자료 또는 일일 계획을 전달할 때 사용하는 사용자용 템플릿이다. 입력 성격에 맞는 템플릿을 복사해 작성한 뒤 Codex에게 전달한다.

## 무엇을 선택할까

| 템플릿                      | 사용 대상                       |
| ------------------------ | --------------------------- |
| `general.md`             | 유형이 모호한 일반 자료, 링크, 짧은 메모    |
| `class-notes.md`         | 강의·수업 중 작성한 필기와 강사 설명       |
| `code-and-experiment.md` | 작성한 코드, 실행 결과, 실습·실험 기록     |
| `algorithm-problem.md`   | 코딩 테스트 문제, 풀이, 오답 기록        |
| `project-reference.md`   | PBL·프로젝트 요구사항, 구현 기록, 참고 자료 |
| `daily-plan-request.md`  | 학습 계획·핵심 목표·수업·필수·선택 학습 제출  |

## 작성 원칙

- `원본` 영역은 요약하거나 고치지 말고 받은 내용 그대로 붙여 넣는다.
- 모르는 항목은 추측하지 말고 비워 두거나 `모름`이라고 쓴다.
- `category`는 `java`, `spring`, `database`, `cs`, `algorithm`, `architecture`, `testing`, `project`, `inbox` 중 하나를 사용한다.
- `category`는 Wiki 분류를 위한 메타데이터이며 Raw 저장 폴더를 결정하지 않는다. 채팅으로 제출한 텍스트 원문은 `raw/inbox/`, 이미지와 첨부 자료는 `raw/assets/`에 보존한다.
- `type`은 `concept`, `comparison`, `troubleshooting`, `project`, `experiment`, `interview`, `summary` 중 하나를 사용한다.
- 직접 실행하지 않은 코드에 성공했다고 쓰지 않는다. 실행하지 않았다면 `실행하지 않음`으로 표시한다.
- 비밀번호, API 키, 개인정보, 비공개 일정은 제거한 뒤 전달한다.
- 프로젝트나 트러블슈팅 자료를 제출해도 해당 Wiki 문서는 자동 생성되지 않는다. 필요하면 `요청사항`에 명시한다.
- 일일 계획을 요청할 때는 `daily-plan-request.md`의 다섯 항목만 작성한다. `필수 학습`에는 오늘 반드시 끝낼 일을, `선택 학습`에는 필수 학습 후 시간이 남으면 할 일을 적는다. Codex는 기존 계획과 Wiki를 확인해 시간 배분, 완료 기준, 이월 항목, 기존 지식 연결, 추천 복습과 추가 자료를 보강한다. 계획 결과의 `막힌 점`과 `하루 회고`는 사용자가 직접 작성하므로 Codex가 대신 채우지 않는다.

## 최소 제출 방법

시간이 없으면 `general.md`에서 다음 네 가지만 작성해도 된다.

1. 날짜
2. 제목
3. 분야
4. 원본

나머지는 ingest 과정에서 확인할 수 있지만, 사용자의 경험이나 실행 결과는 AI가 추정하지 않는다.

## ingest 연결

frontmatter의 `category`, `type`, `status`, `created`, `updated`, `sources`, `related`, `tags`, `verification`은 Wiki 스키마와 같은 이름을 사용한다. Raw 제출물은 기본적으로 `status: draft`, `verification: required`이며, ingest 후 생성되는 Wiki 문서의 `sources`에는 이 Raw 파일이 연결된다.
