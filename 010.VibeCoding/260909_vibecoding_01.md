
## 과제

* 예제
    - https://github.com/stepanowon/budget-book
    - https://github.com/stepanowon/team-caltalk
    - https://github.com/stepanowon/multichat-web

* 강의학습창
    - http://10.4.1.100:5000
    - PW: multi0907

## 실습

* 목표
    - to do list
        - from - until 입력
        - to do를 기간에 따라 상태를 볼 수 있어야..
        - to do list의 category도 있음
    - 사용자 인증
    - 메모 기능

* 기술 spec
    - frontend: react, typewrigth???
    - backend: 무료로 배포가능한 node.js express framework
    - DB: supabase


## Step

1) github에서 새로운 project 생성
    - .gitignore를 "node"로 선택 (자동생성)
2) command 창에서 해당 project clone
3) VScode에서 "폴더열기"
4) VScode에서 command창 열어서 claude 실행
    - 기본 template (files/참고스크립트.zip) 적용
5) subagent 규칙 반영
```
    - .mcp.json
    - .claude/
        \_ agents/
            \_ { api-designer, backend-expert, business-analyst, document-engineer, frontend-expert, security-auditor, technical-writer, ui-designer}.md
        \_ skills
            \_ develop-backend/SKILL.md
            \_ develop-frontend/SKILL.md
```
6) 불필요한 MCP 삭제
    - ".mcp.json" 편집해서 사용할 MCP 남겨두기

7-1) CLAUDE.md 작성

```
## 반드시 준수할 규칙

- 모든 대화는 한국어로만 진행할 것
- 지시하지 않은 작업은 수행하지 말것
- 오버엔지니어링 금지

## 프로젝트 디렉토리 기본 구조

- 백엔드: /backend
- 프런트엔드: /frontend
- 설계, 요구사항 문서: /docs

### 더 자세한 디렉토리 구조 정보는 프로젝트 구조 설계 원칙 문서를 참조합니다.
```

7-2) "prompts/domain_prompt.md"를 작성하여 "/docs/1-domain-definition.md"을 AI에게 생성하도록 요청
    7) 도메인 정의서 작성 (option)
        - 상황: 사용자 요구사항이 없을때, 개발자가 혼자 개발할때는 없어도 됨
        - 사용자-개발자 간 문서
        - 문제 정의
        - 비지니스 도메인(문제 영역) 정리: **범위/개념/규칙/경계**를 명확히 기술
        - BRD(Business Requirement Document) 작성
        - "1-domain-definition.md"에 기술
        - 작성된 것을 "개발자"가 검토
            - AI로 문서들을 수정하도록 해라
        - 1인 개발시 꼭 필요한 문서는 아님


8) PRD
    - 개발자-AI간 문서 (필수)
    - 배포환경을 고려해야 함
        - 예) 주소, 키 등을 환경변수로 분리해야 한다.
    - PRD안에 "사용자 시나리오" 내용이 있다면 별도의 문서로 분리해라
        - "사용자 시나리오"는 추후 시험에서 활요할 수 있다.

9) PRD -> User Scenario
    - 시나리오가 생성됨
    - 시나리오별로 UI가 필요 -> Wireframe으로 설계 (Figma...)
        - 시나리오별 기능은 PRD에 기술
    - ex. docs/3-user-scenario.md

10) 기술 Architecture 설계
    - 사용자에게 설명하기 위한 그림을 기반으로 기술된 문서
    - 1인개발시 없어도 됨
    - project 구조 원칙 : 있으면 좋다.
    - layer/tier 등의 구조를 알려주면 설계를 더 분명히 할 수 있다. => 지침으로 작성

11) Diagram
    - 필요시 mermaid처럼 Diagram을 추가할 수 있다
    - (결과) ex. 6-arch-diagram.md


12) ERD (Entity Relationship Diagram)
    - DB의 저장구조 정의, 설계
    - Backend와 DB
        - 용어: ERD, index, entity, ...
    - 도메인 정의서의 "핵심 엔티티"를 기반으로 설계
    - ERD를 기반으로 SQL 문서(DDL) 생성
        - 생성된 결과 문서를 DB 생성하는 MCP를 이용해서 AI에게 생성을 요청

13) 작업 분할/실행 (**중요**)
    - (p.25) 요청하는 프롬프트를 잘게 명료히 분할 (WBS: Work Breakdown Structure)
    - 작업 분할, 분할된 entity간 선/후 연결, 일정 수립이 가능함
    - (결과) docs/8-plan.md

14) Swagger
    - OpenSource framework
    - Frontend/Backend간 
    - Backend의 API(REST API)의 규격을 설계할때 사용
    - ERD가 생성된 후에 가능 (Frontend/Backend간 주고받는 데이터는 보통 DB에 저장하는 데이터 구조와 연관됨)
    - 주요기능
        - Swagger Spec
        - Swagger UI: Spec의 예시로 제공하는 UI(사용자/개발자용, AI는 필요없음), 개발자가 시험할때 활용할 수 있음
        - Swagger Codegen:
    - 예) https://contactsvc.bmaster.kro.kr/
    - swagger spec(ex. swagger.json)이 있다면
        - frontend/backend 개발을 동시에 진행할 수 있음
        - swagger spec으로 Mockup Server을 구축하면 frontend 개발이 가능
        - 그동안 backend를 동시 개발 가능
        
15) 1차 설계 마무리
    - 현재까지 작업된 문서 정합성 검토
    - main branch에 commit 후 push 요청

16) swagger를 이용하여 mockup server UI 시험
    - ex) stoplight Prism CLI
    - ex) https://editor.swagger.io/

* Hermes
    - AI model
