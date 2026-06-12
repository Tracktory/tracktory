# 0001 — 서비스 간 내부 인증: Preshared Token 헤더 + 사용자 식별 Envelope

## Status

Accepted (2026-05-26 결정, 양 서비스 구현 완료)

## Context

Tracktory 는 React Native 앱 → Spring Boot 메인 백엔드 → FastAPI AI 서버의 3계층 구조입니다. 사용자 인증(JWT 검증)은 Spring Boot 가 전담하고, FastAPI AI 서버는 Spring Boot 만을 호출자로 갖는 내부 서비스입니다.

이 구조에서 Spring Boot → FastAPI 내부 HTTP 호출을 어떻게 인증할지 결정이 필요합니다. 내부 호출을 무인증으로 두면 내부 네트워크 침투(SSRF, lateral movement) 한 건으로 AI 서버 전체가 노출됩니다. 동시에 AI 서버가 사용자별 기능(추천 이력, 캐싱, 트레이싱)을 제공하려면 "어느 사용자의 요청인가"가 내부 호출에도 안전하게 전파되어야 합니다.

팀은 4인 캡스톤 규모이고 MVP 일정이 짧아, 운영 부담이 큰 인증 인프라를 도입할 여력이 없습니다.

## Decision Drivers

- **구현·운영 비용**: 소규모 팀이 인증서 발급·rotation 같은 운영을 감당할 수 없음.
- **사용자 신원의 안전한 전파**: AI 서버의 사용자별 캐싱·이력·트레이싱이 식별자에 의존.
- **마이그레이션 비용**: production 전환 시 인증 방식을 갈아끼울 수 있어야 함.

## Decision

**1. 내부 호출 인증 — preshared token 헤더.**
256-bit 난수 preshared API key 를 `X-Internal-Token` HTTP 헤더로 전달합니다. Spring Boot 는 HTTP 클라이언트의 기본 헤더로 자동 부착하고, FastAPI 는 timing-safe 비교(`secrets.compare_digest`)로 검증하며 AI 라우터 전체를 검증 의존성으로 게이트합니다. 토큰은 환경변수로 관리하고(실제 값은 gitignore, example 파일만 커밋) 저장소에 커밋하지 않습니다.

**2. 사용자 식별 전파 — envelope 헤더 필수.**
Spring Boot 가 JWT 검증 후 추출한 사용자 식별자를 `X-User-Id` 헤더로 전달합니다. FastAPI 는 이 헤더를 **필수**로 요구하고(부재·공백 시 422 거부) 값은 추가 검증 없이 신뢰합니다. 더미 식별자 fallback 을 두지 않고 처음부터 필수 계약으로 강제해, 사용자별 캐싱·이력·트레이싱이 식별자 없이 적재되는 경로를 차단합니다.

**3. 네트워크 격리 전제.**
FastAPI 는 내부 네트워크에만 노출하고 public 인터넷 진입을 금지합니다(컨테이너 환경은 내부 네트워크만 join, 단일 VM 은 internal IP 바인딩). 이 전제는 envelope 신뢰 모델의 prerequisite 입니다 — 깨지면 토큰을 획득한 외부 호출자가 `X-User-Id` 를 임의로 부착해 다른 사용자 행세를 할 수 있습니다.

## Consequences

### Positive

- 검토한 대안 중 구현·운영 비용이 가장 낮습니다(헤더 부착 + 헤더 검증). OWASP Microservices Security Cheat Sheet 는 서비스 간 인증 패턴으로 mTLS 와 token-based 인증을 설명하며, 외부 사용자 토큰을 내부 서비스에 그대로 재사용하는 방식은 위험하다고 봅니다. 본 결정은 그 방향성과 맞추되 MVP 규모에 맞게 보안 토큰 서비스 대신 환경변수 기반 preshared token 으로 단순화한 절충안입니다.
- 헤더 기반 인증 경계를 유지한 채, 이후 mTLS 또는 OAuth2 client credentials 로 승격하기 쉽습니다.
- 사용자 식별이 필수 계약이므로 인증 연동 시점에 제거할 임시 코드가 없습니다.

### Negative

- preshared key 는 rotation 이 수동입니다. production-grade zero-trust 가 아닙니다.
- 네트워크 격리 전제가 배포 환경마다 운영 점검 항목으로 남습니다.

### Neutral

- production 진입 시 mTLS(service mesh 또는 자체 cert 운영) 또는 OAuth2 client credentials 로의 승격을 검토합니다. 두 방식 모두 본 결정의 헤더 검증 레이어 위에 얹을 수 있습니다.

## Alternatives Considered

| ID | Description | Why rejected |
|----|-------------|--------------|
| ALT-1 | mTLS 자체보다 인증서 발급·trust | bootstrap·revocation·rotation 운영 부담이 MVP 범위에 비해 큽니다. |
| ALT-2 | OAuth2 client credentials grant | Keycloak/Auth0 등 auth server 셋업 비용이 규모 대비 과투자. |
| ALT-3 | Service mesh (Istio/Linkerd) | k8s 운영 인력 없음. |
| ALT-4 | 내부 네트워크 신뢰 (무인증) | SSRF·lateral movement 한 건으로 무너짐. 내부 서비스 직접 접근을 막는 추가 인증이 없어 SSRF·lateral movement 시 피해 범위가 커집니다. |
| ALT-5 | 사용자 JWT 를 FastAPI 가 재검증 | 게이트키퍼(Spring Boot) 역할 무력화 + 검증 로직 이중화. 외부 사용자 토큰을 내부 서비스 계약에 직접 결합해 공격면과 검증 로직 중복을 늘립니다. OWASP는 외부 access token 재사용보다 내부용 identity representation 전파를 권장합니다. |
| ALT-6 | 사용자 식별자를 MVP 동안 더미 값으로 고정 | 모든 요청이 단일 익명 버킷으로 묶여 사용자별 캐싱·이력·트레이싱 분리 불가. 필수 헤더 계약으로 처음부터 강제하는 쪽이 이후 제거 비용 0. |

## References

- OWASP Microservices Security Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Microservices_Security_Cheat_Sheet.html
- Chris Richardson, microservices.io — Service-to-service security patterns

## Revision History

- 2026-06-12: 최초 작성 (내부 인증 결정 2026-05-26, 사용자 식별 필수화 확정 2026-05-30).
