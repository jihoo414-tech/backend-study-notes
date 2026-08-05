---
title: Spring Boot 프로필별 설정
category: spring
type: concept
status: reviewing
created: 2026-08-05
updated: 2026-08-05
sources:
  - "[[2026-08-05 수업 메모]]"
  - "https://docs.spring.io/spring-boot/reference/features/external-config.html"
related:
  - "[[JPA 일대다 다대일 연관관계]]"
tags:
  - backend
  - spring-boot
  - configuration
verification: partial
---

# Spring Boot 프로필별 설정

## 한눈에 보기

Spring Boot는 공통 설정과 환경별 설정을 분리해 같은 애플리케이션을 개발, 테스트, 운영 환경에서 다르게 실행할 수 있다.

```text
src/main/resources/
├─ application.yml
├─ application-dev.yml
└─ application-test.yml
```

`application.yml`에는 공통값을 두고 `application-{profile}.yml`에는 해당 환경에서 덮어쓸 값만 둔다. 프로필별 파일은 기본 설정 다음에 읽히므로 같은 키를 재정의할 수 있다.

## 예제

```yaml
# application.yml
spring:
  application:
    name: pbl-service
```

```yaml
# application-dev.yml
spring:
  datasource:
    url: jdbc:h2:mem:devdb
```

프로필은 실행 옵션 `--spring.profiles.active=dev` 또는 환경 변수 `SPRING_PROFILES_ACTIVE=dev` 등으로 선택할 수 있다. `spring.profiles.active`는 프로필 전용 파일 안에 다시 선언하지 않는다.

## 주의할 점

- 비밀번호와 API 키를 YAML에 커밋하지 않는다.
- `.properties`와 YAML을 같은 위치에서 혼용하면 우선순위를 놓치기 쉽다.
- 테스트가 실제 개발 DB를 사용하지 않는지 활성 프로필과 데이터소스를 확인한다.

## 관련 개념

- Spring Profiles
- 외부 설정 우선순위
- 테스트 격리

## 출처
