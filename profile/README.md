# ShiftMate

매장 운영팀의 반복적인 스케줄/근태/대타 관리 업무를 줄이기 위해 만든 팀 프로젝트입니다.  
`ShiftMate`는 매장 단위 인력 운영을 하나의 흐름으로 관리하는 웹 서비스입니다.

## 1. 프로젝트 한눈에 보기

- 프로젝트 성격: 팀 프로젝트
- 핵심 목표:
  - 수기/메신저 중심 근무 관리 프로세스의 비효율 개선
  - 매장 관리자와 직원 모두가 같은 데이터 기준으로 일정/근태를 관리
- 제공 가치:
  - 자동 스케줄 생성
  - 출퇴근 및 근태 조회
  - 대타 요청/지원/승인 프로세스 통합
  - 월 단위 급여 집계 기반 데이터 제공

## 2. 어떤 문제를 해결했는가

기존 매장 운영에서는 아래 문제가 반복됩니다.

- 스케줄 작성/수정이 수작업이라 시간 소모가 큼
- 대타 요청과 승인 이력이 흩어져 추적이 어려움
- 근태/급여 계산 기준이 사람마다 달라 분쟁 가능성이 생김

ShiftMate는 이를 다음 방식으로 해결합니다.

- 근무 템플릿 + 주간 자동 생성으로 스케줄 작성 부담 감소
- 대타 요청/지원/승인 상태를 한 화면 흐름으로 관리
- 출퇴근 데이터와 스케줄을 연결해 일관된 근태/급여 근거 제공

## 3. 핵심 기능

- 인증/인가
  - 회원가입, 로그인, 토큰 재발급, 로그아웃
  - 비밀번호 재설정 메일 플로우
- 매장/멤버 관리
  - 매장 생성/수정/삭제
  - 사업자번호 검증(Bizno API)
  - 매장 멤버 등록/조회/수정/삭제
- 스케줄 관리
  - 템플릿 생성, 타입(`COSTSAVER`, `HIGHSERVICE`) 관리
  - 주간 자동 스케줄 생성
  - 매장/개인 스케줄 조회
- 근태 관리
  - PIN 기반 출퇴근 처리
  - 일간/주간/개인 주간 근태 조회
- 대타 관리
  - 요청/지원/취소
  - 관리자 승인/거절/관리자 취소
- 급여 데이터
  - 월 단위 근무시간/예상 급여 집계 API

## 4. 기술적 구조

### 전체 아키텍처

```text
[Next.js Frontend :3000]
          |
          v
[Spring Boot Backend :8081 -> container 8080]
          |
   +------+------+
   |             |
[MySQL :33306] [Redis :6379]
```

### 레포 구조

```text
.
├── ShiftMate_Back/       # 백엔드(Spring Boot)
├── shiftmate-front/      # 프론트엔드(Next.js)
└── docker-compose.yml    # 통합 실행 환경
```

### 백엔드 도메인 구조

```text
ShiftMate_Back/src/main/java/com/example/shiftmate/domain
├── auth
├── user
├── store
├── storeMember
├── shiftTemplate
├── employeePreference
├── shiftAssignment
├── attendance
└── substitute
```

## 5. 기술 스택

- Frontend: Next.js 16, React 19, TypeScript, Tailwind CSS 4
- Backend: Java 21, Spring Boot 4.0.2, Spring Security, JWT, JPA
- Database/Cache: MySQL, Redis
- Infra: Docker, Docker Compose

## 6. 실행 방법

### 전체 실행 (권장)

```bash
docker compose up --build
```

- Frontend: `http://localhost:3000`
- Backend: `http://localhost:8081`
- MySQL: `localhost:33306`
- Redis: `localhost:6379`

### 개별 실행

Backend:

```bash
cd ShiftMate_Back
./gradlew bootRun
```

Frontend:

```bash
cd shiftmate-front
npm ci
npm run dev
```

## 7. 환경 변수

백엔드는 `ShiftMate_Back/.env`를 참조합니다.

- `JWT_SECRET`
- `JWT_ACCESS_EXPIRATION`
- `JWT_REFRESH_EXPIRATION`
- `SPRING_DATASOURCE_URL`
- `SPRING_DATASOURCE_USERNAME`
- `SPRING_DATASOURCE_PASSWORD`
- `REDIS_HOST`
- `REDIS_PORT`
- `BIZNO_API_KEY`
- `MAIL_HOST`
- `MAIL_PORT`
- `MAIL_USERNAME`
- `MAIL_PASSWORD`
- `MAIL_FROM`
- `FRONTEND_BASE_URL`

프론트엔드:

- `NEXT_PUBLIC_API_URL` (미설정 시 기본 `http://localhost:8081`)

## 8. 팀 프로젝트 정체성

이 프로젝트는 단순 CRUD 실습이 아니라, **매장 운영의 실제 업무 흐름**을 기준으로 설계된 협업 프로젝트입니다.

- 관리자/직원 관점의 역할 분리
- 인증, 권한, 상태 전이(대타/근태) 같은 실무형 규칙 반영
- 프론트/백 API 계약을 기준으로 병렬 개발


## 10. 문서 링크

- 백엔드 상세 문서: `ShiftMate-Back/README.md`
- 프론트 상세 문서: `Shiftmate-Front/README.md`
- 화면 라우팅: `shiftmate-front/app`
- API 명세 파일: `shiftmate-front/public/API 명세서.pdf`

## 11. 테스트 / 점검

Backend:

```bash
cd ShiftMate_Back
./gradlew test
```

Frontend:

```bash
cd shiftmate-front
npm run lint
```

## 12. 현재 상태 메모

- 일부 프론트 화면(예: 오픈 시프트)은 목업 데이터 기반 UI가 일부 남아 있습니다.
- 핵심 도메인(인증/매장/스케줄/근태/대타)은 백엔드 API 및 프론트 연동 구조가 구축되어 있습니다.
- 카카오 로그인 / 구글 로그인 연동중 입니다. 
