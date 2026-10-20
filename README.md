# KMU-Moodiary-Server

> AI 감정 일기 Moodiary 서버, Gemini 감정 분류·피드백 (Spring Boot · MongoDB)

Moodiary는 하루 한 편 일기를 쓰면 AI가 감정을 분류하고 친구처럼 답장을 달아 주는 일기 서비스다. 국민대학교 재학 중이던 2024년 10월에 만들었고, 이 저장소는 그 API 서버다.

## 주요 기능
- 카카오 로그인 후 JWT 발급, 내 정보 조회·수정
- 일기 작성은 하루 한 편으로 제한
- 작성·수정 시 Gemini(Vertex AI)가 감정을 HAPPY, SAD, ANGRY, SURPRISED, NEUTRAL 중 하나로 분류하고 피드백을 만든다
- 일기 목록(본문 제외), 상세 조회, 수정, 삭제

API (context path `/api`)

| 메서드 | 경로 | 설명 |
|---|---|---|
| POST | `/auth/login` | 카카오 인가 코드로 로그인 |
| GET | `/auth/me` | 내 정보 |
| PUT | `/user/my/info` | 내 정보 수정 |
| GET | `/diary` | 내 일기 목록 |
| POST | `/diary/create` | 일기 작성 |
| GET · PATCH · DELETE | `/diary/{id}` | 일기 조회·수정·삭제 |

## 기술 스택
| 영역 | 기술 |
|---|---|
| 언어 | Java 17 |
| 프레임워크 | Spring Boot 3.3.4 (Web, Security, Validation), Spring AI Vertex AI Gemini, java-jwt, Unirest |
| 데이터 | MongoDB (Spring Data MongoDB) |
| 인프라 | Docker |

## 구조
```
src/main/java/com/github/son_daehyeon/
  common/security/   JWT 필터, 인증
  common/handler/    예외 응답 처리
  domain/auth/       카카오 로그인
  domain/user/       유저 정보
  domain/diary/      일기 CRUD, Gemini 감정 분류·피드백
src/main/resources/config/*.example.yaml   설정 예시
```

## 실행
```bash
for f in jwt kakao mongo openai; do cp src/main/resources/config/$f.example.yaml src/main/resources/config/$f.yaml; done
./gradlew bootRun
```
설정 키: `app.jwt.key`, `app.oauth.client-id`, `app.oauth.redirect-uri`, `spring.data.mongodb.*`, `spring.ai.vertex.ai.gemini.project-id`, `spring.ai.vertex.ai.gemini.location`. 파일 이름은 `openai.yaml`이지만 내용은 Vertex AI Gemini 설정이다.

## 관련 저장소
- [KMU-Moodiary-Client](https://github.com/son-daehyeon/KMU-Moodiary-Client) — 웹 프런트엔드

---
2024 · 개인 프로젝트 · 보관됨
