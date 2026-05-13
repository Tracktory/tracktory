# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 이 레포의 역할

Tracktory 프로젝트의 **허브 레포**. 포트폴리오 랜딩 + 교차 기술 문서(서비스 간 아키텍처, API 계약, ADR)를 담는다. 코드는 없음.

## 서브레포 맵

| 레포 | 역할 | 자체 CLAUDE.md |
|---|---|---|
| `tracktory-ai` | LangGraph + FastAPI AI 중계 서버 | 있음 (코드 규칙 권위) |
| `tracktory-server` (예정) | Spring Boot 메인 백엔드 | 생성 예정 |

## 아키텍처 개요

React Native → Spring Boot (트랜잭션·인증) → FastAPI (AI 중계) → LangGraph + LightRAG + LLM

## 문서 정책

- **PRD · 기능명세**: Notion이 원본. 이 레포에는 포함하지 않음.
- **기술 문서**: `docs/architecture/` (ADR, 시스템 설계), `docs/api/` (서비스 간 API 명세). 구현 진행에 따라 추가.

## 소유권 원칙

각 서브레포의 `CLAUDE.md`가 해당 코드의 규칙에 대한 권위. 이 레포는 교차 맥락·기술 문서만 담당하며 서브레포 규칙을 덮어쓰지 않는다.
