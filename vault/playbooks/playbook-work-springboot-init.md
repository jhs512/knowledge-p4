---
id: playbook-work-springboot-init
title: "스프링부트 프로젝트 생성 취향"
type: playbook
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "새 스프링부트 생성 표준: start.spring.io zip, JDK 25+, YAML, Group com/Artifact back, 최신 안정, Web·JPA·Security·H2·DevTools, REST, OSIV 끔. 코프링은 Kotlin DSL + 2.3.0+, dev(파일)/test(메모리) 프로필, permitAll 스켈레톤."
edges:
  - type: supports
    target: pillar-work-foundation
  - type: related_to
    target: pattern-work-context-mode-build-redirect
  - type: related_to
    target: pattern-work-windows-git-bash
tags: [spring-boot, java, kotlin, preference, project-setup]
---

# 스프링부트 프로젝트 생성 취향

새 스프링부트 프로젝트를 만들 때 따르는 표준 절차이자 기본값. 생성은 [start.spring.io](https://start.spring.io)에서 zip을 받아(`curl -d ... starter.zip`) 압축 해제한다 — wrapper/플러그인/버전이 정확히 보장됨. Windows Git Bash에서 zip 해제는 [[pattern-work-windows-git-bash]] 참고 (tar 불가, unzip → `powershell Expand-Archive` 폴백). 빌드 검증은 [[pattern-work-context-mode-build-redirect]]대로 ctx_execute로.

## 프로젝트 메타데이터

| 항목 | 값 |
|---|---|
| JDK | **25 이상** |
| 설정 파일 형식 | **YAML** (`application.yml`, `.properties` 사용 안 함) |
| Group | `com` |
| Artifact | `back` (폴더명과 무관하게 유지, 패키지 `com.back`) |
| 스프링부트 버전 | **최신 안정(stable) 버전** |
| Gradle DSL | 자프링: Groovy / **코프링: Kotlin DSL** (`build.gradle.kts`) |

## 기본 의존성

- Spring Web
- Spring Data JPA
- Spring Security
- H2 Database
- Spring Boot DevTools
- Lombok — **자바 + 스프링(자프링)일 때만** (Kotlin이면 불필요)
- 코프링 추가: `kotlin-reflect`, `jackson-module-kotlin`, `kotlin("plugin.spring")`, `kotlin("plugin.jpa")` (initializr가 자동 포함)

## 코프링 주의사항

- **Kotlin 버전이 JVM target을 결정**: Kotlin 2.2.x는 JVM target 25 미지원 → initializr가 toolchain을 24로 다운그레이드함. JDK 25 쓰려면 Kotlin 플러그인을 **2.3.0+로 수동 업그레이드** 후 toolchain 25 지정.
- 테스트는 JUnit5 기본 (`kotlin-test-junit5`). MockK/kotest는 필요해질 때 추가.

## 프로필·설정 기본값

- `application.yml`(공통): `spring.profiles.active: dev`, OSIV off, `ddl-auto: update`, SQL 로그(format/highlight/comments + hibernate bind/extract trace)
- `application-dev.yml`(기본): **H2 파일 모드** (`jdbc:h2:./db_dev;MODE=MySQL`), h2-console 활성
- `application-test.yml`: **H2 인메모리** (`jdbc:h2:mem:db_test`), `ddl-auto: create-drop`, 테스트에 `@ActiveProfiles("test")`

```yaml
spring:
  jpa:
    open-in-view: false
```

## 아키텍처 기본값

- 기본 스타일은 **REST** (API 서버 전제)
- 패키지: `com.back.domain.<도메인>.controller` / `com.back.global.security` 구조
- Security 스켈레톤: permitAll + csrf off + frameOptions sameOrigin (H2 콘솔용) — 인증은 나중에
- 부팅 확인용 스켈레톤: `GET /` 인사 컨트롤러 + `contextLoads` 테스트 1개, 생성 직후 `gradlew build`로 검증
