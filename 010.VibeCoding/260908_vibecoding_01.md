# Vibe Coding을 위한 개발도구

## 관련 tools

* tools
    - ponytail: claude code의 출력 토큰 사용량을 극대화 (비용절감 효과)
        - "/ponytail full"
    - caveman: ponytail과 유사한 기능 제공
        - ""
        - "npx skills add JuliusBrussee/caveman" 실행하여 설치
            - "caveman", "caveman-explore" 설치
            - Claude Code용 설치
            - global 설정
    - claude code 사용량 등의 상태정보 출력 @ prompt (p.47)
        - https://github.com/sirmalloc/ccstatusline
        - prompt 아래에 claude 사용량 상태표시
        - "npx ccstatusline@latest" 로 설치
        - command창에서 "npx ccstatusline"으로 실행하여 설정

## Claude Code

```
 $ claude [options] [command] [prompt]
```

* options
    - "--dangerously-skip-permission": 권한체크를 skip (하위폴더도 동일)
    - "--append-system-prompt <prompt>": 입력한 프롬프트를 시스템 프롬프트에 추가 (우선순위: system prompt > user context)
    - "--permission-mode <mode>": 현 세션에 대한 권한 모드 설정, mode="acceptEdits", "bypassPermissions", "defaul", "plan"
    - "--resume" or "-r": 기존 세션 이어서 작업시

```
 $ claude --dangerously-skip-permissions
```

* keywords
    - session: 작업이 진행되는 세션 (AI와 작업한 내용들이 내부폴더에 저장됨)
    - context: 
        - AI가 작업할때 관련된 자료들을 저장한 공간
        - 불필요한 자료들을 정리할 필요가 있음
        - "ccstatusline"에서 "Ctx Used"로 표시됨 (50% 넘지않도록 관리)
    - compact: context를 정리하는 기능
        - auto compact: context가 약 90% 차지하면 AI가 자동으로 compact 실행
        - compact는 "추론" 비용이 발생
    - clear: 모든 context를 삭제
        - Data center에서는 context를 유지하는 시간은 대략 1시간으로 알려짐
        - 1시간 이내의 작업을 이어서 할때는 "/resume"
        - 1시간 이후의 작업을 이어서 할때는 그전에 "/clear"하는것이 효율적
        - Cache가 삭제됨
        - 재시작할때 context를 cache화하는데 token이 소비됨

* Claude 사용시 권장
    - [권장1] 중요작업에 대한 내용은 "md" 파일로 저장해라.
        - md 파일을 사용자가 수정한 후에는 claude에게 알려줘라.
        - claude는 해당 파일을 load하여 context에 반영
    - [권장2] Context 사용량이 50%이상이 되면 "/compact"로 정리해라
    - [권장3] 1시간 이상 pause할 경우에는 "/clear"해라

* 

* 주요 slash (session, auth, ...)
    - "/help": 
    - "/clear": context를 비우고 정리
    - "/quit": claude code를 종료
    - "/resume": 기존 세션을 이어서 시작
    - "/recap": 현재 세션의 대화를 읽고 한줄의 요약을 생성함 (요약 필요시 실행)
        - 토큰/context를 절약하려면 auto로 동작시키지 말자
    - "/login", "/logout":
    - "terminal-setup"

* 주요 slash 명령어 (mode, model)
    - "/model": "sonet"
    - "/effort": medium
    - "/permissions":
        - "manual", "accept edit", "auto", "bypass permissions"    
        - "Allow" 탭에서 도구/skill 등의 사용을 allow
            - WebFetch: 웹에서 내용을 fetch
            - Edit: 문서 수정
            - Delete: 파일 삭제
    - "/status":
    - "/config":

* 주요 slash 명령어 (coding)
    - git 도구와 함께 사용해야 함 (commit, push, diff, branch, merge, pull request, ...)
    - "/review": 현재 세션에서 PR(Pull Request)을 검토 
        - default: last commit과 비교한 코드 검토
        - 전체 코드 리뷰도 가능
        - PR: sub branch에서 main branch에 pull 요청 (@git)
            - merge되면 자동으로 배포하도록 system화
        - 이 과정을 AI가 자동화함
    - "/security-review"
    - "/diff"
    - "/simplify": 병렬로 3개의 agents를 실행해서 코드재사용, 품질, 효율성 관점에서 분석 및 정리
    - "/install-github-app"

* 주요 slash 명령어 (utils)
    - "/statusline": ccstatusline을 사용하자.
    - "/upgrade"
    - "/doctor"
    - "/fast": 더 많은 토큰비용(30%)을 소비해서라도 빠른 응답을 얻음
    - "/export": AI와의 chat을 백업하는 기능


* 주요 slash 명령어 (context)
    - "/rewind": 
        - 특정 이전 시점으로 대화/결과물을 되돌림 (checkpoint로 이동)
        - claude code를 종료하면 해당 checkpoint로 이동못함
        - 대신해서 git commit을 사용
    - "/branch"
        - git의 branch와 유사
    - "/compact"
    - "/context"
        - 관련된 context 사용량과 각종 tools등을 보여줌
        - MCP tools은 실행시 로딩됨
        - context 사용량을 자동으로 표시해주는 "ccstatusline"으로 확인가능
    - "/plan"
        - shift + tap을 이용해서 Plan mode로 이동
        - 실행은 하지않고 계획만...
        - 결과물을 markdown 형태로 남겨라. 
        - /compact에 의해 내용이 사라질수 있음

* 주요 slash 명령어 (mem,directory)
    - "/init"
        - "CLAUDE.md" 만들어짐
        - @CLAUDE.md 파일에 "최상위 지침"을 추가(ex. 한국어로 입력/출력)
        - 기존 project를 이용해서 AI에게 이해시키기 위한 명령으로 "CLAUDE.md" 파일을 참조
    - "/memory"
        - "Project instructions": 프로젝트 관련 지침
            - CLAUDE.md는 프로젝트별로 다를수 있음
        - "User instructions": %USERPROFILE%/.claude"에 생성되는 사용자용 지침
        - "auto-memory folder": 
            - %USERPOFILE%/.claude/projects/<project>/memory에 AI와 chat한 내용이 MEMORY.md에 저장
            - 여러개의 파일(jsonl)이 있을 수 있다. 
    - "/add-dir"
        - 작업폴더를 manual로 추가

* 주요 slash 명령어 (tools)
    - "/mcp"
    - "/skills"
    - "/agents"

* 수준별 CLAUDE.md
    - 지침별 우선순위
        - System prompt > 프로젝트 수준의 CLAUDE.md > 사용자 수준의 CLAUDE.md (%USERPROFILE/.claude/CLAUDE.md)
    - CLAUDE.md 기본 추가 지침
        - "한글로 입출력"
        - "overengineering 금지"
        - "중요작업 규칙"
            - "DO", "DO NOT"
        - "작업 전 체크리스트"

* 주요 slash 명령어
    - 사용자 정의 지침(지시) 작성하여 실행하는 방법
    - slash 명령어 대신 skill로 변화하는 추세 (/reload-skills 명령으로 skill처럼 사용)
    - 지시(지침)를 미리 markdown으로 만들어서 slash 명령어로 사용
        - 예) ./claude/commands/create-test.md
    - "skill" 형태로 작성하여 사용하기를 권장
```
    > /reload-skills
    > create-test.md @src/tododao.js": "create-test.md"에 기술된 지침을 "src/tododao.js"에 적용하여 test
```


## SubAgent

* SubAgent
    - 특정유형의 작업을 처리하는 Claude Code의 전문 AI agent (cf. skill, mcp)
    - 기본적으로 "main agent"가 prompt를 처리
    - **spwan**: "개발자 - Main Agent" 구조에서 "Main Agent" 밑에 SubAgent(1M tokens)들에게 미션을 부여
    - Main Agent는 SubAgent로부터 결과를 Reporting 받음
        - Main Agent의 context가 오염되지 않음: Main Agent의 context는 compact 상태를 유지할 수 있음
        - 병렬 실행이 가능
    - 3개의 default agent가 존재: 범용 Agent, Explorer, Plan 
        - "/agent" 명령으로 subagent를 spawn할 수 있음 (현재는 지원하지 않음)
        - claude code에는 agent 수를 제한하지 않음 (codex에는 기본 6개)
        - 작성 위치
            - [프로젝트 수준]: $project/.claude/agents/
            - [사용자 수준]: ~/.claude/agents/
        - subagent의 기능은 생성된 agents 폴더안에서 .md로 작성
            - VScode에서 agents 폴더 생성
            - 너무 상세한 지침을 작성하지 마라.
            - subagent에서 subagent를 spawn하지 말라.
            - 해당 task를 subagent에게 명시적으로 실행하라 라고 main agent에게 요청할 수 있음
            - subagent마다 model을 설정할 수 있다. (when: spawning subagent, who: user)
                - 업무 종류에 따라 낮은 사양의 model을 지정하여 비용절감을 할 수 있다.
                - 기획/설계: opus
                - 코딩: sonnet
                - 문서: haiku
            - subagent의 실행상태를 2줄씩 보여줌 (색상을 지정하여 구분할 수 있도록 함)
            - agent team을 구성하려면 "MAX" 요금제에서만 가능
```
  Main Agent/
    \_ SubAgent#1 ..... SubAgent#n
       (1M tokens) .... (1M tokens)
```


## MCP(Model Context Protocol)

* MCP
    - SQL에 대한 기초 지식이 필요
    - MCP 없는 상황에서 DB 활용
    - Vibecoding을 통해 MCP 서버들을 만들 수 있으며, 다른 AI와도 연동이 가능
    - 다양한 도구들을 포함하고 있어 메모리 사용량이 크다.
    - 도구들의 메모리 적재는 실제 사용될때 실행
    - 프로젝트별로 필요한 MCP를 사용하도록 설정
    - MCP

```
                 (AI 결과로 요청)
    (( 사용자 )) ------------ ((DB client tool)) ------------- (( DB ))
        \
         \
          \____ (( DB 조회해줘 @ AI Browser)) ------ (( AI Data Center))

```

    - MCP 이용할 경우
        - 사용자가 자연어로 DB 조회, 관리가 가능
```
                                              2.검색명령
    (( 사용자 )) ------------ ((Claude Code)) ---------- ((AI model))
                 1.oo 조회해줘        |
                                      | 3.조회 요청/응답
                                      |
                              (( MCP 서버/도구 installed @ CC)) ---- ((DB))

```


* 자주 사용하는 MCP들
    - Context7
    - Postgresql
    - Supabase
    - markitdown mcp: 기존 문서(doc,hwp,...)을 markdown형태로 추출
    - Playwright MCP: E2E 시험용 MCP
    - Chrome Devtools MCP: Frontend 디버깅 툴(F12로 얻을 수 있는 정보를 추출하여 디버깅)
    - 결제시스템 API 활용하는 MCP
        - Toss payments MCP
        - PortOne MCP
    - github (term에서 "!명령"을 통해 수행할 수 있음)
    - vercel (배포시 오류나면 build.log를 분석해서 스스로 해결)
    - kubernetes
    - sast-mcp (고급 AI로 보안취약점을 검색하는 것이 더 효과적)
    - frontend 디자인 설계용 MCP
        - shadcn MCP
        - Webpixels : Bootstrap5 컴포넌트 기반의 디자인 툴
        - Figma: figma wireframe을 연동하여 디자인 기능 제공

* 주의사항
    - 로컬에서 동작하는 MCP를 사용해라
    - MCP 해당 문서와 코드를 AI에게 "보안취약점" 분석 요청 후 사용
    - MCP 설정 파일에 PW나 key를 넣지 말아라.
    - 가급적 사용자 평가가 좋고 설치수가 많은것을 사용해라

* 사용방법

    - 사용자 수준에 등록하기보단 프로젝트 수준의 .mcp.json 설정파일을 만들어서 MCP 도구들을 추가
    - 아래 설정파일은 claude code를 실행될때 실행됨
```
{
    "mcpServers": {
        "context7": {
            "command": "npx",
            "args": [ "-y", "@upstash/context7-mcp" ]
        },
        "playwright": {
            "command": "npx",
            "args": [ "-y","@playwright/mcp@latest" ]
        },
    }
}
```
    - 위에서 기술한 것을 term에서 실행하려면,
```
 $ npx -y @upstash/context7-mcp
```
    - CC를 reload하면 자동으로 .mcp.json을 실행
    - 예제 
        - "프롬프트에서 playwright mcp 도구를 이용해 OOO을 검색해줘" 실행하면,
        - 해당 OOO을 검색한 결과를 browser에서 보여줌



## CLAUDE.md

* CLAUDE.md
    - 안에 complexity가 높으면 AI가 지침을 무시할 수 있음 (강제사항은 아님)
    - 내용이 많을 경우, 쪼개어 저장하고, CLAUDE.md에 link를 넣어라

* Claude Code Hook
    - CC의 lifecycle 과정중 특정이벤트(명령실행, 완료, 실패, 입/출력 등) 발생시 자동실행되는 사용자 정의 script 추가 가능
    - (목적)
        - 특정이벤트 발생시 강제로 작업을 실행할 수 있다.
        - 트리거 기반 자동화 수행
    - (예시)
        - 작업이 완료되었을때 "소리,문자"등으로 알림을 발생
        - 작업완료시 결과문서를 특정 문서형태로 생산
        - 코드 완료시 단위시험을 자동실행

* 사용가능한 이벤트
    - PreToolUse: 도구 호출전에 실행 (보안관점에서 "조건"을 보고 차단 기능)
    - PostToolUse: 도구 호출이 완료된 후에 실행 (코드완료시 단위시험 자동실행)
    - UserPromptSubmit: 사용자가 입력후 "enter"를 입력했을때
    - ...

* matcher
    - PreToolUse, PostToolUse 이벤트에만 matcher로 지정한 도구 실행


## Skill

* Skill
    - 특정기능, 작업을 수행하는 모듈화된 기능
    - slash 명령을 markdown(skill.md)으로 저장해서 사용하는 것 + script (java,python,bash,...)
        - node.js와 python을 기본으로 사용
    - token 비용절감, 일관된 결과물을 얻을 수 있는 장점이 있음
    - 단위 기능을 skill로 만들어 여러개의 skill의 조합으로 work flow를 만들 수 있음
    - open된 skill도 매우 많다
    - open skill등을 사용하려면
        - 보안 취약점이 있는지 확인한 후에 사용

* 형식
    - 주요 frontmatter
        - agent: 사용할 subagent 지정
        - context: 격리된 context에서 실행하도록 함
        - disable-model-invocation: "true"일때, CC가 자동으로 스킬을 자동선택하지 않도록 설정
    - 사용시 skill과 subagent를 명시화하여 요구

```
---
 name: $name
 description: ....
 context: ....
 agent: ...
 allowed-tools: Bash(gh *)
 disable-model-invocation: true
---

## Pull reuest context
- PR diff: !`gh pr diff`
```

* 사용법

```
 > /$(skill 이름) $arguments
```

* Claude 공식 skill: Anthropic Skills
    - Doc.:
    - Ex
    - skill-creator:

```
❯ /plugin marketplace add anthropics/skills
  ⎿  Successfully added marketplace: anthropic-agent-skills

❯ /plugin install document-skills@anthropic-agent-skills

❯ /plugin install document-skills@anthropic-agent-skills
  ⎿  ✓ Installed document-skills. Plugin is now active.

❯ /plugin install example-skills@anthropic-agent-skills
  ⎿  ✓ Installed example-skills. Plugin is now active.
```



## Plan Mode

* plan mode
    - 중요한 내용은 md 파일로 저장이 필요
    - 복잡한 task는 AI가 작은 단위의 task으로 쪼개어 설계
    - 작업 계획을 수립 (실행까지 안함)
    - 단점
        - 직접 수정 못함(md 파일로 저장한 후 수정)
        - 세션 종료시 plan은 사라짐

* 계층화된 지침
    - CLAUDE.md를 단순화, 명료화, 계층화(아래 예시)
    - 프로젝트별로 존재

```
    ./CLAUDE.md
        \_ /frontend/CLAUDE.md: frontend에 관련된 지침만 기술
        \_ /backend/CLAUDE.md: backend에 관련된 지침만 기술

    ./CLAUDE.local.md
        - 프로젝트 수준에서 개발자 자신의 지침을 추가
```


## Plugin

* Plugin
    - Plugin = "MCP + Skill + Hook"으로 구성된 툴 + 설치 script
    - 로컬에 설치할 수 있고, 배포도 가능
    - Anthropic open skills: https://github:com/anthropics/skills
    - Open plugins: http://claudecodemarketplace.com
    - 설치방법
```
 > /plugin marketplace add $(plugin_name)
 > /plugin install $(plugin_name)@$(...)
```


## 규칙(rules)

* rules
    - 규칙(path명에 위치한 `*.ts`)에 부합될때만 지침을 수행




