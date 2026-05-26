# Contributing to tracktory

이 레포는 Tracktory 프로젝트의 **허브**입니다. 코드는 없고 교차 기술 문서(ADR, API 명세, ERD)와 포트폴리오 랜딩만 담습니다. 코드 규칙은 각 서브 레포(`tracktory-ai`, `tracktory-server`)의 `CONTRIBUTING.md`를 따르세요.

## 1. 문서가 들어갈 위치

| 종류 | 위치 |
|---|---|
| 아키텍처 결정 (ADR) | `docs/architecture/` |
| 서비스 간 API 명세 | `docs/api/` |
| ERD | `docs/erd/` |

각 디렉토리의 `README.md` / `WORKFLOW.md` / `OWNERSHIP.md`가 세부 규약의 권위입니다.

## 2. 커밋 메시지 규칙

### 포맷

```
{이모지} [Type] 작업 내용
```

### Types & 이모지

| Type | 이모지 | 용도 |
|---|---|---|
| `Feat` | ✨ | 새 문서 · 새 섹션 추가 |
| `Docs` | 📝 | 기존 문서 수정 · 보완 |
| `Fix` | 🐛 | 오탈자 · 잘못된 정보 수정 |
| `Refactor` | ♻️ | 구조 개편 · 파일 이동 |
| `Chore` | ⚒️ | 설정 · 템플릿 · gitignore 등 |

### 예시

```
✨ [Feat] ADR-0001 추천 파이프라인 임베딩 boundary
📝 [Docs] ERD v1 직무-역량 매핑 테이블 보완
🐛 [Fix] API 명세 응답 스키마 필드명 오탈자
⚒️ [Chore] ISSUE & PR 템플릿 등록
```

## 3. PR / Issue

- PR 본문은 `.github/pull_request_template.md` 형식을 따릅니다. 섹션이 비어 있어도 헤더는 유지하고 "(해당 없음)"으로 표시.
- Issue 는 `.github/ISSUE_TEMPLATE/` 의 종류별 템플릿(bug / chore / docs / feature / refactor / other)을 사용합니다.
- Issue 본문은 **What / Why / 완료 조건**만 — 구체 파일 경로 · 함수명은 PR 에서 다룹니다.
