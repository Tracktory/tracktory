# ERD 작업 방식

## 기본 흐름

1. 본인 Primary 영역은 dbdiagram.io 또는 vscode dbdiagram extension 에서 자유롭게 작업
2. `.dbml` 텍스트로 추출
3. tracktory 레포에 PR

vscode extension 으로 `.dbml` 을 직접 편집하면 git 추적 + 병렬 작업이 자연스럽습니다.

## 겹침 영역 순서

`user` ↔ `user_profile` 은 의존성이 있어 순차 진행:

1. `user`: 전종현
2. `user_profile`: 정종진 (전종현 작업 후)
3. Secondary 리뷰어가 누락 필드 코멘트
4. 이재원이 최종 확인

## 다른 사람 Primary 테이블

직접 수정 X. 멘션 · 코멘트 · 디스코드 ERD 스레드로 요청합니다. Primary / Secondary 매트릭스는 [OWNERSHIP.md](./OWNERSHIP.md) 참조.

## 정규화 / 반정규화 결정 공유

정규화 분리 또는 의도적 반정규화 (중복 컬럼 · 계산 컬럼 · 캐시 컬럼 등) 결정 시:

- 디스코드 ERD 스레드에 **사유와 함께** 공유
- 추후 ADR 화 또는 코드 리뷰의 근거로 사용

## 컨벤션

테이블 · 컬럼 명명, 타입, 공통 컬럼 규칙은 [노션 ERD 작성 컨벤션](https://www.notion.so/ERD-364335044a4480878c70fcc7e60bdc6d) 단일 출처를 따릅니다.
