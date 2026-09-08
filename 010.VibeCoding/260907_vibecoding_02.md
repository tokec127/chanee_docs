# 개요


## 1. 개요

* Vibe Coding
    - 비추천 예제) https://www.youtube.com/watch?v=bSHLtglLbJA


```
이 문서는 Vibe Coding의 개념, 특징, 적용 방법, 장단점, 미래 개발자 역량, 관련 개발 방법론 및 도구에 대해 설명한다.
* Vibe Coding 개념과 특징
	- 자연어 프롬프트를 통해 AI가 코드를 생성하는 개발 방식이다.개발자는 프롬프트 작성과 피드백에 집중하며, AI가 코드를 자동으로 완성한다.기존 코딩과 달리 설계와 구현의 경계가 모호하며 반복 실험에 적합하다.
* Vibe Coding 등장 배경
	- 대규모 언어 모델 발전과 AI 코딩 도구 등장으로 가능성이 높아졌다.비개발자도 소프트웨어 개발에 참여할 수 있는 사회적 요구가 커졌다.빠른 프로토타입 제작과 실험을 위한 환경이 조성되었다.
* 개발 프로세스와 영향
	- 요구사항 분석, 설계, 구현, 테스트 단계에서 프로토타이핑과 빠른 피드백이 가능하다.설계와 구현이 점진적으로 통합되며, AI와의 대화로 개발이 진행된다.테스트는 AI가 자동 생성하며, 빠른 반복이 가능하다.
* Vibe-Driven Development (VDD)
	- 아이디어부터 테스트, 개선까지 빠른 반복 루프를 강조한다.CI/CD와 연계되어 자동화와 빠른 배포를 지원한다.TDD와 병행하여 신속한 검증과 품질 확보가 가능하다.
* AI-DLC와 역할
	- AI를 SDLC 전 단계에서 중심 동료로 활용한다.인간은 목표 설정과 품질, 보안 책임을 지며, AI는 세부 작업을 수행한다.빠른 프로토타입과 실험에 적합하며, 핵심 시스템에는 전통적 방법 병행이 권장된다.
* 실무 적용 범위와 한계
	- 프로토타입, 개인 프로젝트에 적합하며, 보안이 중요한 대규모 시스템에는 부적합하다.보안, 책임, 복잡한 로직, 성능 최적화는 전통적 방법이 필요하다.AI 의존으로 인한 보안 취약, 유지보수 어려움, 성능 문제 등이 존재한다.
* 미래 개발자 역량과 역할
	- 과거: 코드 작성자, 현재: 문제 해결자, 미래: AI 협력자로 진화한다.자연어 요구 표현 능력과 비판적 사고, 품질 판단이 중요하다.AI와 효과적 협업을 위해 프롬프트 설계와 도구 활용 능력이 필요하다.
* Vibe Coding 핵심 구성요소
	- AI-내재적 사고방식과 자연어 대화 능력.다양한 AI 개발 도구와 표준화된 개발 프로세스.자동화 인프라와 관찰성, 품질관리 도구.체계적 개발 단계와 문서화, 프롬프트 엔지니어링이 포함된다.
```


* 절차
    - UML: Use Case Diagram 
        - actor, use case, action으로 diagram을 구성
        - 목적: ROC 기술할때 많이 사용
        - 향후: Vibe coding을 이용해서 UML, use case 등을 그림
    - 설계
        - peer간 데이터 구조, 절차(Architecture) 등을 AI로 설계
        - DB, frontend-backend간 통신규격
        - GUI layout 및 화면설계, 화면전환 등에 대한 기술(UserStory) 설계 
        - [AI] 프롬프트로 요청 -> 초안 생성 -> 설계자가 검토
    - 최종
        - ROC, 설계는 개발자가 주도 (Application 설계, 요구사항 작성)
        - 코딩은 AI가 ..

* 구현
    - TDD (Test-Driven Development): APP을 개발/시험 단계를 RED/Green/Refactoring 상태로 구분하여 개발
        - Red
        - AI에 의해 TDD 시간이 확 줄어듬
    - SDD (Spec-Driven Development): 설계서 기반 개발 방식
        - Specification: App의 설계내용
    - VDD (Vibe-Driven Development)
        - 설계/개발/시험/배포 등을 AI를 활용하여 수행
        - CI/CD 자동화
            - 배포 설정/초기화 작업은 사람이 해야 한다.
                - ex) github의 mainbranch가 변경되면 자동으로 컴파일, 설치, 배포 등은 자동화 (Kubernetes??)
            - CI(Continuous Integration)
            - CD(Continuous Delievery/Deployment)
            - 자동화된 빌드, 테스트, 배포 파이프라인
            - Code로 클라우드의 인프라 환경을 구축할 수 있음 (설정이 모두 API화되어 있음)
                - IaC(Infrastruture as Code): Ansible, Terraform

* SDLC (Software Development Life Cycle)
    - SDLC: "게획(Plan) - 설계(Design) - 빌드(build) - 테스트(test) - 배포(deploy) - 운용(maintain)" 단계로 구성된 SW 개발주기
    - AI-DLC: AWS에서 만든 AI 협업 개발방법론
    - Man/AI의 역할 구분, 협업 등을 정의
    - 수립/
    - "INTENT.md"를 이용한 AI 코딩 => AI-DLC과 연관
    - 주의
        - 일정 모듈 개발완료시 사람이 해당내용을 검토하는 지점을 잘 세워라
        - 라이센스, 보안 등의 위험성을 사람이 통제
        - Observability: 일정시간동안 수집된 수치화된 정보(자원 사용율 등), 동작중 특정이벤트 발생시 생성되는 로그 정보 검토 => 병목지점, 오류발생 지점을 확인

* 시험
    - AI가 결과물을 자동 테스트함


* 
    - 역할: User, BA, IT Dev. (점차 BA 역할이 없어짐)
    - AI
        - User:
        - IT Dev.: => Full Stack Developer(FSD)
    - FSB(Full Stack Builder) = User + FSD


* 설계 문서 작성
    - SDD(Spec-driven Development): markdown으로 만듬
    - Over-engineering 금지 필요 (저렴하고 성능이 안좋은 AI일수록 이련 경향이 높다)


## Vibe Coding의 주요 구성요소(p.19)

* 기획문서 생성
    - PRD -> BRD
    - Use Case -> Story(or scenario) -> (추가) Edge case 추가(오류상황에 대한 story)
    - wire frame (GUI 및 action 등을 정의) => 초안을 AI로 만들 수 있음
        - tool: pigma

* Architecture 설계
    - (결과물) 프로젝트 구조에 관한 문서
    - (과정)
        - Frontend - Backend - DB 관점에서 설계
        - DB부터 설계
            - 데이터 저장 구조 설계: ERD (Entity Relationship Diagram)
        - Backend와 DB사이에 
        - Backend와 Frontend 사이에 통신구격
            - API: (Swagger/OpenAPI)
        - Frontend, Backend의 directory 구조 설계
            - Frontend: directory 구조
            - Backend: 각 계층(layer) 설계

* 구현
    - 앞서 생성된 설계문서를 토대로 AI에게 명령
    - (예제) "task#: OO, OO 문서를 이용해서 xxx을 개발해" -> prompt engineering

* Test
    - E2E(End-to-End) 시험
        - Frontend에서 제공하는 기능(@ browser)을 시험
        - Scenario 문서를 토대로 frontend 시험하고, 시험결과 만들고, 최종 보고서까지 AI가 생성
            - tool: Playwright/Cypress
            - 보안 취약점 검토: AI로 확인가능 (취약점 체크리스트, 확인 툴을 AI가 자동으로 실행)
            - Code refactoring 가능
* 배포
    - ...

* Testing tools
    - E2E 시험환경은 성공했으나 배포/운용 단계에서 오류발생
        - 배포/운용 단계에서 시험: Smoke Test
        - 배포/운용 후 E2E 시험을 진행

* SDD
    - 명세가 가장 중요
    - 명세 변경에 대한 이력을 남겨야 함
    - tool: spec kit
        - AI code에 기본적으로 탑재되어 외부 툴을 굳이 사용하지 않아도 됨

### Claude Code 사용법

* Markdown으로 명세서 작성
    - Viewer: stackedit.io
    - """ 사용법 """ => 다중라인 요구사항 정리
    - mermaid 활용
    - 수식: `$$ ... $$`으로 표현
```
    $$
    \Gamma(z) = \int_0^\infty t^{z-1}e^{-t} dt\,.
    $$
```

* 출력고정(Format Lock)
    - AI 결과의 형식을 고정하면 다른 AI 모델에 전달할때 효과적
    - tools
        - "n8n" 여러 AI 모델을 이용하여 work flow을 구성할 수 있음
        - Herdr (Herdr.dev)
            - 터미널 기반의 HERDR
            - 터미널 창 2개를 herdr가 관리 
            - term1: claude code, term2: codex AI로 동작시키면 Herdr가 term1의 결과를 term2으로 전달
            - 개발과 코드 검토를 가급적 다른 AI 모델로 수행
```
 > powershell -ExecutionPolicy Bypass -c "irm https://herdr.dev/install.ps1 | iex"
```
    - 
* Decomposition
    - 여러 task로 분해하는 기능은 AI가 알아서...
    - AI에게 task 분해결과를 요청 -> 개발자가 검토
    - 단계적 분해/출력은 이제 AI가 ...
    - (p.17)에서 보여주는 decomposition의 예제는 이제 AI가 알아서 함

* Testing
    - 결과물에 대한 반복적 시험과 feedback으로 검증
    - AI에게 주어진 검수 script를 refine (재사용 목적 but 모델변경에 따른 수정이 필요)
    - Loop 작업시 필요조건
        - 무한반복, 종료조건 등을 명시화
        - 결과 내용 정리도 요청
        - >> 완료/검수 기준
    - Cache 기능 활용: token 사용률을 극대화 -> 비용절감 효과
    - 검수 관련 tools들이 있음
    - 검수 프롬프트를 남겨야 함 (추후 재사용 목적) -> 프롬프트 개선도 필요
        - 프롬프트 개선시 이전 결과가 필요 -> github를 이용한 check-point를 이전으로 변경 필요
        - token 비용이 많이 소요
    - checklist 예시
```
 - [] check list1
 - [x] check list2
```



