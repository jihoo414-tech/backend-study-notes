---
title: Spring Bean 등록과 의존성 주입
category: spring
type: comparison
status: reviewing
created: 2026-08-06
updated: 2026-08-06
sources:
  - "[[2026-08-06 Spring Bean 등록과 의존성 주입 수업 메모]]"
  - "https://docs.spring.io/spring-framework/reference/core/beans/java/basic-concepts.html"
  - "https://docs.spring.io/spring-framework/reference/core/beans/classpath-scanning.html"
  - "https://projectlombok.org/features/constructor"
related:
  - "[[ApplicationRunner]]"
tags:
  - backend
  - spring
  - bean
  - dependency-injection
verification: partial
---

# Spring Bean 등록과 의존성 주입

## 한눈에 보기

Spring 컨테이너에 빈을 등록하는 대표적인 두 방법은 다음과 같다.

| 구분 | Java 설정 방식 | 컴포넌트 스캔 방식 |
| --- | --- | --- |
| 등록 위치 | `@Configuration` 클래스의 `@Bean` 메서드 | 대상 클래스의 `@Component`, `@Service`, `@Repository`, `@Controller` |
| 객체 생성 제어 | 개발자가 `new`와 생성 인자를 직접 작성 | Spring이 스캔한 클래스의 생성자를 호출 |
| 적합한 경우 | 외부 라이브러리, 생성 과정·구현체·설정값을 명시적으로 제어할 때 | 애플리케이션의 일반적인 서비스·컨트롤러·리포지터리 |
| 비용 | 설정 코드와 파일이 늘 수 있음 | 등록 과정이 간결하지만 생성 구성이 클래스에 붙음 |

두 방식 모두 최종 결과는 **Spring 컨테이너가 관리하는 빈**이다. 한 프로젝트에서 함께 사용할 수도 있다.

## `@Configuration`과 `@Bean`

```java
@Configuration
public class AppConfig {

    @Bean
    public PaymentClient testPaymentClient() {
        return new PaymentClient("test", 1_000);
    }

    @Bean
    public PaymentClient productionPaymentClient() {
        return new PaymentClient("production", 10_000);
    }
}
```

`@Bean` 메서드는 객체를 생성하고 설정하는 팩터리 메서드다. 같은 클래스라도 서로 다른 생성 인자와 빈 이름으로 여러 인스턴스를 등록할 수 있고, 직접 수정할 수 없는 외부 라이브러리 클래스도 등록할 수 있다.

단, 같은 타입의 빈이 여러 개라면 주입받을 대상을 `@Qualifier`나 `@Primary` 등으로 구분해야 한다.

## 컴포넌트 스캔

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

Spring은 컴포넌트 스캔 범위에서 `@Component`와 그 특수 형태인 `@Service`, `@Repository`, `@Controller` 등을 찾아 빈 정의로 등록한다. 반복적인 설정 코드를 줄이는 데 유리하다.

### 수업 표현에서 보완할 점

컴포넌트 방식이라고 해서 클래스에서 `new`나 여러 생성자를 문법적으로 사용할 수 없는 것은 아니다.

- 애플리케이션 코드에서 `new OrderService(...)`로 만든 객체는 생성할 수 있지만, 그 객체는 Spring이 만든 빈이 아니므로 컨테이너의 관리와 자동 주입을 받지 않는다.
- 컴포넌트 클래스에도 여러 생성자를 선언할 수 있다. 다만 Spring이 어떤 생성자를 사용할지 명확해야 한다.
- 한 클래스를 서로 다른 인자·설정으로 여러 빈으로 명시적으로 등록하는 작업은 `@Bean` 방식이 더 자연스럽다.

## `@Autowired`와 `@RequiredArgsConstructor`의 정확한 관계

둘은 같은 종류의 기능이 아니다.

- `@Autowired`는 Spring에게 **주입 지점**을 알려 주는 애너테이션이다. 생성자, 필드, 세터 등에 붙일 수 있다.
- Lombok의 `@RequiredArgsConstructor`는 초기화되지 않은 `final` 필드와 일부 `@NonNull` 필드를 받는 **생성자 코드를 컴파일 시 자동 생성**한다.
- 클래스에 생성자가 하나뿐이면 Spring 4.3부터 생성자의 `@Autowired`를 생략할 수 있다. 따라서 `final` 필드와 `@RequiredArgsConstructor` 조합은 간결한 생성자 주입이 된다.

```java
@Service
@RequiredArgsConstructor
public class OrderService {
    private final OrderRepository orderRepository;
}
```

위 코드는 개념적으로 다음 코드와 같다.

```java
@Service
public class OrderService {
    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

### 필드 주입과 생성자 주입의 시점

```java
@Autowired
private OrderRepository orderRepository;
```

위와 같은 **필드 주입**은 먼저 객체를 생성한 다음 Spring의 후처리 과정에서 필드에 값을 넣는다. 반면 생성자 주입은 Spring이 생성자를 호출하여 객체를 만드는 순간 의존성을 전달한다.

따라서 수업에서 들은 내용을 정확히 고치면 다음과 같다.

- “`@Autowired`는 중간에 주입한다” → 필드에 붙인 `@Autowired`라면 객체 생성 후 주입된다.
- “`@RequiredArgsConstructor`는 완전히 만든 후 주입한다” → 반대다. 생성된 생성자를 통해 **객체 생성과 동시에** 의존성을 받는다.

생성자 주입은 필수 의존성을 `final`로 유지할 수 있고, 의존성이 빠진 불완전한 객체 생성을 막으며, Spring 없이 단위 테스트에서 직접 객체를 만들기 쉽다는 장점이 있다. `@RequiredArgsConstructor` 자체가 안전성을 만드는 것이 아니라, 이 애너테이션이 생성자 주입 코드를 간단히 만들어 주는 것이다.

## `self`, `@Lazy`와 자기 호출

```java
@Service
public class MemberService {

    @Autowired
    @Lazy
    private MemberService self;

    public void outer() {
        self.inner();
    }

    public void inner() {
        // AOP 적용 대상 로직
    }
}
```

프록시 기반 AOP에서 `this.inner()`는 현재 대상 객체 내부의 직접 호출이므로 프록시를 다시 통과하지 않는다. 반면 Spring이 주입한 `self`가 프록시라면 `self.inner()` 호출은 프록시를 거쳐 해당 메서드의 트랜잭션이나 부가 기능이 적용될 기회를 얻는다.

`@Lazy`를 주입 지점에 붙이면 실제 대상 빈을 즉시 해결하는 대신 지연 해석용 프록시를 주입할 수 있다. 자기 자신을 즉시 주입할 때 생기는 순환 참조 문제를 늦추는 데 쓰일 수 있다.

다만 자기 주입은 구조를 이해하기 어렵게 만들 수 있다. Spring 공식 문서도 가능한 경우 자기 호출이 필요하지 않도록 책임을 다른 빈으로 분리하는 방식을 먼저 고려하고, 자기 주입은 대안으로 제시한다. 오늘 수업의 프록시 상세 내용은 아직 사용자 설명이 남아 있으므로 여기서는 동작 이유만 기록한다.

## 주의사항

- `@Bean`과 `@Component` 중 하나가 항상 우월한 것은 아니다. 객체 생성 제어의 필요성과 코드의 역할에 따라 선택한다.
- `@Autowired`라는 이름만 보고 필드 주입으로 한정하면 안 된다. 생성자에도 사용할 수 있다.
- `@Lazy`는 빈의 초기 생성을 늦추는 용도와 주입 지점에 프록시를 넣는 용도로 쓰일 수 있다. JPA의 `FetchType.LAZY`와는 다른 기능이다.
- 자기 주입을 순환 참조의 일반적인 해결책으로 사용하지 않는다. 먼저 클래스 책임 분리가 가능한지 확인한다.

## 검증 필요

- 수업 코드에서 `self`로 호출한 메서드에 실제로 어떤 애너테이션 또는 AOP 기능이 붙었는지
- 사용한 Spring Boot 버전과 순환 참조 관련 설정
- 프록시에 관한 나머지 수업 내용

## 출처

- 사용자 제공 수업 메모
- [Spring Framework — `@Bean`과 `@Configuration`](https://docs.spring.io/spring-framework/reference/core/beans/java/basic-concepts.html)
- [Spring Framework — 컴포넌트 스캔과 관리 컴포넌트](https://docs.spring.io/spring-framework/reference/core/beans/classpath-scanning.html)
- [Spring Framework — 프록시와 자기 호출](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html)
- [Project Lombok — `@RequiredArgsConstructor`](https://projectlombok.org/features/constructor)
