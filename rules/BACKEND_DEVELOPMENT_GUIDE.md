# Hodolog Backend Development Guide

## 1. Project Structure (프로젝트 구조)

본 프로젝트는 계층형 아키텍처(Layered Architecture)를 따르며, 각 계층은 명확하게 분리된 책임을 갖습니다.

``` Structure
hodolog/
├── src/
│   ├── main/
│   │   ├── java/com/hodolog/api/
│   │   │   ├── config/         # Spring Security, Web MVC 등 설정 파일
│   │   │   ├── controller/     # API 엔드포인트 및 HTTP 요청/응답 처리
│   │   │   ├── domain/         # JPA 엔티티 (도메인 모델)
│   │   │   ├── exception/      # 사용자 정의 예외 클래스
│   │   │   ├── repository/     # 데이터 접근 계층 (JPA Repository, QueryDSL)
│   │   │   ├── request/        # API 요청 DTO (Data Transfer Object)
│   │   │   ├── response/       # API 응답 DTO
│   │   │   └── service/        # 비즈니스 로직 처리
│   │   └── resources/
│   │       ├── application.yml # 애플리케이션 주요 설정
│   │       └── mapper/         # MyBatis 매퍼 XML
│   └── test/
│       └── java/com/hodolog/api/ # 테스트 코드
└── pom.xml                     # Maven 프로젝트 설정
```

- **`controller`**: HTTP 요청을 받아 유효성을 검사하고, `service` 계층에 비즈니스 로직 처리를 위임합니다.
- **`service`**: 핵심 비즈니스 로직을 수행합니다. 트랜잭션 관리의 단위가 됩니다.
- **`repository`**: 데이터베이스와 상호작용합니다. JPA, QueryDSL, MyBatis를 사용하여 데이터를 조회하고 조작합니다.
- **`domain`**: 데이터베이스 테이블과 매핑되는 JPA 엔티티 클래스를 정의합니다.
- **`request`/`response`**: API의 요청과 응답에 사용되는 DTO를 정의하여, `domain` 객체가 외부에 직접 노출되지 않도록 합니다.
- **`exception`**: `PostNotFound` 와 같이 특정 상황에 맞는 사용자 정의 예외를 정의합니다.
- **`config`**: `SecurityConfig`, `WebMvcConfig` 등 애플리케이션의 핵심 설정을 담당합니다.

## 2. Coding Conventions (코딩 규칙)

### Naming Convention (이름 규칙)

- **Classes**: `PascalCase` (예: `PostController`, `PostService`)
- **Methods & Variables**: `camelCase` (예: `getList`, `postRepository`)
- **API Endpoints**: 리소스는 복수형 명사를 사용합니다. (예: `/posts`, `/users`)

### Architecture & Design (아키텍처 및 설계)

- **Layered Architecture**: `Controller` -> `Service` -> `Repository` 순서로 의존성이 흐릅니다. 각 계층은 자신의 역할에만 충실합니다.
- **Dependency Injection**: 생성자 주입을 사용합니다. Lombok의 `@RequiredArgsConstructor`를 사용하여 코드를 간결하게 유지합니다.
- **DTO Pattern**: `request`와 `response` DTO를 사용하여 API 계층과 도메인 모델을 분리합니다. 이를 통해 API 스펙 변경이 도메인 모델에 미치는 영향을 최소화합니다.
- **Entity Management**: `domain` 객체(Entity)는 `service` 계층까지만 사용하며, `controller` 계층으로 전달하지 않습니다. `controller`에서는 `response` DTO로 변환하여 반환합니다.

### API Development (API 개발)

- **Validation**: 요청 DTO(`request`)에서 `@NotBlank`, `@Email` 등 Jakarta Bean Validation 애노테이션을 사용하여 입력값 검증을 수행합니다.
- **Error Handling**: 비즈니스 로직 상 예외가 필요한 경우, `exception` 패키지에 사용자 정의 예외를 생성하여 사용합니다. 전역 예외 처리는 `ExceptionController`에서 담당합니다.
- **Authentication**: 인증이 필요한 API는 Spring Security를 통해 접근을 제어합니다.

### Database (데이터베이스)

- **JPA & QueryDSL**: 복잡한 동적 쿼리는 QueryDSL을 사용하여 타입-세이프(type-safe)하게 작성합니다.
- **MyBatis**: 필요한 경우, `resources/mapper/` 경로에 XML 매퍼를 작성하여 MyBatis를 함께 사용할 수 있습니다.

### Testing (테스트)

- **Test Framework**: JUnit 5와 Spring Boot Test를 사용합니다.
- **Controller Tests**: `MockMvc`를 사용하여 API 엔드포인트를 테스트합니다.
- **Test Naming**: `@DisplayName`을 사용하여 테스트 목적을 한글로 명확하게 기술합니다. (예: `@DisplayName("글 작성 요청시 DB에 값이 저장된다.")`)
- **BDD Style**: `given-when-then` 구조를 주석이나 메서드 분리를 통해 암시적으로 따릅니다.

### Logging (로깅)

- **Logger**: SLF4J와 Logback을 사용합니다. Lombok의 `@Slf4j` 애노테이션을 사용하여 로거를 주입합니다.
- **Log Level**: 운영 환경에서는 `INFO` 레벨 이상을, 개발 중에는 `DEBUG` 레벨을 사용하여 로그를 기록합니다.
