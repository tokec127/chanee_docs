# 8. 배포 

* 5일자 교육
    - 2026.9.11.(금)
    - 멀티캠퍼스 선릉역
    - 목표: 배포 및 관리


## 견고한 애플리케이션 개발

* 기본원칙
    - 프로젝트 구조 설계 원칙 최상위 원칙에 추가
    - ex. 5-project-principle.md
```
 > SOLID 원칙을 준수
 > Clean Architecture 준수
```

* SOLID 원칙
    - 객체지향 언어 사용시 추가할 기본원칙
    - (원칙1) 단일책임원식 (SRP: Single Responsibility Principle)
        - 코드 재활용, 단위시험 용이, loosely coupled된 기능은 side-effect가 적다
    - (원칙2) 개방, 폐쇄 원칙 (OCP: Open Closed Principle)
        - 설계도를 기반으로 제품을 생산 (ex. Class를 기반으로 Object를 생산)
        - 기존 클래스를 변경할 수 없으나, 상속해서 새로운 기능을 추가할 수 있다.
    - (원칙3) 리스코프 치환원칙 (LSP: Liskov Substitution Principle)
        - 자식 클래스는 부모 클래스를 대체할 수 있어야 함
        - 상속 관계가 아닌 클래스를 상속 관계로 설정하면 안됨
    - (원칙4) 인터페이스 분리 원칙 (ISP: Interface Segregation Principle)
        - 특정 클라이언트는 자신이 사용하지 않는 인터페이스(ex.swagger, API 규격)를 구현하면 안된다.
    - (원칙5) 의존성 역전 원칙 (DIP: Dependency Inversion Principle)
        - 상위 모듈이 하위 모듈에 의존하면 안됨.
        - 추상 클래스나 인터페이스에 의존
        - 하위 모듈이 변경되더라도 상위 모듈 변경이 요구되면 안됨


* Clean Architecture
    - 4개의 계층으로 구조화하여 상위에서 하위로 의존성을 둔다.
        - 4계층 (최상위): framework
        - 3게층 : Interface Adapters: 외부계층에서 사용할 수 있는 형태로 데이터를 변환
        - 2게층 : Use Case, 비즈니스 로직을 처리하는 계층
        - 1게층 : Entity, 시스템의 비즈니스 규칙을 캡슐화 및 저장
    - 의존성 역전 원칙과 유사


## 시험

* 브라우저에서 시험
    - 시나리오를 기반으로 E2E 시험
    - 완성도를 높이려면 수동 시험도 필요
    - playwright MCP 사용 (browser 뿐만 아니라 Android 에서도 가능)
        - Android studio에서 UI를 조작할 수 있는 MCP가 존재
    - 시험완료 후, test report를 받아야 함
        - 시험도중 문제 발생시 디버깅해서 완료
        - report에 추가할 주요화면을 capture함 (playwright MCP에게 요청할 수 있다.)
    - (HOW)
        - 명령 프롬프트에 "[E2E 시험프로그램]"를 입력한다.
    - (TIPS)
        - MCP 연결이 안될 경우
        - "[Prompt-02]" 참고

```
[E2E 시험프로그램]
현재 백엔드와 프론트엔드가 모두 개발서버로 실행중이야. 
playwright-mcp를 이용하고 시나리오를 참조해 통합테스트를 수행해줘.
- 테스트 결과는 test/e2e 폴더에 저장해줘
- 주요화면과 엣지케이스에 대한 테스트에 대해 스크린 캡처해줘.
- 스크린 캡처는 테스트 리포트에 포함해줘.
- 모바일 브라우저, 데스크탑 브라우저 모두 시험해줘
```

```

## Supabase

* 개발환경

```
          | FrontEnd | BackEnd | DB 
    |--|--|--|
    Local |                    | DB-string     |
          |                    | CORS_ORIGIN   |
          |  5173              | 3000          | 5432
    Prod. | Vercel             | Vercel (or Railway) | Supabase-Postgres

```

* 배포에 사용할 플랫폼
    - (무료) ex. Vercel (node.js 지원)
    - (유로) ex. Railway (spring boot 지원)

* 배포전 준비사항
    - Supabase의 DB 생성 (키 확인)
    - frontend/backend의 .env.product의 환경변수 설정 (vercel.app)

* Supabase에서 project 생성
    - 내부적으로 storage를 유로로 제공
    - Supabase에 new project 생성 (DB password를 기억해야 함)

* Supabase로 전달
    - 전제: 문서 정합성 검토 후 schema.sql 업데이트
    - (방법1) shcmea.sql 파일을 "SQL Editor"@Supabase에 넣어서 적용
    - (방법2) postgresql MCP로 가능

* 관련 기술 스택
    - frontend 기술: react, next.js, vue
    - 가능한 배포: node.js, python, go, ruby

## VERCEL

* vercel로 배포될 경우
    - frontend 주소: $hostname.vercel.app
    - backend 주소: $hostname-be.vercel.app
        - ex) $hostname=todomemo-tokec ("_"는 입력에서 사라짐, 대소문자 구분안함)
    - Naming rule
        - PascalCasing
        - camelCasing
        - kebob-casing
        - snake_casing

* vercel
    - serverless functions
        - 실시간 서비스가 아니라 polling 방식으로 구동
    - 배포전 checklist
        - 환경변수: Settings-Environment Variables 클릭 후 환경변수 입력
        - 입력값
            - Environments: 
            - Key-Value

* vercel for github
    - vercel에 내장된 github 연동 배포 기능
    - (동작) github에 code push와 함께 자동으로 vercel에 배포
        - main/master branch에 코드가 합병되면 production 환경으로 배포
        - other branch는 preview 환경으로 배포
    - (장점)
        - 설정이 간단
        - CI/CD 파이프라인 구성이 필요없음

## 배포 절차
* backend 배포
    1. login with github account @ vercel
    2. create new project, set project name with $hostname written at ".env.product"
    3. import ".env.product"
    4. deploy
        - 배포시 vercel MCP을 활용하면 git commit 할때마다 재배포가 자동으로 실행됨
        - (배포실패 원인1) 배포자체 실패
            - 배포시 필요한 패키지를 같이 다운받아서 설치됨
            - 오류 발생시 **로그** 메뉴에서 로그를 copy하여 AI에게 수정 요청
            - AI가 수정한 후에 다시 배포해줘 요청하면 됨
        - (배포실패 원인2) 실행시 오류
    5. check backend status 
        - "http://$hostname-be.vercel.app/health" ("[prompt4] 확인")
        - 성공시 화면에 {"status": "ok"} 표시

```
 > 백엔드의 health check endpoint 주소를 알려줘

GET http://localhost:3000/health

backend/src/app.js에 등록된 라우트로, 포트는 .env의 PORT(기본 3000)를 따른다.
```


* frontend 배포
    1. create new project @ vercel
    2. set ENV_VIRONMENTS (i.e., VITE_API_BASE_URL=https://$(hostname)-be.vercel.app)
    3. deploy

* frontend 접속
    1. HTTP -> Get/login
    2. FE에서 404 not found 응답 (실제 배포 파일에는 login파일/폴더 없음)
        \_ http 서버에는 default fallback UI를 지원함
        \_ 1. 해당 문서가 없다면 기본문서 index.html을 전달
        \_ 2. 해당 문서가 없다면 js을 다운받아서 처리

```
> vercel에 frontend 앱을 배포했어. frontend fallback UI 설정을 해줘.
```

## CI/CD

* CI/CD
    - Continuous Integration (CI): 지속적 통합
    - Continuous Delivery (CD): 지속적 전달
    - Continuous Deployment (CD): 지속적 배포
    - 자동화 도구
        - Jenkins
        - Github Action
        - Gitlab CI


* github action 사용
    - github action을 단계별로 구분
        - development, preview, deploy
    - 단계에 따른 action 스크립트를 구분할 수 잇음
        - {frontend, backend}-preview.yaml
        - {frontend, backend}-prod.yaml
    - 입력값
        - 필요정보: 
            - (Vercel정보) Vercel 토큰, 조직 ID, Project ID (backend, frontend)
            - (Github 반영) 
                - VERCEL_TOKEN
                - VERCEL_ORG_ID
                - VERCEL_PROJECT_ID_BACKEND
                - VERCEL_PROJECT_ID_FRONTEND
    - .github/workflows 폴더에 해당 파일 저장
        - backend-prod.yaml
        - backend-preview.yaml
        - frontend-prod.yaml
        - frontend-preview.yaml
    - 기존 vercel for github 배포 중단 설정
        - 파일: {backend, frontend}/vercel.json
        - 추가

```
        "git": {
        "deploymentEnabled": false
        }
```
    - 반영: `> commit & push`
    - 실행
        - 소스 변경
        - claude에게 "> commit & push"
        - github에 자동 반영 => github action에서 실행
