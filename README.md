# Backend Wiki

백엔드 개발 학습 자료를 원본(`raw/`), 정리된 지식(`wiki/`), 학습 계획(`planner/`)으로 나누어 관리하는 Obsidian Vault입니다.

## 매일 사용하는 흐름

1. 수업 전에 오늘 계획을 AI에게 알려 `planner/daily/YYYY-MM-DD.md`를 만듭니다.
2. AI가 기존 Wiki, 추천 복습, 추가 자료를 찾아 계획에 연결합니다.
3. 수업 후 사용자가 배운 내용과 코드를 `raw/`에 직접 정리합니다.
4. AI가 `raw/` 자료를 분석해 기존 Wiki 지식과 연결하고, 관련 개념 사이의 지식 그래프를 구축합니다.
5. 변경 내용을 확인한 뒤 GitHub에 커밋하고 푸시합니다.

## 정기 리뷰

- 일일 정리: 매일 `reviews/daily/YYYY-MM-DD.md`에 그날 배운 내용과 기존 Wiki의 연결을 기록합니다.
- 주간 정리: 요청 시 `reviews/weekly/YYYY-Www.md`에 그 주의 학습 연결을 기록합니다.
- 월간 정리: 요청 시 `reviews/monthly/YYYY-MM.md`에 주간 리뷰와 Wiki 변화를 통합합니다.
- 주간·월간 계획은 만들지 않으며, 일일 계획만 `planner/daily/`에 누적합니다.
