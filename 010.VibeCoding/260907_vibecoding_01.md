# 교육

* 강사
 - 원형섭 (stepanowon@ssamz.com)

* 개요
  - 관련분야
    - 클라우드: AWS, GCP
    - frontend: React, Vue(js의 한분야???)
    - OpenAPI, OAuth 2.0, OpenID
    - NOSQL (MongoDB, DynamoDB ..)
    - Docker, Kubernetes
    - DevOps, MSA
    - Augmented Developing

* VibeCoding
    - 기획, 설계가 90%
    - 남은 코딩은 AI
    - 코드검토
    - AI 성능 >> 시스템 prompt tool의 성능 (ex. claud code의 harness) 
    - claud code를 term에서 실행하는 이유
        - GUI 사용필요 없음
        - term에서 실행도 가능함, 지원 tool도 좋음



## 1.환경설정

* node.js
    - claud code가 node.js로 만들어짐 (최초)
    - NVM
        - node.js의 버전 업데이트가 매우 빠름
        - NVM을 이용해서 버전을 선택할 수 있음
        - nvm-2.0.1-hotfix.1-amd64-setup.exe

```
 $ nvm list
 $ nvm install 24    # 24는 nvm의 버전정보
 $ nvm use 22
 $ nvm list

 * 24.20.0 (Currently using 64-bit executable)
   22.23.2 

 $ npm
```

* python
    - MCP, Skill 등이 python으로 작성됨
    - python 3.14.7 (64-bit)
        - 설치 마지믹에 PATH 길이 제한을 disable

```
 $ pip install uv
```

* github 계정 만들기
    - AI가 잘못만든 결과물을 수정 => 마지막 이전단계로 이동(checkpoint)하여 다시 시작
    - git으로 checkpoint 설정/이동


* Claude Code
    - install "claude code"
```
 $ irm https://claude.ai/install.ps1 | iex  << claude code 설치
```
    - set ENV_PATH
```
    PATH=%USERPROFILE%\.local\bin:$PATH
```


* 추가 AI code 설치
```
 $ npm install -g opencode-ai
```

* Visual studio code 설치

 > https://code.visualstudio.com/download

* Extension tools @ Visual Studio Code
   - ESLint: 불필요한 코드 삭제 (
   - Prettier: 코드를 가독성있게 변환해주는 툴


## Claude 설치


```
PS C:\Users\student> claude
Welcome to Claude Code v2.1.263
..........................................................

     *                                       █████▓▓░
                                 *         ███▓░     ░░
            ░░░░░░                        ███▓░
    ░░░   ░░░░░░░░░░                      ███▓░
   ░░░░░░░░░░░░░░░░░░░    *                ██▓░░      ▓
                                             ░▓▓███▓▓░
 *                                 ░░░░
                                 ░░░░░░░░
                               ░░░░░░░░░░░░░░░░
       █████████                                        *
      ██▄█████▄██                        *
       █████████      *
.......█ █   █ █..........................................

 Logged in as to.eckim@gmail.com
 Login successful. Press Enter to continue…
 ```


```
──────────────────────────────────────────────────────────────────────────────────────────
  Settings  Status   Config   Usage   Stats

    Version:          2.1.263
      Session name:     /rename to add a name
        Session ID:       58bb2ac6-f1c2-445d-858f-8fd57603ce72
          Session kind:     interactive
            Peer address:     uds:\\.\pipe\LOCAL\cc-msg-5caa0db8bd547fd2cd5476e6fddd5b3a
              cwd:              C:\Users\student
                Login method:     Claude Team account
                  Organization:     Multicampus IT
                    Email:            to.eckim@gmail.com

                      Model:            Default (Opus 5 with 1M context · Best for everyday, complex tasks)
                        MCP servers:      6 need auth · /mcp
                          Setting sources:  User settings, Shared project settings

                            Esc to cancel
```


* Superbase 설치
    - 대표적 BaaS (Back-as-a-Service)
    - 지원기능: 인증, 스토리지, API지원, Edge Function, ...
    - 여기서는 DB용으로만 사용("PostgreSQL")
    - 회사에서는 직접 Backend Engine을 만들어야 함(보안이슈)
    - "Frontend - Backend - DB" 구조에서는 Frontend는 Backend 정보만 가지고 있어야 함. 
        - (DB 관련 정보를 가지면 안됨)
    - > new organization >>

* Vercel 설치
    - vercel.com
    - 무료로 Frontend를 배포할 수 있다. (Plan: hobby mode)
    - Backend도 배포가능 (제한적범위)
    - java 기반의 backend 배포는 안됨 (railway 사용 - 유료)



