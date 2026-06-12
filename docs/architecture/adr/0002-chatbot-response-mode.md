# 0002 — 챗봇 응답 모드: MVP 단일 JSON 응답

## Status

Accepted (2026-05-26)

## Context

챗봇 요청은 React Native 앱 → Spring Boot `/chat` → FastAPI AI 서버 → LLM 워크플로 경로로 처리되며, 응답 생성에 5~20초가 걸립니다. 이 응답을 클라이언트까지 어떤 모드로 전달할지 결정이 필요합니다 — 생성 완료 후 한 번에 반환(단일 JSON)할지, 토큰 단위로 streaming(SSE)할지.

Spring Boot 메인 백엔드는 servlet 기반 Spring MVC 스택입니다. 이 스택에서 SSE pass-through 를 구현하려면 reactive 스택(WebFlux) 도입 또는 까다로운 비동기 chunked 설정·디버깅이 필요합니다.

## Decision Drivers

- **MVP 일정**: streaming 도입 시 학습·구현 비용이 챗봇 1개 endpoint 대비 과대.
- **사용성 하한**: 현재 응답 시간(5~20초) 수준에서 단일 응답으로 사용성이 충분한가.
- **인증 경계 보존**: 모든 클라이언트 트래픽은 Spring Boot 를 경유한다는 경계(ADR-0001)를 깨지 않을 것.

## Decision

**MVP 는 단일 JSON 응답으로 합니다.** Spring Boot 가 FastAPI 호출 결과 전체를 공통 응답 envelope 로 감싸 한 번에 반환합니다.

챗봇 대화 스레드 상태는 AI 서버 측 워크플로 체크포인터가 영속하며 메인 DB 에는 저장하지 않습니다. 로그아웃·서버 재시작 시 새 스레드로 시작합니다.

**전환 조건** — 다음 중 하나가 확인되면 SSE streaming + WebFlux 로 전환합니다:

- 응답 시간이 30초를 넘거나, 응답 길이로 인한 대기 UX 저하가 확인될 때
- streaming UX 가 제품 차별점으로 부각될 때

전환 시 챗봇 endpoint 1개만 영향을 받습니다 — 해당 controller 만 교체하면 되고 다른 endpoint 는 변경이 없습니다(two-way door).

## Consequences

### Positive

- 기존 Spring MVC 스택을 유지해 코드가 단순하고, 새 패러다임 학습 비용이 없습니다.
- 응답 전체를 받은 뒤 envelope 로 감싸므로 에러 처리·로깅이 일반 REST 호출과 동일합니다.

### Negative

- 긴 응답에서 사용자가 생성 완료까지 빈 화면으로 대기합니다. token-by-token 출력 UX 가 없습니다.

## Alternatives Considered

| ID | Description | Why rejected |
|----|-------------|--------------|
| ALT-1 | MVP 부터 SSE + Spring WebFlux | reactive 패러다임 학습 + 새 의존성 비용이 챗봇 1개 endpoint 를 위해 전체 스택을 바꾸는 수준. |
| ALT-2 | React Native → FastAPI 직호출 (Spring Boot 우회) | streaming 은 자연스러우나 내부 인증 경계(ADR-0001)와 충돌 — 인증·rate-limit 책임이 AI 서버로 이동하고 클라이언트-백엔드 인증 경계가 모호해짐. |

## References

- Server-Sent Events (WHATWG HTML Living Standard) — https://html.spec.whatwg.org/multipage/server-sent-events.html

## Revision History

- 2026-06-12: 최초 작성 (결정 2026-05-26).
