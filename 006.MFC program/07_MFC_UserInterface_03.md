## User Interface - Status Bar

* MFC의 상태바 (Status Bar)
	* 윈도우 하단에 표시되는 상태 정보
	* 메뉴항목, 마우스 위치에 대한 정보등을 출력

* 고정크기 PANE
	* 종류: CAP, NUM, SCRL(Scroll Lock)
	* 윈도우 하단 우측에 표시됨


* 상태바 작성방법
	* CStatusBar 클래스를 사용
	* 순서
		1. CStatusBar 객체 만들기
		2. CStatusBar::Create() 함수로 상태표시줄 생성
		3. Pane을 만들고, 크기를 설정
	* Application Wizard의 상태바
		* CMainFrame 클래스 -> CStatusBar 클래스 -> m_wndStatusBar 멤버변수
	* MainFrm.cpp에 아래 배열이 생성됨
		* ID_SEPERATOR
		* ID_INDICATIOR_CAPS
		* ID_INDICATIOR_NUM
		* ID_INDICATIOR_SCRL


* Lab
	* Resource View 창에서 프로젝트 우클릭 -> Resource Symbols
	* ID_xxx 새로운 PANE 추가
	* PANE에 새로운 값 설정하기 위해, 
		* CMainFrame 클래스에 indicator에 새로운 ID (ex. ID_POINT) 추가
		* CMainFrame 클래스 생성자 OnCreate()에 초기화 추가
```c
	static UNIT indicators[] = 
	{
		ID_SEPARATOR,
		ID_POINT,				// <- 새로 추가한 ID
		...
	}

	int CMainFrame::OnCreate(...)
	{
		...	
		m_wndStatusBar.SetPaneInfo(1, ID_POINT, SBPS_NORMAL, 140);
		...
	}
```
	* Message map 설정
		* Cxx_View 클래스 -> Message -> WM_MOUSEMOVE
	* 메시지 핸들러 소스 추가
```c
	void Cxxx_View::OnMouseMove (UNIT nFlags, CPoint point)
	{
	
		CString str;
		CMainFrame *pF;
		pF = (CMainFrame *)AfxGetApp()->m_pMainWnd;

		CStatusBar *pStatus = &pF->m_wndStatusBar;
		if (pStatus) {
			str.Format(_T("마우스 위치 x:%d, y:%d"), point.x, point.y);
			pStatus->SetPaneText(1, str);
		}

		CView::OnMouseMove(nFlags, point);
	}
```
```
	+---------------+--------------------------------------------------------
	|	함수        | 설명
	+---------------+--------------------------------------------------------
	| AfxGetApp()   | - 응용 프로그램의 여러요소에 직접 접근할 수 있는 함수
	|               | - AfxGetApp()는 CWinApp 객체의 포인터를 반환
	+---------------+--------------------------------------------------------
	| SetPaneText() | - CStatusBar 클래스의 멤버함수
	|               | - 상태바의 특정 Pane에 문자열 출력
	|               | - 1st arg: Pane의 위치, 2nd arg: 출력할 문자열
	+---------------+--------------------------------------------------------
```
	* CView 클래스에서 SetPaneText() 함수호출 방법
```c
	ex1) pStatus->SetPaneText(...);
	ex2) CstatusBar *pStatus = & pFrame->m_WndStatusBar;
	ex3) CMainFrame *pFrame = (CMainFrame *) AfxGetApp()->m_pMainWnd;
```

