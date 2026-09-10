# REVIEW, Backend 마무리

> 26.9.10.(목)

## Review

* 문서 종류
    - 도메인 정의서: 사람간 소통을 위한 초기 문서
    - **[PRD]** : Product Requirement Document
        - 개발자-AI간 요구사항 문서, 
        - 도메인 정의서로부터 확대가능, 
        - 기술스택, 비기능, 성과지표, 제약사항 등이 기술
        - 기본 골격이며, 일의 시작
    - 기능요구사항 정의서: PRD에 기술하기 어려운 detail한 내용을 추가
    - 사용자 시나리오(스토리):
        - 사용자별 시나리오를 AI로 최초 생성
        - 필요시 사용자와 논의
    - wireframe
        - 사용자 시나리오에 따른 화면 구조, 화면 전환 등을 표시
        - 대표적 도구: Figma, 
    - 프로젝트 구조원칙
        - 기능 요구사항 정의서 이후에 작성해도 가능
        - AI가 코딩하기 위한 기본 원칙
            - 폴더 구성, Tier, Layer 구조, Naming 원칙, 테스트 정책, 설정/보안/운영 정책
    - architecture diagram
        - 프로그램의 흐름, 절차도
    - **[ERD]** : Entity Relationship Diagram
        - 데이터 저장 구조 (DB, Backend 등의 기능과 구조에 결정적인 영향을 줌)
    - **[PLAN]** :
        - 작업을 분할, 절차, 완료조건, 선후행 관계를 기술함
        - AI가 plan 문서를 참고로 코드를 생성하고, 시험, 완료 또는 그 과정을 수차례 반복
    - CLUADE.md
        - 예제: https://github.com/multica-ai/andrej-karpathy-skills/blob/main/CLAUDE.md

* 디버깅
    - AI가 못하는 디버깅 부분
        - 문서, 설계, Architecture가 구조화되지 않을 경우 문제가 해결되지 않으면서 토큰만을 소비
        - 로깅이 부족한 상태
            - "주요 부분에 로깅을 남겨라"를 develop-{backend,frontend}.skill에 추가해라
            - 보통 로그는 파일시스템에 남긴다. 
            - 하지만 배포환경에 따라 파일시스템에 남길수 없는 경우가 있음(ex. vercel) > 터미널에 로그를 남긴다.
        - 에러 로그는 AI도 남김 (compile error, runtime error, logical error)
        - 에러 로그
            - backend 서버를 수동으로 재시작
            - 콘솔 로그를 AI에게 자동으로 debugging
    - 대표적인 오류
        - 숫자의 type (결과는 integer, 사용시 string, 또는 반대)
        - 시각 (UTC, GMT)

* 다국어처리
    - AI가 best

* 개발모드 적용
    - 필요성
        - backend 수정 후 재시작 필요
        - 로깅의 AI 분석
    - 적용 prompt

```
> backend에 개발모드를 추가해줘
> npm run dev 명령어로 백엔드 API 서버를 네가 구동해줘
> 백엔드 API 서버를 /tasks 명령어로 관찰할 수 있도록 실행해줘.
> /tasks
```

* CORS 설정
    - frontend가 접속할 정보를 추가
    - CORS 미들웨어 설치
    - backend/.env에 CORS관련 환경변수 추가 (CORS_ORIGIN)

* 자동화 시험
    - 아래 프롬프트 사용
    - 시험 스크립트 생성(ex. tests/e2e.sh). 추후 bash에서 해당 스크립트를 실행

```
❯ 사용자 시나리오를 기반으로 백엔드 API에 대한 자동화된 E2E 테스트를 수행해줘.
  curl 도구를 사용해서 테스트하면 돼.
```

