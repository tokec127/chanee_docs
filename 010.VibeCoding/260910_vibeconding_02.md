# Frontend

> 26.09.10.(목)


## 용어

| Keywoard | 내용 |
|--|--|
| REST API | ... |
| 


## 화면 시안 개발

* wireframe
    - (방법2) 하위 fidelity 정의 + Figma
    - (방법2) style guide 활용
        - style guide 지정 필요 (head size, font, color, ...)
        - 비슷한 UI를 여러개 capture하여 claude code에 넣는다. (AlT-V)
        - http, css 파일들을 넣어주고 유사한 형태의 style을 요청
    - (방법3) 대표적 css framework을 사용
        - ex. TailwindCSS, bootstrap

* style guide 활용
    
```
❯ @docs/4-wireframes.md 문서의 3.할일 목록 화면의 데스크톱 화면을 스타일 가이드 문서의 내용을 결합해서 svg 이미지를 만들어줘
```
    - 이미지 로드 오류시
```
> [Image #2] 만들어진 이미지가 안보여. 수정해줘
```
    - /design: 여러 디지인 시안을 만들어주는 skill



## Frontend 개발

* Log @ Frontend
    - F12 @ browser => "개발자도구"
    - "Console 로그" 탭에 로그가 출력됨 (개발시에만 활용, 배포시에는 삭제)
    - 필요한 출력정보는 특별 URL로 전송하도록 고려
    - 아래 내용을 "skills/develop-fronted/SKILL.md" 에 추가
      `개발환경에서만 주요 지점에 대해서 콘솔 로깅하도록 로깅기능을 추가한다.`


* 하위 CLAUDE.md 지침
    - frontend에만 해당하는 지침
    - REACT를 사용하는 환경이라면, "03_Vibe_Coding을 위한 프롬프트 엔지니어링.pdf"의 문구 일부 적용

```
## DO: 반드시 준수할 것
- 중복된 기능의 코드가 발생되면 React Custom Hook으로 기능을 분리한다.
- 하나의 컴포넌트에 두가지 이상의 핵심 기능이 포함될 경우 컴포넌트를 반드시 분할하도록 한다.
```

* MCP 설정
    - playwright
        - 다양한 browser를 제어함
        - E2E 시험시, 사용자 시나리오의 UI 시험할때 효과적
        - E2E 시험은 기능 위주의 시험만 수행 (UI 시험은 사람이 수행해야 함)
    - chrome-devtools:
        - chrome browser만 가능
        - chrome browser의 개발자탭의 모든 정보를 접근할 수 있다.
            - 예) 개발자탭의 콘솔로그를 접근할 수 있기때문에 개발시 AI로 디버깅 가능
            - Logical error의 경우, backend/DB과의 연동에서 발생한 오류일 수 있음
            - 이 경우에는 개발자 메뉴-Network탭의 정보가 필요할 수 있음

```
> 디버깅할때 playwright MCP를 사용할 경우, Chrome-devtools를 사용해
```

* Calender
    - Opensource를 활용해서 Calender를 개발하면 배포시 license 문제가 발생할 수 있음
    - 만일 다른 기술 스택을 사용할 경우, 개발자에게 물어보도록 하위 CLAUDE.md에 추가

* 환경변수
    - 

* frontend 개발
    - 실행
```
/develop-frontend FE-1~FE-8
```

```
> 프런트엔드 앱을 /tasks로 관리되도록, 백그라운드에서 npm run dev로 실행해줘
```

    - localhost:5173/todos


* 다국어 처리

* Backend/Frontend/DB
    - framework: library, component 등으로 구성된 
    - backend
        - 전자정부: Java spring/spring boot
        - 실습: Express/NestJS (배포환경 고려)

* API
    - RESTful API
        - API 정의하는 형식이 swagger
    - GraphQL
    - SOAP


* 3-tier 앱
    - 단일 애플리케이션: 통신 필요없이 바로 실행가능한 앱 (GUI + Logic + DATA)
        - 사용할 대상에 설치가 필요
        - 변경시 패치가 필요
    - 2-tier App.
        - 서버 + 클라이언트(GUI + 로직)
        - 클라이언트 수정사항이 있다면 역시 패치가 필요
    - 3-tier App.
        - GUI
        - 

* frontend 배포
    1. 아래처럼 빌드한다.
    2. 빌드결과물을 frontend 호스팅 서버에 올려둔다.
    3. 사용자는 frontend 호스팅 서버에 올린 빌드결과를 실행한다.

```
 > npm run build

  Ran 1 shell command

빌드 성공 — tsc -b 타입 오류 0, Vite 프로덕션 빌드 227ms.

┌─────────────────────────┬───────────┬───────────┐
│         산출물          │   크기    │   gzip    │
├─────────────────────────┼───────────┼───────────┤
│ dist/index.html         │ 0.45 kB   │ 0.29 kB   │
├─────────────────────────┼───────────┼───────────┤
│ dist/assets/index-*.css │ 18.52 kB  │ 3.94 kB   │
├─────────────────────────┼───────────┼───────────┤
│ dist/assets/index-*.js  │ 328.41 kB │ 100.58 kB │
└─────────────────────────┴───────────┴───────────┘

모듈 150개 변환, 경고 없음. JS 100 kB(gzip)는 React 19 + TanStack Query + Router + Zustand를 포함한 값이고, 다국어 사전(3개 언어)과 테마 토큰도 함께 들어 있습니다.
```


* "애플리케이션 아키텍처 소개"
    - p.6
        - 읽기에는 "cache" 서버를 주로 둔다.
        - 쓰기에는 비동기 처리를 위해 "Message queue"를 둔다.
        - DB는 Master/Slave 방식으로 운용하고, 쓰기는 Master에서만 수행한다. Master에서 쓰기 수행하면 자동으로 Slave에 반영한다.
        - 프론트엔드/백엔드 서비스 앞단에 LB(Load Balancer)를 두어 부하를 분산한다.



* JWT 인증흐름
    - ID/PW로 사용자 인증방식
        - ID/PW로 인증된 사용자에 임시 Secret token을 발행
        - JWT(Jason Web Token)을 secret으로 사용하는데 Base-64로 인코딩 (binary을 text형태로 변환하는 알고리즘)


* CORS: Cross-Origin Resource Sharing
    - 웹 브라우저에서 실행되는 Frontend가 다른 출처(origin)의 Backend API에 접근할 수 있는지를 서버가 허용하는 보안 메커니즘
    - ORIGIN: 사용자 화면에서 보여주는 HTML 페이지를 제공하는 서버의 정보 (IP, Port)  
    - Cross-Origin:
    - Same-Origin:
    - CORS
        - Cross-Origin에서는 Origin이 다르면 통신 차단
        - 서비스를 제공하기 위해 backend가 허용해줘야 함
        - backend에 허용할 orgin을 등록해줘야 함 (@ backend/.env에 CORS_ORIGIN)


* Social Auth
    - SNS 인증으로 사용자 인증을 대체
