---
title: ApplicationRunner
category: spring
type: concept
status: reviewing
created: 2026-08-06
updated: 2026-08-06
sources:
  - "[[2026-08-06 Spring Bean 등록과 의존성 주입 수업 메모]]"
  - "https://docs.spring.io/spring-boot/api/java/org/springframework/boot/ApplicationRunner.html"
related:
  - "[[Spring Bean 등록과 의존성 주입]]"
tags:
  - backend
  - spring-boot
  - application-runner
verification: partial
---

# ApplicationRunner

## 한눈에 보기

`ApplicationRunner`는 **Spring Boot 애플리케이션의 컨텍스트 준비가 끝난 뒤 한 번 실행할 작업**을 정의하는 콜백 인터페이스다.

```java
@Component
public class DataInitializer implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) {
        System.out.println("애플리케이션 시작 후 한 번 실행");
    }
}
```

인터페이스를 구현했다는 사실만으로 실행되는 것은 아니다. 구현 객체가 `@Component`나 `@Bean`으로 **Spring 빈에 등록되어 있어야** 한다.

## 왜 자동으로 실행되는가

`SpringApplication.run(...)`이 애플리케이션을 시작할 때 Spring Boot가 정해 둔 시작 절차가 실행된다.

1. `ApplicationContext`를 만들고 설정한다.
2. 컨텍스트를 refresh하여 싱글턴 빈을 생성하고 의존성을 주입한다.
3. 컨테이너에서 `ApplicationRunner`와 `CommandLineRunner` 타입의 빈을 찾는다.
4. `@Order` 또는 `Ordered` 기준으로 정렬한다.
5. 각 Runner의 `run(...)`을 호출한다.

즉 Java나 Spring이 모든 구현 클래스를 보편적으로 자동 실행하는 것이 아니다. **Spring Boot의 `SpringApplication`이 이 인터페이스를 시작 콜백 계약으로 약속했고, 등록된 빈을 타입으로 찾아 호출하도록 구현되어 있기 때문**이다.

## 언제 사용하는가

- 개발 환경의 테스트 데이터 초기화
- 시작 시 필요한 캐시 준비나 상태 점검
- 명령행 인자를 읽어 수행하는 일회성 작업
- 애플리케이션이 요청을 받기 전에 실행해야 하는 간단한 초기화

초기화가 무겁거나 실패를 허용해야 하는 작업, 반복 스케줄 작업을 무조건 Runner에 넣는 것은 적절하지 않을 수 있다. Runner에서 예외가 발생하면 애플리케이션 시작 자체가 실패할 수 있으므로 실패 정책을 정해야 한다.

## `CommandLineRunner`와 차이

실행 시점과 목적은 거의 같다. 주된 차이는 명령행 인자를 받는 형태다.

| 인터페이스 | `run` 인자 | 특징 |
| --- | --- | --- |
| `ApplicationRunner` | `ApplicationArguments` | 옵션 인자와 일반 인자를 구조적으로 조회하기 편함 |
| `CommandLineRunner` | `String... args` | 원시 문자열 배열을 그대로 사용 |

## 여러 Runner의 순서

여러 빈이 있으면 `@Order` 또는 `Ordered`로 순서를 지정할 수 있다. 순서가 중요하다면 명시해야 하며, 같은 순서 값에 우연히 의존하지 않는다.

```java
@Component
@Order(1)
public class FirstRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) {
        // 먼저 실행할 초기화
    }
}
```

## 추가 자료

- [Spring Boot 구동 시 초기화 코드 넣는 방법](https://tweety1121.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-%EA%B5%AC%EB%8F%99%ED%95%A0-%EB%95%8C-%EC%B4%88%EA%B8%B0%ED%99%94-%EC%BD%94%EB%93%9C-%EB%84%A3%EB%8A%94-%EB%B0%A9%EB%B2%95CommandLineRunner-ApplicationRunner) — 무료 한국어 글로 빈 등록, `@Order`, 두 Runner의 인자 차이를 예제와 함께 확인할 수 있다.
- [ApplicationRunner 초기화 방법과 내부 호출 흐름](https://umanking.github.io/2021/08/06/spring-boot-application-runner-example/) — 무료 한국어 글이며 Spring Boot 소스의 Runner 수집·호출 흐름까지 다룬다.

## 검증 필요

- 오늘 수업 코드에서 Runner가 맡은 실제 초기화 작업
- 여러 Runner를 사용했는지와 실행 순서 설정 여부

## 출처

- 사용자 제공 수업 메모
- [Spring Boot API — `ApplicationRunner`](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/ApplicationRunner.html)
- [Spring Boot API — `SpringApplication`](https://docs.spring.io/spring-boot/api/java/org/springframework/boot/SpringApplication.html)
