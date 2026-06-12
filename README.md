# Tracktory

> AI 기반 자율전공 학습경로 추천 시스템

관심사만 있고 진로·전공이 불명확한 학생에게, AI 추천을 통해 **직무 → 전공/트랙 → 교과목**으로 이어지는 맞춤형 학습경로를 제공하는 모바일 플랫폼.

<img src="https://raw.githubusercontent.com/Tracktory/.github/main/profile/app-ui.svg" alt="Tracktory 앱 주요 화면" width="100%" />

## Features

- **직무 추천** — 관심사·이수 이력 등 온보딩 입력을 의미 검색으로 매칭해 적합 직무를 추천
- **트랙 조합 추천** — 직무 도달에 필요한 트랙 조합을 시너지 점수로 평가해 추천, 조합 다양성 보장
- **학습 로드맵** — 선수 관계를 지키는 학기별 수강 로드맵 (기초 → 핵심 → 응용 → 산학 단계)
- **추천 근거 설명** — 검색된 근거 컨텍스트를 기반으로 LLM 이 추천 이유를 자연어로 설명
- **AI 챗봇** — 진로·트랙 관련 질문에 응답하는 대화형 에이전트

추천 파이프라인의 상세 설계는 [tracktory-ai 문서](https://github.com/Tracktory/tracktory-ai/tree/main/docs)를 참조.

## Architecture

```
React Native App                          ← Frontend
       ↓
Spring Boot (트랜잭션 · 인증 · 도메인)    ← Backend
       ↓
FastAPI (AI 중계)                         ← tracktory-ai
       ↓
LangGraph 추천 워크플로 + LightRAG + LLM
```

## Repositories

| Repository | Role | Stack |
|---|---|---|
| **[tracktory](https://github.com/Tracktory/tracktory)** | 프로젝트 허브 · 교차 기술 문서 (ERD · ADR · API 명세) | - |
| **[Frontend](https://github.com/Tracktory/Frontend)** | 모바일 앱 | React Native |
| **[Backend](https://github.com/Tracktory/Backend)** | 메인 백엔드 — 인증 · 트랜잭션 · 도메인 | Spring Boot |
| **[tracktory-ai](https://github.com/Tracktory/tracktory-ai)** | AI 추천 파이프라인 · 중계 서버 | Python 3.13 · FastAPI · LangGraph |

## Tech Stack

- **Frontend**: React Native
- **Backend**: Spring Boot
- **AI Relay**: FastAPI + LangGraph
- **Retrieval**: LightRAG (RAGFlow 내 구현) + LLM

## Team

| Member | Role |
|---|---|
| 이재원 (Lead) | 추천 시스템 · LangGraph 파이프라인 |
| 정종진 | 트랙 시너지 알고리즘 · 직무-역량 매핑 |
| 박성훈 | 데이터 처리 · 프론트엔드 |
| 전종현 | 논문 · 임베딩/RAG 최적화 |

## Documentation

- **ERD**: [`docs/erd/`](./docs/erd/)
- **서비스 간 ADR**: [`docs/architecture/adr/`](./docs/architecture/adr/)
- **AI 파이프라인 설계**: [tracktory-ai/docs](https://github.com/Tracktory/tracktory-ai/tree/main/docs) (파이프라인 구조 · 트랙 시너지 설계 · ADR)
- **제품 요구사항(PRD) · 기능명세**: Notion (팀 내부)
