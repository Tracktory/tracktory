# Architecture Decision Records

서비스 **간** 경계에 걸리는 아키텍처 결정을 기록합니다. 단일 서비스 내부의 결정은 해당 서브 레포의 ADR 이 담당합니다 (예: AI 파이프라인 내부 결정은 [tracktory-ai/docs/adr](https://github.com/Tracktory/tracktory-ai/tree/main/docs/adr)).

## 목록

| ID | 제목 | Status |
|---|---|---|
| [0001](./0001-internal-service-authentication.md) | 서비스 간 내부 인증: Preshared Token 헤더 + 사용자 식별 Envelope | Accepted |
| [0002](./0002-chatbot-response-mode.md) | 챗봇 응답 모드: MVP 단일 JSON 응답 | Accepted |

## 규약

- 파일명: `NNNN-kebab-case-title.md`, 번호는 등재 순 증가.
- 섹션: Status / Context / Decision Drivers / Decision / Consequences / Alternatives Considered / References / Revision History.
- ADR 은 **immutable** — 등재된 결정의 내용은 수정하지 않습니다. 결정이 바뀌면 새 ADR 을 추가하고 기존 ADR 의 Status 를 `Superseded by NNNN` 으로 표시합니다. 오탈자·표현 수정은 Revision History 에 남깁니다.
- 본문은 self-contained 로 작성합니다 — 팀 내부 문서나 비공개 노트를 읽어야만 이해되는 표현을 쓰지 않습니다.
