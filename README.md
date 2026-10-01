# Snipy — 박장현 담당 업무

Snipy(URL 단축·마케팅 통계 서비스) 4인 팀 프로젝트(멋쟁이사자처럼 DevOps 7기 3팀, 2026.08)에서 통계 API, 통계 대시보드(프론트엔드), 프론트엔드 정적 호스팅 인프라를 담당했습니다.
코드는 팀 저장소에 있습니다: [Team-likelion-2nd-Project/likelion-devops-7th-team03](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03)

| 영역 | 내용 | PR |
|------|------|----|
| DB | `click_events` 마이그레이션 V1~V4: FK 제거, 월별 RANGE 파티셔닝, 쿠키 기반 visitor_id. 이후 팀이 클릭 로그를 Kinesis → Firehose → S3 → Athena 방식으로 옮기면서 V5에서 이 테이블은 제거됨 | [#14](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/14) |
| 통계 API | 기간별·링크별·유입경로별 조회 API와 집계 Repository, 서비스·컨트롤러 테스트 | [#56](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/56) |
| 리팩터링 | 소유권 검증을 JWT 클레임(`@CurrentUser`) 방식으로 통일 | [#68](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/68) |
| 프론트엔드 | React(Vite) 구성, 링크 CRUD·통계 API 연동, 인증 라우팅, 통계 대시보드 | [#74](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/74), [#84](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/84) |
| 실시간 통계 | 배치(Athena → MySQL)는 전일까지만 확정하므로 오늘 하루만 Redis 값(클릭 카운터, 방문자 HyperLogLog)으로 표시, 새로고침 버튼 | [#153](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/153) |
| 봇 필터 | CloudFront가 User-Agent를 오리진에 전달하도록 수정, 실시간 통계도 배치와 같은 기준(`is_bot=false`)으로 맞춤 | [#183](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/183) |
| 인프라 | 프론트엔드 S3(OAC) + CloudFront 정적 호스팅 Terraform 모듈, dev/prod 분리 | [#129](https://github.com/Team-likelion-2nd-Project/likelion-devops-7th-team03/pull/129) |
