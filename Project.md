## 프로젝트 개요

> **프로젝트명 :** 라잇나우 (Rightnow)
>
> **운영 :** [https://aegis.daeryunlaw.com](https://aegis.daeryunlaw.com)
>
> **개발 :** [https://devaegis.daeryunlaw.com](https://devaegis.daeryunlaw.com)

## 프로젝트 목적

> 본 프로젝트는 **법무법인 대륜의 사건 수임부터 상담·배당·수임료 정산·인사 관리에 이르는 업무 전반을 하나의 플랫폼에서 처리하기 위한 사내 통합 업무 시스템(ERP/그룹웨어)**을 구축하는 것을 목적으로 한다.
>
> 이를 통해 부서·지점·직군 간 분산되어 있던 업무 데이터를 표준화된 구조로 통합하고, 사건·의뢰인·구성원 정보를 실시간으로 공유함으로써 업무의 투명성과 생산성을 제고하고자 한다.
>
> 아울러, 본 시스템은 외부 공개 서비스가 아닌 **임직원 전용 내부 업무 시스템**으로서, 접근제어·권한 분리·개인정보 보호에 중점을 둔 **안전하고 신뢰 가능한 로펌 업무 운영 플랫폼** 구축을 지향한다.

**프로젝트 범위**

- 문의·사건·상담 관리 (문의 접수 → 배당 → 상담 → 수임)
- 의뢰인 관리 (CRM)
- 구성원 관리 (인사자료, 부서 매뉴얼, 출입 이력)
- 인사평가 (자가/상향 평가, 심의, 평가 설정)
- 회계 관리 (입금 관리, 매출 관리, 수임료 정산)
- 일정·메시지·쪽지·알림
- 전자결재(EDMS)
- 차량·장비·게시물·공지사항·마케팅
- 외부 연동 (본인인증, 화상회의, 푸시 알림, NaverWorks 알림)

**개발 담당자**

- 제윤태
- 성찬홍
- 이진혁
- 김현태
- 박수현
- 강희진

---

## 2. 프로젝트 진행 현황

| 기능명 | 구분 | 상태 | 우선순위 | 담당 부서 | 비고 |
| --- | --- | --- | --- | --- | --- |
| 문의/사건 관리 | 개발 | 완료 | 상 | 개발팀 | 지속 고도화 |
| 의뢰인 관리(CRM) | 개발 | 완료 | 상 | 개발팀 |  |
| 구성원 관리 | 개발 | 완료 | 상 | 개발팀 | 인사자료 포함 |
| 인사평가 | 개발 | 완료 | 상 | 개발팀 |  |
| 입금/매출 관리 | 개발 | 완료 | 상 | 개발팀 |  |
| 일정/메시지 | 개발 | 완료 | 중 | 개발팀 |  |
| 전자결재(EDMS) | 개발 | 완료 | 중 | 개발팀 |  |
| 부서 매뉴얼 PDF | 개발 | 완료 | 중 | 개발팀 | S3 캐싱, CloudFront |
| 본인인증(Barocert) | 개발 | 완료 | 중 | 개발팀 |  |
| NaverWorks 알림 | 개발 | 완료 | 중 | 개발팀 |  |
| 푸시 알림 | 개발 | 완료 | 중 | 개발팀 | Web Push / Firebase |

---

## 3. 프로젝트 일정 요약

- [x]  ~~요구사항 정의~~
- [x]  ~~데이터베이스 설계~~
- [x]  ~~구축~~
- [x]  ~~내부 테스트~~
- [x]  ~~운영 반영~~
- [x]  ~~기능 고도화 및 안정화~~

---

# 매뉴얼

---

### Frontend

> Language : Next.js 14 (pages router) / TypeScript / React 18

- UI: TailwindCSS, TailAdmin, MUI, Headless UI
- 상태관리: Recoil, Zustand
- 데이터 페칭: React Query, tRPC, SWR, Axios
- 에디터: CKEditor 5, Tiptap
- 차트/시각화: ApexCharts, Nivo, Recharts
- PDF: pdf-lib, @react-pdf/renderer, react-pdf
- 캘린더: FullCalendar

### Backend

> Language : TypeScript (Next.js API Routes)

- Next.js API Routes (모놀리식) + tRPC
- PostgreSQL
- Prisma ORM
- Puppeteer (서버 사이드 PDF 생성)
- AWS S3 / CloudFront (파일 저장 및 배포)

### 외부 연동

- **Barocert** — 본인인증 (전자서명)
- **NaverWorks** — 업무 메신저 봇 알림
- **Firebase Admin** — 모바일 푸시
- **Web Push** — 웹 푸시 알림
- **Google APIs / Google Meet** — 화상회의 링크 생성
- **AWS S3 + CloudFront** — 파일 업로드·배포, PDF 캐싱

### 인프라 / 배포

- **EC2** (Amazon Linux) + **PM2** (클러스터 모드)
- **Nginx** — upstream 블루/그린 스위칭
- **Jenkins** — CI/CD 파이프라인 (브랜치별 자동 배포)
- **블루그린 무중단 배포** — 포트 3000/3001 토글
- **환경 분리:** `develop` → 개발 / `develop-stage` → 스테이지 / `main` → 운영

---

## 2. 프로세스

1. 홈페이지/콜/AI대륜 등에서 고객 **문의(inquiry)** 접수
2. **상담(consult)** 생성 및 담당 변호사 **배당**
3. 상담 진행 → 수임 결정(`consult_decide`)
4. **의뢰인(customer)** 등록 및 사건 연결
5. **입금(payment)** 처리 및 **매출(biz)** 집계
6. 내부 구성원 간 **쪽지(message)**, **일정(schedule)**, **전자결재(edms)** 로 업무 협업
7. 분기별 **인사평가(personnel-evaluation)** 및 성과 관리

---

## 3. 데이터베이스 운영 원칙

### 공통 원칙

- 모든 테이블에 `created_at`, `modified_at`, `deleted_at` 컬럼 포함 (소프트 삭제 원칙)
- 고객 개인정보는 암호화 저장
- 외래키 사용 최소화 (운영 안정성 고려)
- Prisma push 관련 명령 **절대 금지** (스키마 반영은 별도 마이그레이션 절차)

### 주요 테이블

- `admin_user` : 구성원(임직원) 정보
- `customer` : 의뢰인
- `consult` : 상담/사건
- `consult_decide` : 수임 결정
- `inquiry` : 문의 접수
- `payment` : 입금
- `biz` : 매출
- `schedule` : 일정
- `message` : 쪽지
- `group` / `role` / `group_admin` / `group_role` : 조직 및 권한
- `human_resource` : 인사자료
- `department_manual` : 부서 매뉴얼
- `edms_admin` : 전자결재

---

## 4. 개발 규칙

### 코드 작성 원칙

- 민감 정보는 환경변수(`.env.production`)로 관리, 하드코딩 금지
- 패키지 매니저는 **npm** 사용
- Prisma 스키마 변경은 PR 리뷰 후 정해진 절차로만 반영 (`db push` 금지)
- TypeScript strict 원칙 준수
- 공통 API 핸들러는 `verifyHeaders` 래퍼로 인증 검증

### API 설계

- Next.js Pages API Routes 기반 (`pages/api/**`)
- 도메인별 폴더 분리 (예: `pages/api/member`, `pages/api/consult`, `pages/api/personnel-evaluation`)
- 공통 유틸은 `lib/api/**` 에 위치

---

## 5. 배포 및 운영 규칙

### 배포 정책

- 브랜치 기준 자동 배포: `develop` → dev / `develop-stage` → stage / `main` → prd
- Jenkins 파이프라인에서 **블루그린 배포**로 무중단 반영
- 배포 후 헬스체크(`/`)로 HTTP 200~499 응답 확인 후 Nginx 스위칭
- 문제 발생 시 이전 릴리즈(`releases/<timestamp>`)로 롤백 가능 (최근 5개 보관)

### 운영 정책

- 운영 DB 직접 수정 금지 (변경 이력 기록 필수)
- 배포 직후 PM2 로그 및 주요 지표 모니터링
- S3 캐시 경로 변경 시 기존 캐시와의 혼선 주의

---

## 6. 장애 대응 절차

1. `pm2 logs` 로 에러 로그 확인
2. 최근 배포 이력(`/home/ec2-user/lawfirm/releases`) 검토
3. Nginx 현재 포트(`active_port`) 확인
4. 필요 시 이전 릴리즈로 PM2 재시작 → Nginx upstream 스위칭(롤백)
5. 원인 분석 후 재배포

---

## 7. 변경 이력

| 일자 | 변경 내용 | 담당 |
| --- | --- | --- |
| 2026-04-16 | 프로젝트별 매뉴얼 작성 | 개발팀 |
|  |  |  |
|  |  |  |
