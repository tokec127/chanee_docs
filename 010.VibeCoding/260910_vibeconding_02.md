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

