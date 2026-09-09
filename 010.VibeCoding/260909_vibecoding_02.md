# DB 개발

* 교재: 06_백엔드 개발.pdf

## DB 준비
    - download: https://www.enterprisedb.com/downloads/postgres-postgresql-downloads
    - install: postgresql-17.11-3-windows-x64.exe
    - super-user ID/PW: postgres/postgres
    - server: PostgresSQL 

## DB 필요 정보
    - DB 연결 문자열: IP, port, ID/PW, DB이름
    - 환경변수로 설정한다. (예제: backend/.env)
```
    DB_CONN_STRING=postgresql://postgres:패스워드@localhost:5432/postgres
```
    - .env는 github에 올라가지 않도록 ".gitignore"에 등록

## DB 생성 및 조회

```
.env 파일의 환경설정 정보를 이용하여 postgresql-mcp 도구를 사용해 로컬 데이터베이스에 연결가능하지 시험해줘.
DB_CONN_STRING으로 통일해주고, DB 스키마 만들어줘
postgresql-mcp를 이용해서 생성된 DB와 table 목록을 조회해줘
```


# Backend 개발

## 준비

* Issue tracking system
    - 대표적인 tool: JIRA
    - 이슈 등록시 부가 정보(...) 추가 + backlog
    - github도 issue tracking 기능이 있음

## 실행

* Plan 문서의 task를 기준으로 issue 생성

* issue가 주어진 경우
    - "skill"을 만들어서 issue별 진행 절차를 기술한다. (p.13)
    - 각 issue별로 진행 내용과 결과를 로그화함

* issue가 없는 경우
    - plan.md(/docs/8-plan.md)의 DB/BE/FE의 아이템을 지정해서 실행하도록 한다.
    - 예시로 skill/develop-backend/SKILL.md 파일을 이용해서 BE-##을 실행

```
> /develop-backend BE-1
```
    - 여러개의 Backend Task를 동시에 병렬로 진행하려면
```
> /develop-backend BE-2~BE-8
```



