# Tracktory

> AI 기반 자율전공 학습경로 추천 시스템

관심사만 있고 진로·전공이 불명확한 학생에게, AI 추천을 통해 **직무 → 전공/트랙 → 교과목**으로 이어지는 맞춤형 학습경로를 제공하는 모바일 플랫폼.

## Architecture

```
React Native App
       ↓
Spring Boot (트랜잭션 · 인증 · 도메인)    ← tracktory-server
       ↓
FastAPI (AI 중계)                         ← tracktory-ai
       ↓
LangGraph 추천 워크플로 + LightRAG + LLM
```

## Repositories

| Repository | Role | Stack |
|---|---|---|
| **[tracktory](https://github.com/Tracktory/tracktory)** | 프로젝트 허브 · 기술 문서 | - |
| **[tracktory-ai](https://github.com/Tracktory/tracktory-ai)** | AI 추천 파이프라인 · 중계 서버 | Python 3.13 · FastAPI · LangGraph |
| tracktory-server *(예정)* | 메인 백엔드 | Spring Boot |

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

- **제품 요구사항(PRD) · 기능명세**: Notion (팀 내부)
- **기술 문서**: [`docs/`](./docs/) (아키텍처, API 명세 — 구현 진행에 따라 추가)
