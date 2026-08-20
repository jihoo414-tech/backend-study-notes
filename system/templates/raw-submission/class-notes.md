---
title: 수업 주제
category: inbox
type: concept
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
related: []
tags:
  - backend
  - class-note
verification: required
---

# 수업 주제

## 수업 정보

- 수업 날짜: 2026-08-19
- 강의 또는 과정명: 프로그래머스 데브코스
- 강사:
- 학습 분야:
- 사용한 기술과 버전:
- 관련 교재·슬라이드·URL:

## 오늘 다룬 범위

- 프론트엔드에서 동기, 비동기 처리가 필요한 이유
-

## 원본 수업 메모

> fetch의 비동기, 동기 처리
```
// Promise 체이닝
fetch("https://jsonplaceholder.typicode.com/posts")
  .then((res) => res.json())
  .then((data) => {
    console.log("123");
  });

console.log("hihihi");

// await 동기식
// async function fetchPost() {
//   const res = await fetch("http://localhost:8080/api/v1/posts");
//   const data = await res.json();
//   console.log(data);
// }

// fetchPost();

```

## 수업에서 작성한 코드

```text

```

## 내가 이해한 내용

-

## 이해하지 못했거나 검증이 필요한 내용

-

## 강사가 강조한 내용

-

## 요청사항

> Wiki 정리 범위나 특별히 연결할 기존 지식이 있으면 작성한다.
