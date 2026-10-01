# Snipy — 박장현 담당 업무 정리

Snipy(URL 단축·마케팅 통계 서비스) 4인 팀 프로젝트(멋쟁이사자처럼 DevOps 7기 3팀, 2026.08)에서 **통계 API / 통계 대시보드(FE) / 프론트엔드 정적 호스팅 인프라**를 담당했습니다.
코드는 팀 저장소에 있고, 이 문서는 제 기여 내역을 PR 링크로 정리한 것입니다. 팀 저장소: [Team-likelion-2nd-Project/likelion-devops-7th-team03](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03)

## 요약

| 영역 | 내용 | 관련 PR |
|------|------|---------|
| DB | `click_events` 등 스키마 마이그레이션 V1~V4 (FK 제거, 파티셔닝, 쿠키 기반 visitor_id), 통계 쿼리·검증 SQL | [#14](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/14) |
| 통계 API (BE) | 기간별 / 링크별 / 유입경로 통계 조회 API, 일별·차원별 집계 Repository | [#56](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/56) |
| 리팩터링 | `StatsService`를 `@CurrentUser` 패턴으로 통일, Auth/Stats 컨트롤러·서비스 테스트 재작성 | [#68](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/68) |
| 프론트엔드 | React(Vite) 초기 구성, 링크 CRUD·통계 API 연동, 인증 라우팅, 통계 대시보드 | [#74](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/74), [#84](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/84) |
| 실시간 통계 | 대시보드에 "오늘" 데이터 실시간 반영 + 새로고침, 봇 트래픽 필터링, CloudFront User-Agent 전달 | [#153](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/153), [#183](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/183) |
| 인프라 (Terraform) | 프론트엔드 S3 + CloudFront 정적 호스팅 모듈 (dev/prod 적용) | [#129](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/129) |

## 상세

- **DB 설계 ([#14](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/14))** — `click_events` 월별 RANGE 파티셔닝(V3, PK를 `(id, clicked_at)` 복합키로 변경), 파티셔닝 선행조건인 FK 제거(V2), 순 방문자 집계를 위한 쿠키 visitor_id(V4)를 마이그레이션 스크립트로 관리.
- **통계 API ([#56](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/56), [#68](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/68))** — 기간·링크·유입경로 단위 조회. 소유권 검증은 JWT 클레임(`@CurrentUser`)으로 처리하고 JOIN을 쓰지 않는 팀 설계 원칙을 따름. 서비스·컨트롤러 통합 테스트 작성.
- **"오늘(잠정)" vs "확정" 통계 ([#153](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/153))** — 배치(Athena → MySQL)는 전일까지만 확정하므로, 오늘 하루만 Redis 실시간 값(클릭 카운터, 방문자 HyperLogLog)으로 대체해 표시. 서비스 단위 테스트 추가.
- **프론트엔드 ([#74](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/74), [#84](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/84))** — 링크 생성/목록/통계 화면, fetch 기반 http 클라이언트(`api/http.js`), 보호 라우트.
- **정적 호스팅 ([#129](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/129))** — S3(OAC) + CloudFront Terraform 모듈, 환경별 outputs 분리.
- **봇 필터 ([#183](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/183))** — CloudFront가 User-Agent를 오리진에 전달하도록 수정하고, 실시간 통계에서도 봇 요청을 제외해 Athena 배치(`is_bot=false`)와 집계 기준을 일치시킴.
