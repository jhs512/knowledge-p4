---
id: playbook-work-springboot-init
title: "스프링부트 프로젝트 생성 취향"
type: playbook
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "새 스프링부트 프로젝트 생성 시 따르는 개인 표준: start.spring.io 참고, JDK 25+, YAML 설정, Group com / Artifact back, 최신 안정 버전, Web·JPA·Security·H2·DevTools(+자바면 Lombok) 의존성, REST 기본, OSIV 끔."
edges:
  - type: supports
    target: pillar-work-foundation
tags: [spring-boot, java, preference, project-setup]
---

# 스프링부트 프로젝트 생성 취향

새 스프링부트 프로젝트를 만들 때 따르는 표준 절차이자 기본값. 생성 시 [start.spring.io](https://start.spring.io)를 참고한다.

## 프로젝트 메타데이터

| 항목 | 값 |
|---|---|
| JDK | **25 이상** |
| 설정 파일 형식 | **YAML** (`application.yml`, `.properties` 사용 안 함) |
| Group | `com` |
| Artifact | `back` |
| 스프링부트 버전 | **최신 안정(stable) 버전** |

## 기본 의존성

- Spring Web
- Spring Data JPA
- Spring Security
- H2 Database
- Spring Boot DevTools
- Lombok — **자바 + 스프링(자프링)일 때만** (Kotlin이면 불필요)

## 아키텍처 기본값

- 기본 스타일은 **REST** (API 서버 전제)
- **OSIV(Open Session In View)는 끈다**:

```yaml
spring:
  jpa:
    open-in-view: false
```
