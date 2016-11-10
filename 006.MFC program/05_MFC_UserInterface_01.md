## User Interface - Menu Bar

* Menu
	* 윈도우 상단 보여지는 명령들의 집합 메뉴
	* 주메뉴(main)와 부메뉴(sub)로 나뉘다
	* 예)

```
	+------+------+------+-------+------+-------+------+-----+------+
	| FILE | EDIT | VIEW | DEBUG | TEAM | TOOLS | TEST | ... | HELP |
	+------+------+------+-------+------+-------+------+-----+------+
```

* 메뉴의 종류
	* Pull-down Menu : 사용자가 메뉴를 선택하면 하위 메뉴가 drop down되는 메뉴
	* Cascading Menu : 메뉴의 오른쪽(또는 왼쪽)으로 메뉴가 나오는 경우
	* Pop-up Menu (or Context Menu) : 우클릭 또는 이벤트에 의해 발생하는 메뉴


* MFC의 메뉴작성
	* Application Wisard를 이용해 윈도우 생성
	* MFC가 제공하는 기본메뉴 위에 프로그래머가 원하는 메뉴 등록
	* Resource View의 [Menu] 폴더에 IDR_MAINFRAME을 선택
	* SDI/MDI 프로젝트에 따라 다름

* 메뉴의 속성
	* ID : 메뉴항목의 식별자
	* Prompt : 메뉴의 상태바에 나타나는 문자열
	* Seperator : 메뉴 항목들을 분리하는 구분선
	* Break
		* None : 분리선이 표시되지 않음
		* Column : 부 메뉴 칼럼으로 표시
		* Bar : 칼럼과 유사, 부메뉴의 경우 컬럼 사이에 분리선을 표시
	* Caption : 메뉴에 표시되는 문자열
	* Checked : 메뉴 앞에 체크 표시를 함
	* Enabled : 메뉴 항목을 선택가능하도록 설정
	* Grayed  : 메뉴 항목이 회색으로 표시. 메뉴를 비활성화
	* Pop-up  : 메뉴의 하위 항목이 포함되도록 설정

* Lab
	* 주메뉴에 [색상] 메뉴에 Pull-down 메뉴 등록하기
	* Pull-down에 색상을 두고 선택시 윈도우 색을 변경하기
	* (1) View 클래스에 멤버변수 추가 (m_color, COLORREF)
	* (2) View 생성자에 멤버변수 초기화 추가
	* (3) View 클래스의 OnDraw()함수에 기능 추가
		* CRect 클래스 : 사각형을 나타내는 클래스. 
		* CRect는 RECT클래스를 상속
		* 클래스 객체를 통해 left, top, bottom, right 멤버 변수에 접근 가능
	* DrawText() :
		* -1 : 문자열이 null로 끝나야 함
		* r : 서식화되는 텍스트 좌표


```c
	CxxView::CxxView()
	{
		m_color = RGB(0, 0, 0);		// default BLACK
	}

	CxxView::OnDraw()
	{
		...
		CRect r;
		GetClientRect(r);
		pDC->SetTextColor(m_color);
		pDC->DrawText(_T("무슨색"), -1, r, DT_SINGLELINE | DT_CENTER | DT_VCENTER);
	}


	typedef struct tagRECT {
		LONG left;
		LONG top;
		LONG right;
		LONG bottom;
	} RECT;
```

* 메뉴 추가하기
	1. Open [Resource View]		// VIEW -> Resource View (Ctrl+Shift+E)
	2. Select [Menu] @ Resource View
	3. IDR_MAINFRAME <- Menu
	4. "색상(&C)" : 새로운 메뉴 추가하기
	5. "검정색(&C)" : 새로운 하위메뉴 추가하기
		* 검정색 메뉴의 속성를 설정한다. (ID, Prompt, Caption)
		* 여러색에 대한 하위메뉴를 추가한다.
	6. 메뉴 선택에 대한 핸들러를 등록한다.
		* Project -> Class Wisard
			* Class이름을 CxxxView 선택
			* 명령(Command)에서 ID_xxx 을 더블클릭하여 메시지에 OnBlack추가

```c
	void CxxView::OnBlack()
	{
		m_color = RGB(0, 0, 0);
		Invalidate();
	}

	void CxxView::OnRed() { ... }
	void CxxView::OnBlue() { ... }
	void CxxView::OnGreen() { ... }
```
