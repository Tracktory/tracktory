# ERD Primary / Secondary 매트릭스

각 테이블군의 1차 담당자(Primary)와 2차 리뷰어(Secondary).

| 테이블군 | Primary | Secondary |
|---|---|---|
| `user` (인증/인가) | 전종현 | 정종진 |
| `user_profile`, `user_*` 관계 (`user_track`, `user_interest`, `user_dev_field`, `user_company_type`, `user_work_value`, `user_tech_stack`, `user_completed_subject`) | 정종진 | 이재원, 전종현 |
| `recommendation`, `recommended_job`, `recommended_track`, `roadmap`, `roadmap_item` | 이재원 | 정종진 |
| `job`, `tech_stack`, `job_tech_stack`, `job_track`, `subject_job` | 이재원 | 정종진 |
| `track`, `college`, `department`, `subject`, `track_subject`, `subject_prerequisite` | 이재원 | 정종진 |
| `interest`, `dev_field`, `company_type`, `work_value` (온보딩 카탈로그) | 정종진 | — |
| `chat_session`, `chat_message` | 정종진 | — |

## 역할 정의

- **Primary**: 해당 테이블군의 `.dbml` 작성 · 수정 책임자. dbdiagram (또는 vscode dbdiagram extension) 에서 작업 후 PR.
- **Secondary**: 누락 필드 · 의문점 · 경계 케이스에 코멘트 남기는 리뷰어. PR 머지 전 한 번씩 봐줌.
- 다른 사람 Primary 테이블은 직접 수정 X — 멘션 · 코멘트로 요청.
