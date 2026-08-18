---
title: Spring Bean Validation
category: spring
type: concept
status: reviewing
created: 2026-08-10
updated: 2026-08-10
sources:
  - "[[2026-08-10 HTTP 멱등성과 데이터 유효성 검사 수업 메모]]"
  - "https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html"
  - "https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-config/validation.html"
related: []
tags:
  - backend
  - spring
  - validation
  - bean-validation
verification: partial
---

# Spring Bean Validation

## 한눈에 보기

클라이언트 입력은 신뢰할 수 없으므로 백엔드는 요청 데이터를 다시 검증해야 한다. 프론트엔드 검증은 빠른 피드백과 사용자 경험을 위한 것이고, 백엔드 검증은 잘못되거나 조작된 요청이 애플리케이션 로직과 데이터베이스에 도달하지 못하게 하는 최종 경계다.

Spring MVC에서는 요청 DTO의 필드에 `@NotBlank`, `@Size` 같은 Jakarta Bean Validation 제약 조건을 선언하고, 컨트롤러 매개변수에 `@Valid` 또는 `@Validated`를 붙여 검증을 실행할 수 있다.

## 왜 필요한가

- 사용자는 브라우저 개발자 도구나 별도 HTTP 클라이언트로 프론트엔드 검사를 우회할 수 있다.
- 잘못된 형식과 범위의 데이터를 애플리케이션 진입 지점에서 차단할 수 있다.
- 필드별 오류를 일정한 응답 형식으로 반환하면 프론트엔드가 사용자에게 원인을 안내할 수 있다.
- 제약 조건을 DTO 가까이에 선언하여 입력 규칙을 읽고 재사용하기 쉬워진다.

## 핵심 개념

| 요소 | 역할 |
| --- | --- |
| `@Valid` | Jakarta 표준 애너테이션으로 대상 객체와 중첩 객체의 제약 조건 검증을 유도한다. |
| `@Validated` | Spring 애너테이션으로 기본 검증 외에 validation group을 지정할 수 있다. 사용 위치와 Spring 버전에 따라 메서드 검증과도 연결된다. |
| `@NotBlank` | 문자열이 `null`이 아니고, 공백을 제외한 문자가 하나 이상인지 검사한다. |
| `@Size(min, max)` | 문자열, 컬렉션, 배열 등의 크기가 지정 범위인지 검사한다. |
| `BindingResult` | 검증 대상 바로 다음 매개변수에 두어 바인딩·검증 오류를 컨트롤러에서 확인할 수 있다. |

`@Valid`와 `@Validated`는 검증 규칙 그 자체가 아니다. DTO 필드에 선언된 제약 조건을 실행하도록 연결하는 역할을 한다.

## 동작 원리

```text
클라이언트 요청
  → HTTP 메시지를 DTO로 변환
  → @Valid 또는 @Validated가 검증 실행
  → @NotBlank, @Size 등의 제약 조건 확인
  → 성공: 컨트롤러 로직 진행
  → 실패: 오류 정보 생성 후 클라이언트에 4xx 응답
```

Spring MVC에서는 검증이 적용되는 위치에 따라 `MethodArgumentNotValidException` 또는 `HandlerMethodValidationException`이 발생할 수 있다. 애플리케이션은 `@ControllerAdvice`와 `@ExceptionHandler` 등을 사용해 필드명, 오류 코드, 메시지를 일관된 응답으로 변환할 수 있다.

## 예제

```java
public record SignupRequest(
    @NotBlank(message = "이름은 필수입니다.")
    @Size(max = 20, message = "이름은 20자 이하여야 합니다.")
    String name
) {}
```

```java
@PostMapping("/members")
public ResponseEntity<Void> create(@Valid @RequestBody SignupRequest request) {
    memberService.create(request);
    return ResponseEntity.status(HttpStatus.CREATED).build();
}
```

검증에 실패하면 서비스 로직을 실행하지 않고 오류 응답으로 변환한다. 실제 응답 구조는 프로젝트의 예외 처리 정책에 맞춰 정해야 한다.

## 주의사항

- DTO 형식 검증과 비즈니스 규칙 검증을 구분한다. `@NotBlank`, `@Size`는 입력 모양을 검사하기 좋지만, 이메일 중복이나 주문 가능 상태처럼 저장소 조회와 도메인 판단이 필요한 규칙은 서비스·도메인 계층에서 처리한다.
- `@Size`는 문자열 길이 또는 컬렉션 크기를 검사한다. 숫자의 값 범위에는 `@Min`, `@Max` 등을 사용한다.
- `BindingResult`를 사용할 때는 검증 대상 매개변수 바로 뒤에 둬야 한다.
- 오류 메시지만 반환하지 말고 프론트엔드가 안정적으로 처리할 수 있는 오류 코드와 필드 정보를 함께 설계한다.
- Spring Framework 6.1 이상에는 MVC 내장 메서드 검증이 추가되었다. 컨트롤러의 클래스 수준 `@Validated` 사용 방식은 프로젝트의 Spring 버전과 공식 문서를 확인해야 한다.

## 관련 개념

- 요청 DTO와 데이터 바인딩
- 전역 예외 처리와 오류 응답 규격
- 계층별 검증 책임

## 검증 필요

- 수업 프로젝트의 Spring Boot·Spring Framework 버전
- 검증 실패 시 사용하는 실제 예외 처리 방식과 오류 응답 구조
- 수업에서 `@Validated`를 붙인 정확한 위치와 validation group 사용 여부

## 출처

- 사용자 제공 수업 메모
- [Spring Framework — Spring MVC Validation](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html)
- [Spring Framework — MVC Validator 설정](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-config/validation.html)
